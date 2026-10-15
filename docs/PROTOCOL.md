# ch57x (k884x) protocol notes

How `hotkeyconfig` talks to the keyboard, written down so you do not have to read the code to
understand it (or to port it somewhere else). The message format is ported byte for byte from
[`kriomant/ch57x-keyboard-tool`](https://github.com/kriomant/ch57x-keyboard-tool); what is original
here is the **transport**: plain Windows HID instead of libusb, so no Zadig and no driver surgery.

Everything below is implemented in [`hotkeyconfig/protocol.py`](../hotkeyconfig/protocol.py)
(building the messages) and [`hotkeyconfig/hid_flasher.py`](../hotkeyconfig/hid_flasher.py)
(sending them).

## The device

| | |
|---|---|
| USB id | `1189:8842` (vendor `0x1189`, product `0x8842`) |
| Family | ch57x, also called **k884x** |
| Typical layout | 12 keys + 2 knobs + 3 layers |

The macropad exposes **two HID interfaces**:

- **`MI_00`**, usage page `0xFF00`: the **programming** interface. This is the one we open.
- **`MI_01`**: the regular keyboard interface that types. Never touched.

Writing to `MI_00` is what the proprietary tool does too, so this is the intended channel, not a
hack.

## HID framing (the part that bites)

Every ch57x message is **64 bytes**, padded with zeros, and starts with `0x03`.

Reading the HID capabilities of the device shows `OutputReportByteLength = 65` and no feature
reports. That extra byte is the report id, and here is the catch:

> The leading `0x03` of each ch57x message **is the report id**, not payload.

So the write is:

```python
report = message_64_bytes + b"\x00"   # 65 bytes total
```

Do **not** prepend a `0x00` report id to the 64 byte message. Doing that shifts everything by one
byte and Windows rejects the write with "the parameter is incorrect" (`0x57`).

For the same reason the all zeros **init message is skipped** when sending over HID: a report id of
`0x00` is not valid for this device.

## Binding a key

Each binding is four messages: the payload, then a fixed three message tail.

```
key combo:  [0x03, 0xFE, key_id, layer+1, 0x01, 0,0,0,0,0, 1, modifiers, hid_code]
native media: [0x03, 0xFE, key_id, layer+1, 0x02, 0,0,0,0,0, 0, low, high, 0,0,0,0]
tail:       [0x03, 0xAA, 0xAA]
            [0x03, 0xFD, 0xFE, 0xFF]
            [0x03, 0xAA, 0xAA]
```

`layer` is zero based in the profile and sent as `layer + 1`.

### Key ids

| Control | `key_id` |
|---|---|
| Buttons | `1` to `12` (button index + 1) |
| Knob `k` (0 based), action `a` | `16 + 3k + a`, where `a` is ccw=0, press=1, cw=2 |
| 4th knob on a 12 button board | `13 + a` (a quirk of ch57x: it reuses ids 13 to 15) |

### Modifiers and key codes

Modifier byte is a bit field: `ctrl = 1`, `shift = 2`, `alt = 4`, `win = 8`.

Key codes are standard HID usage ids. The ones that matter here are the phantom function keys,
`F13 = 0x68` through `F24 = 0x73`, sequential.

### Native media codes

Sent as a little endian `u16` when the action is written into the hardware (works with the app
closed):

| Key | Code |
|---|---|
| `next` | `0xB5` |
| `previous` | `0xB6` |
| `stop` | `0xB7` |
| `play_pause` | `0xCD` |
| `mute` | `0xE2` |
| `volume_up` | `0xE9` |
| `volume_down` | `0xEA` |

## RGB lighting

Two messages per layer that has lighting configured:

```
[0x03, 0xFE, 0xB0, layer+1, 0x08, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x00, code]
[0x03, 0xFD, 0xFE, 0xFF]
```

Note the `code` byte is the **last** one of the first message, not the tenth. Putting it
anywhere else makes the keyboard do something unrelated (a single key lighting up in the
wrong color, for instance) instead of failing loudly.

The `code` byte packs mode and color:

```
code = (color << 4) | mode
```

- Modes: `off = 0`, `backlight = 1`, `shock = 2`, `shock2 = 3`, `press = 4`.
- Colors: `white = 0`, `red`, `orange`, `yellow`, `green`, `cyan`, `blue`, `purple = 7`.
- Special case: **backlight + white** is `5`, not `(0 << 4) | 1`.
- `off` is always `0`, whatever the color.

## Per-key colours: what is and is not possible

Short answer for the k884x: **you cannot give each key its own colour**. The message above
carries a single code byte for the whole layer, and there is no field anywhere for a key
number. This is not a limitation of this project: the vendor's own Windows software only
sends mode plus colour per layer, as shown by the USB captures in
[issue #160](https://github.com/kriomant/ch57x-keyboard-tool/issues/160) upstream.

It is easy to assume otherwise, because the `press` mode does light up **only the key you
pressed**. But that decision is made by the keyboard's firmware, not by you: you send one
colour and the firmware picks which LED to light. You never send a key number.

Per-key RGB does exist in this family, on the **K8850** (`514c:8850`, 16 keys and 3 knobs),
which uses a completely different LED packet with 3 bytes of true RGB for every key:

```
[0x03, 0xFE, 0xB0, layer, mode, 17x RGB]
```

- There is **no global colour field**. Bytes `5..7` are key 1, bytes `8..10` key 2, and so
  on: one flat run of 17 RGB triplets starting at byte 5, 56 bytes in total. Only as many
  triplets as the board has keys are visible; the rest map to unpopulated slots.
- `layer` is **zero based** here, unlike the key records of the same device, which are one
  based.
- Modes: `off = 0`, `static = 1`, `reactive = 2`, `ripple = 3`, `rainbow = 4`. The mode and
  the triplets are independent fields, and every mode honours the per-key colours.
- `[0x03, 0xFB, 0xFB, 0xFB]` must be sent **immediately before every LED write**. Without it
  the LED packet is accepted and silently discarded.

Reading bytes `5..7` as a base colour followed by 16 per-key triplets gives a packet of the
same size (`8 + 16*3 = 5 + 17*3`) and the same result for solid colours, so it is an easy
mistake to make. With per-key colours it shifts every colour one key to the right: sending
base `red` with keys `[red, blue, orange, green, off, ...]` lights up as
`red, red, blue, orange, green, off, ...` on hardware. Verified on a `514c:8850` with 12 keys
and 2 knobs, see [issue #1](https://github.com/Mun1to/hotkeyconfig/issues/1).

That is a different model and a different protocol, so supporting it means writing a new
backend, not tweaking this one.

### What hotkeyconfig does instead

Since every control sends a **unique combo**, the listener knows exactly which one you
pressed, and changing the lighting takes about 20 ms (only the two LED messages, no key
rebinding). So a control can carry a `color`, and when you trigger it the **whole keyboard**
takes that colour for half a second and then returns to the layer colour.

It is press feedback, not a static per-key colour, and it needs the listener running. It
also needs the layer to have lighting configured, since otherwise there is no colour to
return to.

## Why unique combos at all

The keyboard sends **standard HID keystrokes**. Nothing in a keypress says "this came from key 7 of
the macropad", so the host cannot tell it apart from the same keystroke on your main keyboard.

The trick is to make every control send a combo that no physical keyboard can produce: the phantom
keys `F13` to `F24` (they exist in the HID spec but not on real keyboards) combined with a
different **modifier family per layer**. See
[`hotkeyconfig/combos.py`](../hotkeyconfig/combos.py). For the usual 12 keys + 2 knobs + 3 layers
board that yields **54 collision free combos**.

Two consequences worth knowing:

1. The combos live in the keyboard's **own memory**, so they survive without the app, on any
   computer.
2. The listener never needs to track the active layer. The hardware already switched it, and each
   layer sends different combos.

## Diagnostics

```bash
hotkeyconfig detect                       # is it there, and on which interface
python scripts/_probe_caps.py             # dump the HID capabilities (report lengths)
```

If a write fails with "the parameter is incorrect", the framing is almost certainly the problem:
re-read the framing section above.
