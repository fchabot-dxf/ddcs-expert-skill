# DDCS Expert M350 — screens and keys

> Every measured statement names the bench test that measured it (see evidence.md).

## Measured on this controller

- A USB keyboard is supported (the Param page has #278 "USB keyboard type"). *(owner report)*
- USB keys measured on the Expert, matching the vendor's published key map: Esc cancels a dialog and leaves a sub-state (nothing when idle); F1-F6 are the soft keys; Tab and Backspace both go to the next tab; the arrows are up / down / left / right; PgUp / PgDn page; Enter loads the file under the cursor on the Program page and answers a dialog; numpad 8 / 2 / 6 / 4 / 9 / 1 jog Y+ / Y- / X+ / X- / Z+ / Z-, numpad 0 / . jog the 5th axis - / +, numpad 5 toggles high / low jog speed. numpad Enter is NOT Start here (it is on the V4.1) unless a parameter assigns it. *(USB HID dongle key sweep, jogs read on the DRO (DONGLE-TESTS.md))*
- Print Screen sent as a USB keyboard report writes the whole screen to the program disk (CNCDISK) as BMP<n>.bmp within 3 s: 1024x600, 16-bit, 1,228,854 bytes. Numbering starts at BMP0 and the clock resets at power-up, so a new capture is the file whose bytes changed, not a new name or a newer time. (A plain USB keyboard's Print Screen was seen to write nothing in two earlier tries; not understood.) *(Print Screen (HID 0x46) from a USB HID dongle, the program disk checked)*
- Reset is a state of its own: after a Reset the status cell reads RESET, register 10002 = 2 and Start is refused; a second Reset gives READY (10002 = 0). A keyboard Reset (a K key assigned Reset) also clears a syntax-error ERROR. After any Reset, press Reset again or check READY before a Start. *(K-key session (Scroll Lock = Reset), V17a_badvar.nc (every step captured))*
