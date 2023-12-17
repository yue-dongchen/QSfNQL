# QSfNQL
**Q**WERTY **S**hortcuts **f**or **N**on-**Q**WERTY **L**ayouts
## What this is
We spend a lot of time using keyboards to interact with computers. Following people's needs to input languages containing symbols that are not present or easily accessible on the US ANSI keyboard, desktop operating system developers have developed software to map keyboard scancodes to characters flexibly, defined by configuration files.

This is a double-edged sword. Applications (by and large) act on shortcuts based on whatever symbols are sent to them by the lower levels of the keyboard input stack. Since the _base layer_<sup><a href="#glossary-base-layer">[glossary]</a></sup> for many layouts is not QWERTY, shortcuts are not necessarily triggered according to their ANSI QWERTY positions. An example problem scenario would be someone confusing `Ctrl+A` with `Ctrl+Q` as they switch back-and-forth between a French AZERTY layout, and an Arabic layout that falls back to QWERTY for `Ctrl` shortcuts: Whilst the Arabic layout is active, they use their AZERTY-oriented muscle memory to perform `Ctrl+A`, which results in `Ctrl+Q` and the application unexpectedly quits. Replace that French AZERTY layout with something less QWERTY-like such as DVORAK, Colemak or Neo — and muscle memory conflicts become even more disruptive.

**Thus, even ignoring QWERTY-oriented shortcuts ergonomics, it is highly necessary for anyone who frequently switches between several layouts to have shortcuts fixed in uniform positions. This project aims to be a repository for configurations of non-QWERTY layouts, adapted to use QWERTY as their `Ctrl` layer.**

## Implementation
To implement such layouts, it is necessary to understand, in the context of an operating system's keyboard input stack, the components responsible for translating absolute key positions into key symbols. Below is an overview of the respective approaches on a few systems.
### Linux (Wayland and X11 display servers)
<figure>
  <img src="https://learning.lpi.org/en/learning-materials/102-500/106/106.1/106.1_01/images/image_01.png" alt="Keyboard input stack in X11 systems, depicting (from bottom to top of image) the kernel, X Server, and three clients — display manager, window manager, and web browser — each sitting on top of a GUI toolkit and Xlib">
  <figcaption>Keyboard input stack in X11 systems<br><a href="https://learning.lpi.org/en/learning-materials/102-500/106/106.1/106.1_01/">Linux Professional Institue Inc</a> | <a href="https://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons BY-NC-ND 4.0</a></figcaption>
</figure>

Both X11 and Wayland use _XKB_<sup><a href="#glossary-XKB">[glossary]</a></sup>-style configuration files to set the keymaps. With X11, XKB is integrated into the display server, sending processed key events to their clients. In the case of Wayland, XKB has been extracted into the library `libxkbcommon`, which the clients themselve invoke when they demand a translation of the keycodes sent via `libinput`.

So on Linux systems, the configurations manifest in XKB-style configuration files. X11 uses `/usr/share/X11/XKB/`; any modification requires root. `libxkbcommon` on Wayland additionally scans `$HOME/.config/XKB/`, and allows for user-specific modifications.
### macOS
Haven't looked much into it. But by the looks of it the configuration XML of `.keylayout` files seem pretty well defined; and there's even a polished open-source GUI program, [“Ukelele”](https://software.sil.org/ukelele/), that allows layout definition graphically.
### Windows
No clue. Contributions would be greatly appreciated. I've heard that the Microsoft Keyboard Layout Creator does not work on Windows 11.

## Glossary
- <p id="glossary-base-layer">base layer: the symbols produced when no modifier key (`Shift`, `AltGr`, `CapsLock`, etc.) is active</p>
- <p id="glossary-XKB">XKB: "X Keyboard Extension", a part of X11 display servers supporting complex keyboard layouts