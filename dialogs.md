# DDCS Expert M350 — operator prompts and dialogs

> Every measured statement names the bench test that measured it (see evidence.md).


## The register forms

OK/Cancel prompt:
```gcode
#1505=1(Check the clamp)
```
*(operator macros (READ_VAR.nc, COPY_WCS.nc))*

Number entry into #120:
```gcode
#2070=120(Part count)
```
*(operator macros)*

Display-only message:
```gcode
#1505=-5000(Roughing done)
```
*(operator macros (READ_VAR.nc, COPY_WCS.nc))*

Status bar line:
```gcode
#1503=1(Roughing)
```
*(vendor macro: slib-m.nc O10300; not a bench test)*

Beep 500 ms:
```gcode
#2042=500 ( Beep duration ms )
```
*(a declared form; not bench-measured)*

## Also measured on this controller

- #1505=1(message) is a blocking OK/Cancel dialog; #1505=-5000(message) is a display-only message; Esc sets #1505 to 0. *(operator macros (READ_VAR.nc, COPY_WCS.nc))*
- Inside a #1505=<n>(message), % is a printf specifier (X=%.3f prints a number from #1510/#1511); in an ordinary comment it is a plain character. *(operator macros)*
- #2070 = <var>(prompt) pauses for the operator to type a number into that variable. *(operator macros)*
- #2037 = 65536 + [KeyValue - 1000] presses a panel key from a macro (MDI 1348, System Info 1378); it must run as code. *(A7b_BUTTON_ONEWAY.nc, photographed pendant)*
- #1503=-3000(message) is a non-blocking status line: the text shows in the bottom status strip, prefixed [0000], with no dialog and no button. It stayed there through a 15 s dwell while the program ran on underneath, and the next message replaced it. *(V24_1503_persist.nc (second run, G04 P15000, photographed every ~2.3 s))*
- #1505=3(message) opens a modal Enter/Esc dialog (prefix [5003]) that holds the program by itself, with no M0 after it: Enter leaves 1 in #1505 and Esc leaves 0 (read back as 1.000 and 0.000). *(V25_1505_binary.nc (rewritten without M0: run 1 Enter, run 2 Esc, photographed))*
- The controller puts its own tag in front of each message: #1505=1 shows [5001], #1505=3 shows [5003], #1503=-3000 and #1505=-5000 show [0000]. The tag is not part of your text. *(V24_1503_persist.nc, V25_1505_binary.nc (photographed))*
- The Monitor page's toolpath panel draws only with Pr244 "Enable realtime toolpath" = Yes and Pr245 "Toolpath mode" = Line (it drew a 20 mm square and its diagonal) or 3D; with Statue a path above work zero drew nothing. In 3D the frame does not re-fit: the view angles Pr261-Pr263 decide what lands in view (30/30/30 drew off-frame, 30/0/30 drew the cube). Pr261-Pr263 are not on the Param page list nor found by its search; they are set over Modbus. *(V34_toolpath_square.nc, V35_toolpath_cube.nc (every state captured))*

## Beep, status and message lines (attested, not bench-measured)

- The Expert has no .rc dialog system: its firmware holds none of the dialog strings. Its operator prompts are the register forms (#1505, #2070). *(firmware read: FIRMWARE-RE the parse.out read; not a bench test)*
- The vendor's sensor waits write the status line with #1503 = 1(<text>) while they poll. *(vendor macro: slib-m.nc O10300; not a bench test)*
