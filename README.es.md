<div align="center">

<img src="branding/logo-256.png" alt="HotKeyConfig" height="88">

# HotKeyConfig

**Cada tecla, a tu manera.**

Convierte un macropad barato de AliExpress en un panel donde **cada tecla ejecuta lo que tú quieras**.

<a href="https://github.com/Mun1to/hotkeyconfig/releases/latest/download/hotkeyconfig-setup.exe"><picture><source media="(prefers-color-scheme: dark)" srcset=".github/readme/download-windows-dark.es.svg"><img src=".github/readme/download-windows-light.es.svg" alt="Descargar el instalador de Windows"></picture></a>

Windows · [Licencia MIT](LICENSE) · actualizaciones automáticas · [sin drivers, sin Zadig](SECURITY.md)

[Todas las descargas](https://github.com/Mun1to/hotkeyconfig/releases/latest) · [Seguridad](SECURITY.md) · [Issues](https://github.com/Mun1to/hotkeyconfig/issues) · [English](README.md)

</div>

<p align="center"><img src="branding/screenshots/es/01-macropad.png" alt="Pantalla principal de HotKeyConfig"></p>

<p align="center">Pantalla principal: capas, luz RGB por capa y cada tecla mostrando la acción que tiene asignada.</p>

## Cómo funciona

<p align="center"><picture>
<source media="(prefers-color-scheme: dark) and (max-width: 600px)" srcset=".github/readme/how-it-works-dark-narrow.es.svg">
<source media="(max-width: 600px)" srcset=".github/readme/how-it-works-light-narrow.es.svg">
<source media="(prefers-color-scheme: dark)" srcset=".github/readme/how-it-works-dark.es.svg">
<img src=".github/readme/how-it-works-light.es.svg" alt="Enchufa el macropad y pulsa Flashear teclado.: Eso graba un código único por tecla en la memoria del propio teclado. Solo hay que hacerlo una vez y persiste sin la app. Haz clic en una tecla o perilla y elige qué hace.: Para abrir un programa, escribe su nombre y elígelo de la lista de lo que tienes instalado (sale con su icono), o pulsa la carpeta para buscar el archivo. Pulsa Probar para verlo al momento. Ajustes &gt; «Arrancar con Windows» y olvídate.: El listener arranca con Windows, así que tus teclas responden con esta ventana cerrada y después de reiniciar.">
</picture></p>

## Qué hace

Esos macropads `1189:8842` de AliExpress (12 teclas, 2 perillas, 3 capas) vienen con un software
propietario que te pasan por un enlace de Google Drive. `hotkeyconfig` lo sustituye por una app de
escritorio open source que programa el teclado **por HID nativo de Windows** y luego ejecuta tus
acciones.

|  |  |
|---|---|
| **Cada tecla y perilla hace lo que quieras** | ejecutar scripts, abrir apps, carpetas o URLs, escribir texto, controlar multimedia, enviar atajos, manejar ventanas o encadenar varias acciones. |
| **También tu teclado normal** | graba cualquier combinación (por ejemplo `Ctrl+Alt+S`) y asígnale una acción, y **remapea teclas sueltas** (`Bloq Mayús` a `Esc`). |
| **Iluminación RGB** | color y modo (fija, al pulsar, onda) por capa, grabados en el teclado. |
| **Color por acción** | dale un color a un control y el teclado se tiñe de ese color al pulsarlo, volviendo después al color de la capa. Ojo, es **solo al pulsar**: el ch57x no puede mantener un color distinto en cada tecla (ver [docs/PROTOCOL.md](docs/PROTOCOL.md)), y hace falta tener el listener en marcha. |
| **Eliges tu teclado de una lista** | ocho formas distintas, de 3 teclas y una perilla hasta 16 y tres, y colocas las perillas donde las tienes de verdad. |
| **Teclas que funcionan con la app cerrada** | las de abrir un programa o una web se pueden delegar en Windows y responden sin el listener (ver más abajo, tiene sus límites). |
| **App de escritorio de verdad** | editor visual, tema claro/oscuro que sigue al del sistema, español e inglés, arranque con Windows, instalador y **actualizaciones automáticas**. |
| **Privada por diseño** | sin telemetría, sin llamadas de red en el listener y **no es un keylogger** (ver [SECURITY.md](SECURITY.md)). |

## Capturas

<table>
<tr>
<td width="50%" valign="top"><img src="branding/screenshots/es/05-models.png" alt="Selector de modelo de teclado"><br>Elige tu teclado: Se elige por lo que se puede contar: teclas y perillas.</td>
<td width="50%" valign="top"><img src="branding/screenshots/es/02-editor.png" alt="Editor de acciones"><br>Editor de acciones: Cada acción, su color, y si debe funcionar con la app cerrada.</td>
</tr>
</table>

<table>
<tr>
<td width="50%" valign="top"><img src="branding/screenshots/es/03-shortcuts.png" alt="Atajos globales y remapeos"><br>Atajos de teclado: Cada combinación dibujada tecla a tecla, como está en el teclado.</td>
<td width="50%" valign="top"><img src="branding/screenshots/es/04-guide.png" alt="Guía interactiva"><br>Guía interactiva: Un tour que te lleva por la ventana real, resaltando cada botón.</td>
</tr>
</table>

<table>
<tr>
<td width="50%" valign="top"><img src="branding/screenshots/es/06-settings.png" alt="Pestaña de ajustes"><br>Ajustes: Arrancar con Windows, si las teclas responden ahora mismo, qué teclado tienes, dónde está tu configuración e idioma.</td>
<td width="50%"></td>
</tr>
</table>

## Descargar

**[Descargar el instalador de Windows](https://github.com/Mun1to/hotkeyconfig/releases/latest/download/hotkeyconfig-setup.exe)**
o mira [todas las descargas](https://github.com/Mun1to/hotkeyconfig/releases/latest).

El instalador no pide administrador, crea los accesos directos y ofrece una casilla para arrancar
con Windows. Tu configuración se guarda en `%LOCALAPPDATA%\hotkeyconfig\profile.yaml`.

> Puede que SmartScreen avise de editor desconocido: la app todavía no está firmada. Pulsa
> **Más información** y luego **Ejecutar de todas formas**, o compílala tú desde el código
> (instrucciones más abajo).

<details>
<summary><b>Actualizaciones automáticas</b></summary>

La app consulta los releases de GitHub y te ofrece la nueva versión en un aviso. Las
actualizaciones se descargan **solo por HTTPS desde GitHub** y se **verifica su SHA256** antes de
ejecutar nada.

</details>

## Hardware compatible

Macropads con el chip **ch57x**. Estos teclados se venden con mil nombres distintos y sin ningún
número de modelo impreso, así que en la app eliges el tuyo por lo que se puede contar: cuántas
teclas y cuántas perillas tiene.

| Forma | Teclas | Perillas | Estado |
|-------|--------|----------|--------|
| 3 teclas y 1 perilla | 3 | 1 | compatible [^mod] |
| 4 teclas | 4 | 0 | compatible |
| 6 teclas y 1 perilla | 6 | 1 | compatible |
| 9 teclas y 2 perillas | 9 | 2 | compatible |
| **12 teclas y 2 perillas** | 12 | 2 | **compatible, verificado en hardware real** |
| 12 teclas y 3 perillas | 12 | 3 | compatible |
| 15 teclas y 3 perillas | 15 | 3 | compatible |
| 16 teclas y 3 perillas | 16 | 3 | todavía no, necesita su propio protocolo |

[^mod]: En este modelo el firmware solo guarda los modificadores de la primera tecla de una secuencia.

Por identificador USB: `1189:8840`, `1189:8842`, `1189:8850` y `514c:8851` hablan el protocolo que
implementa este programa. El `1189:8890` y el `514c:8850` se reconocen y se nombran, pero todavía no
se pueden programar porque usan otros protocolos. Si tienes uno,
[cuéntalo en un issue](https://github.com/Mun1to/hotkeyconfig/issues) y, si puedes, adjunta una
captura USB: es lo que permite añadirlo.

Solo el modelo de 12 teclas y 2 perillas está verificado en hardware real, que es el que tiene su
autor. El resto comparten protocolo y deberían funcionar; se agradece que lo confirmes en cualquier
caso.

```
 [ T1 ][ T2 ][ T3 ][ T4 ]      ( P1 )
 [ T5 ][ T6 ][ T7 ][ T8 ]      ( P2 )
 [ T9 ][T10 ][T11 ][T12 ]
   12 teclas             2 perillas (giro + clic)
```

Cada perilla tiene tres acciones: girar a la izquierda, pulsar y girar a la derecha.

> La parte única vive **en la memoria del teclado**, así que persiste. El listener solo hace falta
> para **ejecutar tus acciones**: si no está en marcha, esas acciones simplemente no se disparan.

## Tipos de acción

| `type` | Qué hace | Campos |
|--------|----------|--------|
| `run` | Ejecuta un script o comando | `cmd` o `args`, `cwd`, `shell` |
| `open` | Abre una app, carpeta o URL | `target`, `browser` |
| `text` | Escribe o pega texto | `text` |
| `media` | Tecla multimedia, gestionada por la app | `key` |
| `hotkey` | Manda un atajo a la app activa | `send` |
| `window` | Minimiza, maximiza, restaura, cierra | `do` |
| `sequence` | Varias acciones en orden | `steps` |
| `native` | Se graba en el hardware (volumen, etc.) | `key` |
| `unassigned` | No hace nada | (ninguno) |

En `open`, el `target` puede ser una ruta, una URL o `app:<id>`, que es lo que guarda la interfaz al
elegir un programa de la lista. Ese identificador se lo da Windows, así que funciona igual con
programas de la Microsoft Store, que no tienen ninguna ruta a la que apuntar.

## Combinaciones que ya usa otro programa

Los combos que se graban en el teclado (`Ctrl+Alt+F13` y parecidos) están pensados para que ningún
otro programa los use, pero eso no lo garantiza nada: en el ordenador donde se escribió esto, 7 de
los 54 estaban cogidos. Una tecla así no hace nada al pulsarla y no hay forma de saberlo mirándola.

La app se lo pregunta a Windows y avisa en la pantalla del macropad, con un botón para darles
combinaciones libres (solo se mueven las teclas afectadas, y hay que reflashear). Lo mismo al grabar
un atajo del teclado normal: si ya es de Windows, te dice para qué lo usa antes de que lo asignes.

## Que una tecla funcione con la app cerrada

El listener es quien ejecuta tus acciones: sin él, la tecla manda su combinación y no pasa nada. Para
las acciones de **abrir** hay una salida, marcando **Funciona con la app cerrada** en el editor de la
tecla. El programa crea entonces un acceso directo de Windows con esa combinación asignada, y quien
responde es el Explorador, que ya está siempre en marcha.

Lo que conviene saber antes de usarlo:

- Solo sirve para **abrir** programas, carpetas y enlaces. Escribir texto, lanzar scripts o el
  destello de color siguen necesitando el listener.
- No vale para las **perillas**: sus combinaciones llevan la tecla Windows y el sistema no la admite
  en un acceso directo.
- **No se graba en el teclado.** La configuración vive en Windows, en ese acceso directo. En otro
  ordenador el teclado seguirá mandando su combinación y allí no ocurrirá nada. Ningún programa puede
  cambiar eso: un macropad solo sabe enviar pulsaciones.
- Si cambias la tecla de sitio muchas veces, Windows puede quedarse con la asociación antigua y esa
  combinación deja de responder. Se arregla reiniciando el Explorador o el equipo.

> Sobre `open`: si dejas `browser` vacío, los enlaces se abren en el navegador del sistema. Si pones
> el nombre de uno instalado (la interfaz te lista los que encuentra), se abren siempre en ese. En
> los dos casos la ventana pasa al primer plano, así que al pulsar la tecla vas a lo que acabas de
> abrir en vez de quedarte en la aplicación donde estabas.

## Cómo funciona

Estos macropads no exponen un identificador por tecla: mandan **pulsaciones HID estándar**, igual
que cualquier teclado. Para que cada tecla sea **única** hay que hacerle enviar una combinación que
ningún teclado físico produce.

`hotkeyconfig` usa las teclas fantasma `F13` a `F24` con una familia de modificadores distinta por
capa, lo que da **54 combos sin colisión** (12 teclas x 3 capas, más 2 perillas x 3 acciones x 3
capas). Un listener local asocia cada combo con tu acción. Como cada combo es único, el runtime no
necesita saber qué capa está activa: ya la ha cambiado el hardware.

Por eso el proyecto son dos mitades:

1. **Grabar los combos en el teclado**, por **HID nativo de Windows**, igual que hace la
   herramienta propietaria: **sin Zadig, sin drivers, sin Rust**. El protocolo está portado byte a
   byte desde [`kriomant/ch57x-keyboard-tool`](https://github.com/kriomant/ch57x-keyboard-tool),
   que queda como backend alternativo opcional.
2. **Escuchar esos combos y ejecutar acciones**, con un listener local basado en la librería
   [`keyboard`](https://pypi.org/project/keyboard/), en un icono de bandeja.

## Privacidad

- El listener **solo** reacciona a los combos `F13` a `F24` configurados.
- **No** registra, almacena ni transmite tus pulsaciones normales, y **no** hace llamadas de red.
- El actualizador es lo único que usa la red, y únicamente contra GitHub.

Todo el detalle, la política de dependencias y el estado de auditoría en [SECURITY.md](SECURITY.md).

## Hoja de ruta

- **Más teclados.** Hoy solo se admite la familia ch57x (`1189:8842`, k884x), porque el protocolo
  de configuración es específico de cada modelo. El siguiente es el **K8850** (`514c:8850`, 16
  teclas y 3 perillas), que además es interesante por sí mismo: ese sí admite **RGB real por
  tecla**, algo que el k884x no puede hacer de ninguna manera.
- **Firmar el ejecutable**, para quitar el aviso de SmartScreen.

¿Tienes otro macropad y quieres que funcione? Abre un issue con su identificador USB
(`hotkeyconfig detect`) y, si puedes, una captura USB del software del fabricante
configurándolo.

## No te fíes, compruébalo

No hace falta que te fíes. Abre [AI-AUDIT.md](AI-AUDIT.md), pega ese texto en tu IA y te dirá en tu idioma qué hace este programa de verdad: qué envía por internet, qué toca en tu ordenador y qué ejecuta al instalarse.

<details>
<summary><b>Para desarrolladores</b></summary>

Solo hace falta Python 3.11+ (nada de Rust):

```bash
python -m venv .venv
.venv\Scripts\pip install -e .
```

Línea de comandos:

```bash
hotkeyconfig detect              # ¿teclado conectado?
hotkeyconfig generate            # crea config/profile.yaml con los combos únicos
hotkeyconfig flash               # graba los combos en la memoria del teclado
hotkeyconfig run                 # escucha y ejecuta (icono en la bandeja)
hotkeyconfig gui                 # abre el configurador visual
hotkeyconfig autostart install   # arrancar con Windows
```

Compilar la app autónoma y el instalador (PyInstaller + Inno Setup):

```bash
.venv\Scripts\pip install -e ".[dev]"
.venv\Scripts\python scripts/build.py              # dist\hotkeyconfig\hotkeyconfig.exe
.venv\Scripts\python scripts/build.py --installer  # + dist\hotkeyconfig-<version>-setup.exe
```

Regenerar las capturas del README (abre la app con un perfil de demostración, nunca toca el tuyo):

```bash
.venv\Scripts\python scripts/screenshots.py
```

Ejemplo de perfil: [`config/profile.example.yaml`](config/profile.example.yaml). Notas del
protocolo: [`docs/PROTOCOL.md`](docs/PROTOCOL.md).

</details>

## Licencia y créditos

MIT, ver [LICENSE](LICENSE).

El protocolo k884x está portado desde
[`ch57x-keyboard-tool`](https://github.com/kriomant/ch57x-keyboard-tool) (MIT, de `kriomant`),
enviado por HID nativo en lugar de libusb. Listener basado en
[`keyboard`](https://pypi.org/project/keyboard/).

<br>

<p align="center">Hecho por <a href="https://munito.dev"><b>Munir Torres</b></a>. También suyo: <a href="https://adeorq.com">Adeorq</a> · <a href="https://vocript.app">VoCript</a> · <a href="https://winshotx.com">winshotx</a> · <a href="https://github.com/Mun1to/Expoal">Expoal</a> · <a href="https://github.com/Mun1to/froede">froede</a></p>
