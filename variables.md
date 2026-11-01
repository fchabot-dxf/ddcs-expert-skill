# DDCS Expert M350 — variables

> Every measured statement names the bench test that measured it (see evidence.md).

## Access: who reads and writes, and when

Where a read or a write comes from, and when it works. Each mark points to the fact that says so: its page, and its bench test there. ✅ works, ⛔ does not, ⚠️ with a catch, ❔ no fact says yet, — does not apply.

| from | read at idle | read during a run | write at idle | write during a run | survives a reboot |
|---|---|---|---|---|---|
| the running program | — | ✅ integration.md (V14_wcs_pos.nc) | — | ✅ integration.md (V6_set.nc) · ⚠️ integration.md (Modbus reads of the whole parameter table against the setting file) | ✅ integration.md (Pr132 written by FC16) |
| a hook macro (an M-code body, a K-key or key-N macro, a start or error hook) | ❔ | ❔ | ❔ | ❔ | ⚠️ integration.md (power-cycle test) · ✅ operation.md (the 2026-10-30 session on the machine) |
| MDI (a line typed on the pendant) | ❔ | ❔ | ❔ | ❔ | ❔ |
| injected G-code (Modbus register 3000) | ✅ integration.md (register scan for #100 = 12345) · ✅ integration.md (six-variable test) | ❔ | ✅ integration.md (#1505 message shown on the pendant) · ⚠️ hazards.md (photographed pendant error) · ✅ integration.md (K-key assignment by Modbus injection) · ⚠️ integration.md (direct FC16 writes to 7024 and 7026 with read-backs) | ❔ | ❔ |
| the uservar file (a PC, over the share) | ✅ integration.md (operator test writes decoded over SMB) · ⚠️ integration.md (A9-a) | ⛔ integration.md (A9-a) | ❔ | ⛔ integration.md (A9-a) | ❔ |
| Modbus registers (a PC) | ⚠️ integration.md (register scan for #100 = 12345) · ✅ integration.md (21-parameter read) · ✅ integration.md (read-only FC03 against the pendant photo and the WCS table) | ⚠️ hazards.md (CALIBRATE.nc) · ✅ integration.md (V23_lineadvance.nc) · ✅ integration.md (read-only FC03 against the pendant photo and the WCS table) | ✅ integration.md (direct FC16 writes to 7024 and 7026 with read-backs) · ✅ integration.md (Pr132 written by FC16) | ❔ | ⛔ integration.md (Pr132 written by FC16) |
| the setting file / Param page | ✅ integration.md (Modbus reads of the whole parameter table against the setting file) | ❔ | ❔ | ❔ | ✅ integration.md (Pr132 written by FC16) |
| the pendant | ❔ | ❔ | ✅ integration.md (Pr132 written by FC16) | ❔ | ❔ |

Source: **Variables-ENG 01-04-2025.csv**, a variable table for the DDCS Expert M350. The descriptions are the table's own words.

The bold words on a row are the vendor's own mark for it, in the words of the vendor's legend for this column (DDCS_Variables_mapping_2025-01-04.xlsx, cell B2): R/O "read only", B "kept after a reboot", S "lost at a reboot". A mark the legend does not name says nothing. "Kept after a reboot" and "lost at a reboot" are about a reboot: none of these marks says whether a write is accepted.

"read and write" on #1552-#1575: the vendor's other variable sheet (Chinese; a screenshot, 2026-11-01): general outputs 01-24 R/W.

⛔ Never write a variable you have not looked up here. A value of 0 does not mean unused.

## Beyond the table

What is declared or measured about these variables beyond the table, each with its evidence.

| variables | | what | evidence |
|---|---|---|---|
| #100-#499 | measured | The work share's uservar file holds them (slot = #var - 100; its slots run on to #549, but #500 up may be the parameters: not settled, never write them). A PC reads it; a PC write made during a run never reached the program, and one between runs is not measured: never write it. | operator test writes decoded over SMB |
| #100-#499 | ❔ not measured | Whether the uservar file is loaded into them at boot, so that they come back after a reboot whatever their S mark says. | a question: no measurement yet |
| #500-#1499 | ⛔ do not write | The Param page, Pr0-Pr999: macro #N is Pr(N - 500). A write changes a machine setting. Never as scratch. | Modbus reads of the whole parameter table against the setting file (FINDINGS section 28) |
| #578 | measured | The active work system number; it does NOT follow a G54..G59 in the program: emit G54..G59 to select. | V4_active_wcs.nc, DIAG_g53setup.nc |
| #622-#626 | ⛔ do not write | The machine position after each axis goes home: a write moves the machine zero. Never as scratch. | the variable table names it |
| #632 | measured | Marked B (kept after a reboot), but HOW it is written decides: written by a Modbus FC16 it was back at 800 after a power-off; edited on the pendant it kept. | Pr132 written by FC16 (2026-10-28), read back after the owner's power-off, then the pendant edit and SYSDISK/setting decoded over SMB |
| #655-#670 | ⛔ do not write | The software limits (enable, stop mode, the negative and positive limit of each axis): a write moves or disables a limit. Never as scratch. | the variable table names it |
| #735-#739 | ⛔ do not write | The home offset of each axis: a write moves the machine zero. Never as scratch. | the variable table names it |
| #766-#767 | ⛔ do not write | The serial baud rates: breaking them loses the Modbus link. Never as scratch. | the variable table names it |
| #779 | ⛔ do not write | The Modbus RTU setting: breaking it loses the Modbus link. Never as scratch. | the variable table names it |
| #796-#797 | ⛔ do not write | The serial stop bits / Modbus mode (the vendor table leaves them unnamed): breaking them loses the Modbus link. Never as scratch. | panel photo + live adapter test |
| #800-#804 | ⛔ do not write | The G92 offset of each axis: a write moves the G92 frame. Never as scratch. | the variable table names it |
| #805-#834 | ⛔ do not write | G54..G59 offsets, 5 per system: a write moves a work zero. Never as scratch. | panel read of all six systems |
| #880-#884 | ❔ not measured | Marked R/O, but the table's own note says the machine position can be edited: whether a write is accepted. | a question: no measurement yet |
| #900-#915 | measured | Writable (a program writes H01..H16): their B mark is about a reboot, not about whether a write is accepted. | V6_set.nc, V6_restore.nc |
| #900-#915 | ⛔ do not write | H01..H16 tool-length offsets: a stray value is a tool length. Never as scratch. | V6_set.nc, V6_restore.nc |
| #915 | measured | The Modbus variable reader borrows it (#915 = #<target>, read register 7330) and must restore 0: it is H16. | six-variable test |
| #916-#919 | ⛔ do not write | The vendor table names these "Possibly H17..H20 offset" (only H01-H16 are measured). Treat as tool lengths: never as scratch. | the variable table names it |
| #920-#939 | ⛔ do not write | D01..D20 cutter-compensation offsets: a stray value is a cutter radius. Never as scratch. | the variable table names it |
| #1505 | measured | #1505=1(message) is a blocking OK/Cancel dialog; #1505=-5000(message) a display-only message. | operator macros (READ_VAR.nc, COPY_WCS.nc) |
| #1630 | ⛔ do not write | Never READ it inside a running program either: it wedges the analyzer until a reboot. | PUSH_RUNSTATE.nc |
| #2000-#2499 | ❔ not measured | Whether they survive a reboot: the table marks them S (lost at a reboot) or nothing (#2039-#2041, #2043-#2071); do not rely on either until it is measured. | a question: no measurement yet |
| #2037 | measured | #2037 = 65536 + [KeyValue - 1000] presses a panel key; it must run as code. | A7b_BUTTON_ONEWAY.nc, photographed pendant |
| #2070 | measured | #2070 = <var>(prompt) pauses for the operator to type a number into that variable. | operator macros |

## Lost at a reboot [S]

| variables | description | note |
|---|---|---|
| #0-#9 | **lost at a reboot** (the vendor's mark) — User variables independent for slib and user UE — This range is not suitable for storing axis coordinates, from variables #790-#844. If you store axis coordinates in this range of variable numbers, the values will be lost. Only this range is suitable for the seamless movement of the axes, with G0, G1, G2, G3 commands. |  |
| #10-#21 | **lost at a reboot** (the vendor's mark) — User variables independent for slib and user UE |  |
| #22-#49 | **lost at a reboot** (the vendor's mark) — User variables independent for slib and user UE — This range is not suitable for storing axis coordinates, from variables #790-#844. If you store axis coordinates in this range of variable numbers, the values will be lost. Only this range is suitable for the seamless movement of the axes, with G0, G1, G2, G3 commands. |  |
| #50-#55 | **lost at a reboot** (the vendor's mark) — Used in G28 and G29 commands |  |
| #56-#100 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* | measured, ❔ not measured |
| #101 | **lost at a reboot** (the vendor's mark) — Used in subroutine O503 (dual y-axis) from slib-g as port number — Subroutine O503 is obsolete | measured, ❔ not measured |
| #102-#499 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* | measured, ❔ not measured |
| #504-#505 | **lost at a reboot** (the vendor's mark) — ??pulses per unit | ⛔ do not write |
| #530 | **lost at a reboot** (the vendor's mark) — machine type | ⛔ do not write |
| #531 | **lost at a reboot** (the vendor's mark) — RTCP Resolution | ⛔ do not write |
| #571 | **lost at a reboot** (the vendor's mark) — Enabling motion scheduling of the G0 segment — G0段运动规划使能 | ⛔ do not write |
| #572 | **lost at a reboot** (the vendor's mark) — G0 command motion characteristics — 0: Interpolation; 1:Independent, Interpolation:synergistic movement of each axis./Independent:each axis independently moves at G0 speed. | ⛔ do not write |
| #586 | **lost at a reboot** (the vendor's mark) — Spindle PWM electric level | ⛔ do not write |
| #587 | **lost at a reboot** (the vendor's mark) — PWM rising rate from 0V to 10V | ⛔ do not write |
| #702 | **lost at a reboot** (the vendor's mark) — The number of processed pieces (Current number of processed pieces) — Number of completed programs in the current session. Upgrades with the completion of M30. Cleared on reboot. | ⛔ do not write |
| #725 | **lost at a reboot** (the vendor's mark) — Feed rate change range — Magnification increment when handwheel exit and manual override adjustment. | ⛔ do not write |
| #726 | **lost at a reboot** (the vendor's mark) — Magnification decrease in pause — Suspension rate change when decelerating to stop. | ⛔ do not write |
| #727 | **lost at a reboot** (the vendor's mark) — Processing magnification change interval — Cooperate with parameter 225. | ⛔ do not write |
| #728 | **lost at a reboot** (the vendor's mark) — Magnification change when resetting alarm | ⛔ do not write |
| #729 | **lost at a reboot** (the vendor's mark) — Reset alarm rate change interval | ⛔ do not write |
| #742 | **lost at a reboot** (the vendor's mark) — Main interface display scheme — 主界面显示方案 | ⛔ do not write |
| #746 | **lost at a reboot** (the vendor's mark) — Max. polyline segments | ⛔ do not write |
| #749 | **lost at a reboot** (the vendor's mark) — parameter gradient — J参数梯度 | ⛔ do not write |
| #750 | **lost at a reboot** (the vendor's mark) — Maximum machining contour error | ⛔ do not write |
| #754 | **lost at a reboot** (the vendor's mark) — Toolpath width (Area width (X)) — Set individually, for each program file name (NC) | ⛔ do not write |
| #755 | **lost at a reboot** (the vendor's mark) — Toolpath Length (Height of area (Y)) — Set individually, for each program file name (NC) | ⛔ do not write |
| #756 | **lost at a reboot** (the vendor's mark) — Toolpath thickness (Region depth (Z)) — Set individually, for each program file name (NC) | ⛔ do not write |
| #757 | **lost at a reboot** (the vendor's mark) — The toolpath plane thickness in Statue mode — Set individually, for each program file name (NC) | ⛔ do not write |
| #758 | **lost at a reboot** (the vendor's mark) — The X Origin point on plane — Set individually, for each program file name (NC) | ⛔ do not write |
| #759 | **lost at a reboot** (the vendor's mark) — The Y Origin point on plane — Set individually, for each program file name (NC) | ⛔ do not write |
| #760 | **lost at a reboot** (the vendor's mark) — The Z Origin point on plane — Set individually, for each program file name (NC) | ⛔ do not write |
| #761 | **lost at a reboot** (the vendor's mark) — X-axis rotation angle in 3D toolpath mode | ⛔ do not write |
| #762 | **lost at a reboot** (the vendor's mark) — Y-axis rotation angle in 3D toolpath mode | ⛔ do not write |
| #763 | **lost at a reboot** (the vendor's mark) — Z-axis rotation angle in 3D toolpath mode | ⛔ do not write |
| #770 | **lost at a reboot** (the vendor's mark) — user permission key — This number is just a reminder of the superuser password. Changing it does not change the password. | ⛔ do not write |
| #835 | **lost at a reboot** (the vendor's mark) — G52 Temporary Coordinates offset X — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #836 | **lost at a reboot** (the vendor's mark) — G52 Temporary Coordinates offset Y — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #837 | **lost at a reboot** (the vendor's mark) — G52 Temporary coordinates offset Z — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #838 | **lost at a reboot** (the vendor's mark) — G52 Temporary Coordinates offset A — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #839 | **lost at a reboot** (the vendor's mark) — G52 Temporary Coordinates offset B — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #940 | **lost at a reboot** (the vendor's mark) — 1th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #941 | **lost at a reboot** (the vendor's mark) — 2th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #942 | **lost at a reboot** (the vendor's mark) — 3th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #946-#948 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1500 | **lost at a reboot** (the vendor's mark) — M1 Unconditional stop Status — Enable/disable for M1 |  |
| #1501 | **lost at a reboot** (the vendor's mark) — Tapping mode: 0 Left-hand tapping thread(Counter-thread) — 1Right-hand tapping thread(Normal thread), Thread cutting mode. Right or left. |  |
| #1502 | **lost at a reboot** (the vendor's mark) — Probe Mode — Sets the probing mode. 0-?; 1-movable sensor; 2-fixed sensor |  |
| #1503 | **lost at a reboot** (the vendor's mark) — prompt information — Prints text to the status bar, only works if "#269 Debug printing enable" = 1 |  |
| #1504 | **lost at a reboot** (the vendor's mark) — Target Tool number Temporary variables — Stores the tool number from the M6Txx command |  |
| #1505 | **lost at a reboot** (the vendor's mark) — Dialog prompt message — Displays a window with a message number. | measured |
| #1506 | **lost at a reboot** (the vendor's mark) — single piece processing finished mark — Saves how the last UE ended. M30 or M47 |  |
| #1507 | **lost at a reboot** (the vendor's mark) — Processing time record switch — Stops and starts the NC execution timer (Work Time) |  |
| #1508 | **lost at a reboot** (the vendor's mark) — 2nd probe operation switch |  |
| #1510 | **lost at a reboot** (the vendor's mark) — Print Value 1 — Works in conjunction with #1503 |  |
| #1511 | **lost at a reboot** (the vendor's mark) — Print Value 2 — Works in conjunction with #1503 |  |
| #1512 | **lost at a reboot** (the vendor's mark) — Print Value 3 — Works in conjunction with #1503 |  |
| #1513 | **lost at a reboot** (the vendor's mark) — Print Value 4 — Works in conjunction with #1503 |  |
| #1514 | **lost at a reboot** (the vendor's mark) — Lift up enable when pause |  |
| #1515 | **lost at a reboot** (the vendor's mark) — The Mark when X back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1516 | **lost at a reboot** (the vendor's mark) — The Mark when Y back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1517 | **lost at a reboot** (the vendor's mark) — The Mark when Z back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1518 | **lost at a reboot** (the vendor's mark) — The Mark when 4th back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1519 | **lost at a reboot** (the vendor's mark) — The Mark when 5th back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1544 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1545 | **lost at a reboot** (the vendor's mark) — Delay time (milliseconds) |  |
| #1546-#1551 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1552 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 01 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1553 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 02 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1554 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 03 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1555 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 04 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1556 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 05 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1557 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 06 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1558 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 07 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1559 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 08 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1560 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 09 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1561 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output Port 10 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1562 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 11 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1563 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 12 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1564 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 13 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1565 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 14 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1566 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output Port 15 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1567 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 16 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1568 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 17 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1569 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 18 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1570 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 19 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1571 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output Port 20 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1572 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 21 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1573 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 22 Status (unused) — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1574 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 23 Status (unused) — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1575 | **read and write** (the vendor's other sheet) · **lost at a reboot** (the vendor's mark) — Common Output port 24 Status (unused) — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1577-#1615 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1616 | **lost at a reboot** (the vendor's mark) — Current spindle speed — Displays how fast the spindle is currently turning. If the spindle is stopped (M5) then 0. |  |
| #1617 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1618 | **lost at a reboot** (the vendor's mark) — Home Start Sign 0:Start 1:End — When any axis is homed = 1. Otherwise 0. Homing progress indicator at start. |  |
| #1619 | **lost at a reboot** (the vendor's mark) — label prompt information — Outputs text to a label printer (physical device) |  |
| #1627-#1629 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1637-#1699 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1719-#1749 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1750-#1759 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1760 | **lost at a reboot** (the vendor's mark) — modbus function macro address — accept data buffer address (e.g. 1790) |  |
| #1761 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Serial port number for receiving data (there are 0 and 1) |  |
| #1762 | **lost at a reboot** (the vendor's mark) — modbus function macro address — ID number of the slave device |  |
| #1763 | **lost at a reboot** (the vendor's mark) — modbus function macro address — encoder circle address |  |
| #1764 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Number of bytes |  |
| #1765 | **lost at a reboot** (the vendor's mark) — modbus function macro address — function code 03 |  |
| #1766 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Receives some data |  |
| #1767 | **lost at a reboot** (the vendor's mark) — modbus function macro address — laps return code (the value of this variable is displayed in a message in the status bar) |  |
| #1768 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1769 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Trigger for the end of data reception. It is set to 1, then WHILE [#1769 NE 0] DO1, a pause G04P10 is written in the loop body. When #1769=0, the data is accepted. |  |
| #1770 | **lost at a reboot** (the vendor's mark) — modbus function macro address — This variable stores the result of data transformation |  |
| #1771 | **lost at a reboot** (the vendor's mark) — modbus function macro address — row index (usually set to 1790 - data buffer address) |  |
| #1772 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Number of bytes |  |
| #1773 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Number type - 1 - 16-bit signed integer; 4 is a 32-bit unsigned integer. Accepts only 1, 2, 3, 4 |  |
| #1774 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1775 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Data conversion (1 - convert data) |  |
| #1776-#1789 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1790 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Used as a buffer for receiving data. Any free modbus variable can serve as a data buffer. |  |
| #1791-#1799 | **lost at a reboot** (the vendor's mark) — modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1800 | **lost at a reboot** (the vendor's mark) — Analyze channel X axis control mark 0:X axis is in analyzing 1:X axis is in manual mode — Utility function, manufacturer only. |  |
| #1801 | **lost at a reboot** (the vendor's mark) — Analyze channel Y axis control mark 0:Y axis is in analyzing 1:Y axis is in manual mode — Utility function, manufacturer only. |  |
| #1802 | **lost at a reboot** (the vendor's mark) — Analyze channel Z axis control mark 0:Z axis is in analyzing 1:Z axis is in manual mode — Utility function, manufacturer only. |  |
| #1803 | **lost at a reboot** (the vendor's mark) — Analyze channel 4th axis control mark 0:4th axis is in analyzing 1:4th axis is in manual mode — Utility function, manufacturer only. |  |
| #1804 | **lost at a reboot** (the vendor's mark) — Analyze channel 5th axis control mark 0:5th axis is in analyzing 1:5th axis is in manual mode — Utility function, manufacturer only. |  |
| #1805-#1894 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1895 | **lost at a reboot** (the vendor's mark) — G31 X axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1896 | **lost at a reboot** (the vendor's mark) — G31 Y axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1897 | **lost at a reboot** (the vendor's mark) — G31 Z axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1898 | **lost at a reboot** (the vendor's mark) — G31 4th axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1899 | **lost at a reboot** (the vendor's mark) — G31 5th axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1900 | **lost at a reboot** (the vendor's mark) — G31 X axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1901 | **lost at a reboot** (the vendor's mark) — G31 Y axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1902 | **lost at a reboot** (the vendor's mark) — G31 Z axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1903 | **lost at a reboot** (the vendor's mark) — G31 4th axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1904 | **lost at a reboot** (the vendor's mark) — G31 5th axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1905 | **lost at a reboot** (the vendor's mark) — G31 X detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1906 | **lost at a reboot** (the vendor's mark) — G31 Y detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1907 | **lost at a reboot** (the vendor's mark) — G31 Z detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1908 | **lost at a reboot** (the vendor's mark) — G31 4th detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1909 | **lost at a reboot** (the vendor's mark) — G31 5th detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1910 | **lost at a reboot** (the vendor's mark) — G31 X detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1911 | **lost at a reboot** (the vendor's mark) — G31 Y detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1912 | **lost at a reboot** (the vendor's mark) — G31 Z detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1913 | **lost at a reboot** (the vendor's mark) — G31 4th detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1914 | **lost at a reboot** (the vendor's mark) — G31 5th detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1915 | **lost at a reboot** (the vendor's mark) — G31 X axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1916 | **lost at a reboot** (the vendor's mark) — G31 Y axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1917 | **lost at a reboot** (the vendor's mark) — G31 Z axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1918 | **lost at a reboot** (the vendor's mark) — G31 4th axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1919 | **lost at a reboot** (the vendor's mark) — G31 5th axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1920 | **lost at a reboot** (the vendor's mark) — G31 X axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1921 | **lost at a reboot** (the vendor's mark) — G31 Y axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1922 | **lost at a reboot** (the vendor's mark) — G31 Z axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1923 | **lost at a reboot** (the vendor's mark) — G31 4th axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1924 | **lost at a reboot** (the vendor's mark) — G31 5th axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1925 | **lost at a reboot** (the vendor's mark) — G31 The X mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1926 | **lost at a reboot** (the vendor's mark) — G31 The Y mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1927 | **lost at a reboot** (the vendor's mark) — G31 The Z mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1928 | **lost at a reboot** (the vendor's mark) — G31 The 4th mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1929 | **lost at a reboot** (the vendor's mark) — G31 The 5th mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1930 | **lost at a reboot** (the vendor's mark) — Key Indicator 1 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1931 | **lost at a reboot** (the vendor's mark) — Key Indicator 2 Output Control (Error) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1932 | **lost at a reboot** (the vendor's mark) — Key Indicator 3 Output Control (Run) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1933 | **lost at a reboot** (the vendor's mark) — Key Indicator 4 Output Control (USB) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1934 | **lost at a reboot** (the vendor's mark) — Key Indicator 5 Output Control (ESC) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1935 | **lost at a reboot** (the vendor's mark) — Key Indicator 6 Output Control (UP) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1936 | **lost at a reboot** (the vendor's mark) — Key Indicator 7 Output Control (Enter) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1937 | **lost at a reboot** (the vendor's mark) — Key Indicator 8 Output Control (LEFT) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1938 | **lost at a reboot** (the vendor's mark) — Key Indicator 9 Output Control (DOWN) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1939 | **lost at a reboot** (the vendor's mark) — Key Indicator 10 Output Control (RIGHT) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1940 | **lost at a reboot** (the vendor's mark) — Key Indicator 11 Output Control (Hight/Low Speed) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1941 | **lost at a reboot** (the vendor's mark) — Key Indicator 12 Output Control (Axis 5 -) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1942 | **lost at a reboot** (the vendor's mark) — Key Indicator 13 Output Control (Brake point Resume) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1943 | **lost at a reboot** (the vendor's mark) — Key Indicator 14 Output Control (Axis 5+) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1944 | **lost at a reboot** (the vendor's mark) — Key Indicator 15 Output Control (K1) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1945 | **lost at a reboot** (the vendor's mark) — Key Indicator 16 Output Control (K2) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1946 | **lost at a reboot** (the vendor's mark) — Key Indicator 17 Output Control (K3) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1947 | **lost at a reboot** (the vendor's mark) — Key Indicator 18 Output Control (K4) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1948 | **lost at a reboot** (the vendor's mark) — Key Indicator 19 Output Control (K5) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1949 | **lost at a reboot** (the vendor's mark) — Key Indicator 20 Output Control (K6) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1950 | **lost at a reboot** (the vendor's mark) — Key Indicator 21 Output Control (K7) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1951 | **lost at a reboot** (the vendor's mark) — Key Indicator 22 Output Control (SPINDLE) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1952 | **lost at a reboot** (the vendor's mark) — Key Indicator 23 Output Control (TruCut) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1953 | **lost at a reboot** (the vendor's mark) — Key Indicator 24 Output Control (CONT STEP MPG) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1954 | **lost at a reboot** (the vendor's mark) — Key Indicator 25 Output Control (START) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1955 | **lost at a reboot** (the vendor's mark) — Key Indicator 26 Output Control (PAUSE) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1956 | **lost at a reboot** (the vendor's mark) — Key Indicator 27 Output Control (RESET) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1957 | **lost at a reboot** (the vendor's mark) — Key Indicator 28 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1958 | **lost at a reboot** (the vendor's mark) — Key Indicator 29 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1959 | **lost at a reboot** (the vendor's mark) — Key Indicator 30 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1960 | **lost at a reboot** (the vendor's mark) — Key Indicator 31 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1961 | **lost at a reboot** (the vendor's mark) — Key Indicator 32 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1962-#1969 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1970 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 1 — Used in mulprobe.nc file |  |
| #1971 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 2 — Used in mulprobe.nc file |  |
| #1972 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 3 — Used in mulprobe.nc file |  |
| #1973 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 4 — Used in mulprobe.nc file |  |
| #1974 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 5 — Used in mulprobe.nc file |  |
| #1975 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 6 — Used in mulprobe.nc file |  |
| #1976 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 7 — Used in mulprobe.nc file |  |
| #1977 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 8 — Used in mulprobe.nc file |  |
| #1978 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 9 — Used in mulprobe.nc file |  |
| #1979 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 10 — Used in mulprobe.nc file |  |
| #1980 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. eleven — Used in mulprobe.nc file |  |
| #1981 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 12 — Used in mulprobe.nc file |  |
| #1982 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 13 — Used in mulprobe.nc file |  |
| #1983 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 14 — Used in mulprobe.nc file |  |
| #1984 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 15 — Used in mulprobe.nc file |  |
| #1985 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 16 — Used in mulprobe.nc file |  |
| #1986 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 17 — Used in mulprobe.nc file |  |
| #1987 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 18 — Used in mulprobe.nc file |  |
| #1988 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 19 — Used in mulprobe.nc file |  |
| #1989 | **lost at a reboot** (the vendor's mark) — One Key Probe Tool No. 20 — Used in mulprobe.nc file |  |
| #1990 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #1992 | **lost at a reboot** (the vendor's mark) — Home function enabled (does not work anymore) — Used to enable homing after downloading. Now doesn't work. |  |
| #1993 | **lost at a reboot** (the vendor's mark) — Probe enabled after tool change |  |
| #1994 | **lost at a reboot** (the vendor's mark) — RTCP Enable/Disable — Turning this setting on doesn't make sense. Because RTCP doesn't work. If enabled, the controller will fail. |  |
| #1995 | **lost at a reboot** (the vendor's mark) — Current Max.AxisNumber — Number of controller axes |  |
| #1996 | **lost at a reboot** (the vendor's mark) — Self-define M code Temporary variables (Custom M code number temporary variable) — used in ext_button.nc (substituted as command number M - M#1996). May be equal to 250-263. Enables the EXT KEY FUNC K01-K14 commands of the Monitor - Manual page |  |
| #1997 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 1 status — Transfers the state of the virtual button FUNC K01, (pressed or released), to the M250 command |  |
| #1998 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 2 status — Transfers the state of the virtual button FUNC K02, (pressed or released), to the command M251 |  |
| #1999 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 3 status — Transfers the state of the virtual button FUNC K03, (pressed or released), to the command M252 |  |
| #2000 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 4 status — Transfers the state of the virtual button FUNC K04, (pressed or released), to the command M253 | ❔ not measured |
| #2001 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 5 status — Transfers the state of the virtual button FUNC K05, (pressed or released), to the command M254 | ❔ not measured |
| #2002 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 6 status — Transfers the state of the virtual button FUNC K06, (pressed or released), to the command M255 | ❔ not measured |
| #2003 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 7 status — Transfers the state of the virtual button FUNC K07, (pressed or released), to the command M256 | ❔ not measured |
| #2004 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 8 status — Transfers the state of the virtual button FUNC K08, (pressed or released), to the command M257 | ❔ not measured |
| #2005 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 9 status — Transfers the state of the virtual button FUNC K09, (pressed or released), to the command M258 | ❔ not measured |
| #2006 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 10 status — Transfers the state of the FUNC K10 virtual button, (pressed or released), to the M259 command | ❔ not measured |
| #2007 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 11 status — Transfers the state of the FUNC K11 virtual button, (pressed or released), to the M260 command | ❔ not measured |
| #2008 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 12 status — Transfers the status of the FUNC K12 virtual button, (pressed or released), to the M261 command | ❔ not measured |
| #2009 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 13 status — Sends the state of the FUNC K13 virtual button, (pressed or released), to the M262 command | ❔ not measured |
| #2010 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 14 status — Transfers the status of the FUNC K14 virtual button, (pressed or released), to the M263 command | ❔ not measured |
| #2011 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 15 status — Missing code in slib-m. The FUNC K15 button is missing from the Manual page. | ❔ not measured |
| #2012 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 16 status — Missing code in slib-m. The FUNC K16 button is missing from the Manual page. | ❔ not measured |
| #2013 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 17 status — Missing code in slib-m. The FUNC K17 button is missing from the Manual page. | ❔ not measured |
| #2014 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 18 status — Missing code in slib-m. The FUNC K18 button is missing from the Manual page. | ❔ not measured |
| #2015 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 19 status — Missing code in slib-m. The FUNC K19 button is missing from the Manual page. | ❔ not measured |
| #2016 | **lost at a reboot** (the vendor's mark) — Manually operation self-define Key 20 status — Missing code in slib-m. The FUNC K20 button is missing from the Manual page. | ❔ not measured |
| #2017 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 1 status — Status of virtual button 1, (pressed or released), from command M270 | ❔ not measured |
| #2018 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 2 status — Status of virtual button 2, (pressed or released), from command M271 | ❔ not measured |
| #2019 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 3 status — Status of virtual button 3, (pressed or released), from command M272 | ❔ not measured |
| #2020 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 4 status — Status of virtual button 4, (pressed or released), from command M273 | ❔ not measured |
| #2021 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 5 status — Status of virtual button 5, (pressed or released), from command M274 | ❔ not measured |
| #2022 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 6 status — Status of virtual button 6, (pressed or released), from command M275 | ❔ not measured |
| #2023 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 7 status — Status of virtual button 7, (pressed or released), from command M276 | ❔ not measured |
| #2024 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 8 status — Status of virtual button 8, (pressed or released), from command M277 | ❔ not measured |
| #2025 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 9 status — Status of virtual button 9, (pressed or released), from command M278 | ❔ not measured |
| #2026 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 10 status — Status of virtual button 10, (pressed or released), from command M279 | ❔ not measured |
| #2027 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 11 status — Status of virtual button 11, (pressed or released), from command M280 | ❔ not measured |
| #2028 | **lost at a reboot** (the vendor's mark) — Automatically operation self-define Key 12 status — Status of virtual button 12, (pressed or released), from command M280 | ❔ not measured |
| #2029-#2036 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* | ❔ not measured |
| #2037 | **lost at a reboot** (the vendor's mark) — Virtual key trigger register 0x1017f 017f is the key number — 1 is the status when pressed down, Presses a virtual button, saves the entered value until reboot #2037=64536+function code | ❔ not measured, measured |
| #2038 | **lost at a reboot** (the vendor's mark) — Possibly Self-define G code Temporary variables — Used in CAM.nc. Substituted as the G number of the team - G#2038 | ❔ not measured |
| #2042 | **lost at a reboot** (the vendor's mark) — Beep sound for macro program — in milliseconds | ❔ not measured |
| #2072 | **lost at a reboot** (the vendor's mark) — K1 function Key indicator Address — Control variable number, button LED K1 (#[1930+#2072]) | ❔ not measured |
| #2073 | **lost at a reboot** (the vendor's mark) — K2 function Key indicator Address — Control variable number, button LED K2 (#[1930+#2073]) | ❔ not measured |
| #2074 | **lost at a reboot** (the vendor's mark) — K3 function Key indicator Address — Control variable number, button LED K3 (#[1930+#2074]) | ❔ not measured |
| #2075 | **lost at a reboot** (the vendor's mark) — K4 function Key indicator Address — Control variable number, button LED K4 (#[1930+#2075]) | ❔ not measured |
| #2076 | **lost at a reboot** (the vendor's mark) — K5 function Key indicator Address — Control variable number, button LED K5 (#[1930+#2076]) | ❔ not measured |
| #2077 | **lost at a reboot** (the vendor's mark) — K6 function Key indicator Address — Control variable number, button LED K6 (#[1930+#2077]) | ❔ not measured |
| #2078 | **lost at a reboot** (the vendor's mark) — K7 function Key indicator Address — Control variable number, button LED K7 (#[1930+#2078]) | ❔ not measured |
| #2079 | **lost at a reboot** (the vendor's mark) — K8 function Key indicator Address (not working) — does not work | ❔ not measured |
| #2080-#2091 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* | ❔ not measured |
| #2092 | **lost at a reboot** (the vendor's mark) — Teach M118 input detection input port number — Used in M118 command. Subroutine O10118 in slib-m | ❔ not measured |
| #2093 | **lost at a reboot** (the vendor's mark) — Teach M118 input port effective level — Used in M118 command. Subroutine O10118 in slib-m | ❔ not measured |
| #2094-#2095 | **lost at a reboot** (the vendor's mark) — Used in the M129 command. | ❔ not measured |
| #2097 | **lost at a reboot** (the vendor's mark) — Stores the result of the M129 command — values 1 or 0 | ❔ not measured |
| #2098-#2099 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* | ❔ not measured |
| #2100-#2104 | **lost at a reboot** (the vendor's mark) — Used in slib-g — in O501 (homing) and O503, as an intermediate variable, With her participation, the function of calculating the accuracy of the operation of the homing sensor works | ❔ not measured |
| #2105 | **lost at a reboot** (the vendor's mark) — Used in slib-g — in O501 (homing) and O503, as an intermediate variable, With her participation, the function of calculating the number of errors when entering the homing sensor works | ❔ not measured |
| #2106-#2499 | **lost at a reboot** (the vendor's mark) — *(no description in the table)* | ❔ not measured |

## Read-only

| variables | description | note |
|---|---|---|
| #633 | **read only** (the vendor's mark) — Probe Tool block thickness — Useful floating tool | ⛔ do not write |
| #790 | **read only** (the vendor's mark) — axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #791 | **read only** (the vendor's mark) — Y axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #792 | **read only** (the vendor's mark) — Z axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #793 | **read only** (the vendor's mark) — 4th axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #794 | **read only** (the vendor's mark) — 5th axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #850 | **read only** (the vendor's mark) — G54 Coordinates X Absolute position — Current axis position in G54 | ⛔ do not write |
| #851 | **read only** (the vendor's mark) — G54 Coordinates Y Absolute position — Current axis position in G54 | ⛔ do not write |
| #852 | **read only** (the vendor's mark) — G54 Coordinates Z Absolute position — Current axis position in G54 | ⛔ do not write |
| #853 | **read only** (the vendor's mark) — G54 Coordinates A Absolute position — Current axis position in G54 | ⛔ do not write |
| #854 | **read only** (the vendor's mark) — G54 Coordinates B Absolute position — Current axis position in G54 | ⛔ do not write |
| #855 | **read only** (the vendor's mark) — G55 Coordinates X Absolute position — Current axis position in G55 | ⛔ do not write |
| #856 | **read only** (the vendor's mark) — G55 Coordinates Y Absolute position — Current axis position in G55 | ⛔ do not write |
| #857 | **read only** (the vendor's mark) — G55 Coordinates Z Absolute position — Current axis position in G55 | ⛔ do not write |
| #858 | **read only** (the vendor's mark) — G55 Coordinates A Absolute position — Current axis position in G55 | ⛔ do not write |
| #859 | **read only** (the vendor's mark) — G55 Coordinates B Absolute position — Current axis position in G55 | ⛔ do not write |
| #860 | **read only** (the vendor's mark) — G56 Coordinates X Absolute position — Current axis position in G56 | ⛔ do not write |
| #861 | **read only** (the vendor's mark) — G56 Coordinates Y Absolute position — Current axis position in G56 | ⛔ do not write |
| #862 | **read only** (the vendor's mark) — G56 Coordinates Z Absolute position — Current axis position in G56 | ⛔ do not write |
| #863 | **read only** (the vendor's mark) — G56 Coordinates A Absolute position — Current axis position in G56 | ⛔ do not write |
| #864 | **read only** (the vendor's mark) — G56 Coordinates B Absolute position — Current axis position in G56 | ⛔ do not write |
| #865 | **read only** (the vendor's mark) — G57 Coordinates X Absolute position — Current axis position in G57 | ⛔ do not write |
| #866 | **read only** (the vendor's mark) — G57 Coordinates Y Absolute position — Current axis position in G57 | ⛔ do not write |
| #867 | **read only** (the vendor's mark) — G57 Coordinates Z Absolute position — Current axis position in G57 | ⛔ do not write |
| #868 | **read only** (the vendor's mark) — G57 Coordinates A Absolute position — Current axis position in G57 | ⛔ do not write |
| #869 | **read only** (the vendor's mark) — G57 Coordinates B Absolute position — Current axis position in G57 | ⛔ do not write |
| #870 | **read only** (the vendor's mark) — G58 Coordinates X Absolute position — Current axis position in G58 | ⛔ do not write |
| #871 | **read only** (the vendor's mark) — G58 Coordinates Y Absolute position — Current axis position in G58 | ⛔ do not write |
| #872 | **read only** (the vendor's mark) — G58 Coordinates Z Absolute position — Current axis position in G58 | ⛔ do not write |
| #873 | **read only** (the vendor's mark) — G58 Coordinates A Absolute position — Current axis position in G58 | ⛔ do not write |
| #874 | **read only** (the vendor's mark) — G58 Coordinates B Absolute position — Current axis position in G58 | ⛔ do not write |
| #875 | **read only** (the vendor's mark) — G59 Coordinates X Absolute position — Current axis position in G59 | ⛔ do not write |
| #876 | **read only** (the vendor's mark) — G59 Coordinates Y Absolute position — Current axis position in G59 | ⛔ do not write |
| #877 | **read only** (the vendor's mark) — G59 Coordinates Z Absolute position — Current axis position in G59 | ⛔ do not write |
| #878 | **read only** (the vendor's mark) — G59 Coordinates A Absolute position — Current axis position in G59 | ⛔ do not write |
| #879 | **read only** (the vendor's mark) — G59 Coordinates B Absolute position — Current axis position in G59 | ⛔ do not write |
| #880 | **read only** (the vendor's mark) — MACH Coordinates X Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write, ❔ not measured |
| #881 | **read only** (the vendor's mark) — MACH Coordinates Y Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write, ❔ not measured |
| #882 | **read only** (the vendor's mark) — MACH Coordinates Z Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write, ❔ not measured |
| #883 | **read only** (the vendor's mark) — MACH Coordinates A Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write, ❔ not measured |
| #884 | **read only** (the vendor's mark) — MACH Coordinates B Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write, ❔ not measured |
| #885 | **read only** (the vendor's mark) — G92 Coordinates X Absolute position — Current axis position in G92 | ⛔ do not write |
| #886 | **read only** (the vendor's mark) — G92 Coordinates Y Absolute position — Current axis position in G92 | ⛔ do not write |
| #887 | **read only** (the vendor's mark) — G92 Coordinates Z Absolute position — Current axis position in G92 | ⛔ do not write |
| #888 | **read only** (the vendor's mark) — G92 Coordinates A Absolute position — Current axis position in G92 | ⛔ do not write |
| #889 | **read only** (the vendor's mark) — G92 Coordinates B Absolute position — Current axis position in G92 | ⛔ do not write |
| #1509 | **read only** (the vendor's mark) — buzzer control — Beeps with a squeaker |  |
| #1520 | **read only** (the vendor's mark) — Common Input port 01 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1521 | **read only** (the vendor's mark) — Common Input port 02 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1522 | **read only** (the vendor's mark) — Common Input port 03 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1523 | **read only** (the vendor's mark) — Common Input port 04 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1524 | **read only** (the vendor's mark) — Common Input port 05 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1525 | **read only** (the vendor's mark) — Common Input port 06 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1526 | **read only** (the vendor's mark) — Common Input port 07 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1527 | **read only** (the vendor's mark) — Common Input port 08 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1528 | **read only** (the vendor's mark) — Common Input port 09 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1529 | **read only** (the vendor's mark) — Common Input port 10 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1530 | **read only** (the vendor's mark) — Common Input port 11 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1531 | **read only** (the vendor's mark) — Common Input port 12 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1532 | **read only** (the vendor's mark) — Common Input port 13 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1533 | **read only** (the vendor's mark) — Common Input port 14 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1534 | **read only** (the vendor's mark) — Common Input port 15 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1535 | **read only** (the vendor's mark) — Common Input port 16 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1536 | **read only** (the vendor's mark) — Common Input port 17 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1537 | **read only** (the vendor's mark) — Common Input port 18 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1538 | **read only** (the vendor's mark) — Common Input port 19 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1539 | **read only** (the vendor's mark) — Common Input port 20 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1540 | **read only** (the vendor's mark) — Common Input port 21 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1541 | **read only** (the vendor's mark) — Common Input port 22 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1542 | **read only** (the vendor's mark) — Common Input port 23 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1543 | **read only** (the vendor's mark) — Common Input port 24 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1576 | **read only** (the vendor's mark) — *(no description in the table)* |  |
| #1620 | **read only** (the vendor's mark) — Analyze channel 1 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1621 | **read only** (the vendor's mark) — Analyze channel 2 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1622 | **read only** (the vendor's mark) — Analyze channel 3 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1623 | **read only** (the vendor's mark) — Analyze channel 4 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1624 | **read only** (the vendor's mark) — Analyze channel 5 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1625 | **read only** (the vendor's mark) — Analyze channel 6 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1626 | **read only** (the vendor's mark) — Analyze channel 7 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1630-#1636 | **read only** (the vendor's mark) — Analyze channel 1 status: -1:Idle 0:Working 1: Pause — Utility function, manufacturer only. | ⛔ do not write |
| #1700 | **read only** (the vendor's mark) — Function Key 1 Status |  |
| #1701 | **read only** (the vendor's mark) — Function Key 2 Status |  |
| #1702 | **read only** (the vendor's mark) — Function Key 3 Status |  |
| #1703 | **read only** (the vendor's mark) — Function Key 4 Status |  |
| #1704 | **read only** (the vendor's mark) — Function Key 5 Status |  |
| #1705 | **read only** (the vendor's mark) — Function Key 6 Status |  |
| #1706 | **read only** (the vendor's mark) — Function Key 7 Status |  |
| #1707 | **read only** (the vendor's mark) — Function Key 8 Status |  |
| #1708 | **read only** (the vendor's mark) — Function Key 9 Status |  |
| #1709 | **read only** (the vendor's mark) — Function Key 10 Status |  |
| #1710 | **read only** (the vendor's mark) — Function Key 11 Status |  |
| #1711 | **read only** (the vendor's mark) — Extended Key 1 Status |  |
| #1712 | **read only** (the vendor's mark) — Extended Key 2 Status |  |
| #1713 | **read only** (the vendor's mark) — Extended Key 3 Status |  |
| #1714 | **read only** (the vendor's mark) — Extended Key 4 Status |  |
| #1715 | **read only** (the vendor's mark) — Extended Key 5 Status |  |
| #1716 | **read only** (the vendor's mark) — Extended Key 6 Status |  |
| #1717 | **read only** (the vendor's mark) — Extended Key 7 Status |  |
| #1718 | **read only** (the vendor's mark) — Extended Key 8 Status |  |

## Kept after a reboot [B]

| variables | description | note |
|---|---|---|
| #500 | **kept after a reboot** (the vendor's mark) — Motor start speed mm/min | ⛔ do not write |
| #501 | **kept after a reboot** (the vendor's mark) — X-axis pulses per mm p/mm | ⛔ do not write |
| #502 | **kept after a reboot** (the vendor's mark) — Y-axis pulses per mm p/mm | ⛔ do not write |
| #503 | **kept after a reboot** (the vendor's mark) — Z-axis pulses per mm p/mm | ⛔ do not write |
| #506 | **kept after a reboot** (the vendor's mark) — 4th axis pulses per mm | ⛔ do not write |
| #507 | **kept after a reboot** (the vendor's mark) — 4th-axis unit 0：pulse/degree — 1：pulse/revolution, If the axis is not used as a servo spindle, 507=1 cannot be set. Otherwise, the 4th axis will not work correctly during manual movements! | ⛔ do not write |
| #508 | **kept after a reboot** (the vendor's mark) — pulse equivalent of the 5th Axis | ⛔ do not write |
| #509 | **kept after a reboot** (the vendor's mark) — 5th-axis unit 0：pulse/degree — 1：pulse/revolution, If the axis is not used as a servo spindle, 509=1 cannot be set. Otherwise, the 5th axis will not work correctly during manual movements! | ⛔ do not write |
| #510 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #511 | **kept after a reboot** (the vendor's mark) — Time interval between DIR & PUL Unit: ns | ⛔ do not write |
| #512 | **kept after a reboot** (the vendor's mark) — X-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #513 | **kept after a reboot** (the vendor's mark) — Y-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #514 | **kept after a reboot** (the vendor's mark) — Z-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #515 | **kept after a reboot** (the vendor's mark) — 4th-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #516 | **kept after a reboot** (the vendor's mark) — 5th-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #517 | **kept after a reboot** (the vendor's mark) — X-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #518 | **kept after a reboot** (the vendor's mark) — Y-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #519 | **kept after a reboot** (the vendor's mark) — Z-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #520 | **kept after a reboot** (the vendor's mark) — 4th-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #521 | **kept after a reboot** (the vendor's mark) — 5th-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #522 | **kept after a reboot** (the vendor's mark) — Enable axis mapping function — Note: When this function is enabled, the two axies are linked only/during machining. | ⛔ do not write |
| #523 | **kept after a reboot** (the vendor's mark) — Primary axis selection | ⛔ do not write |
| #524 | **kept after a reboot** (the vendor's mark) — Mapping axis selection | ⛔ do not write |
| #525-#529 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #532-#534 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #535 | **kept after a reboot** (the vendor's mark) — X-axis max. speed in manual control | ⛔ do not write |
| #536 | **kept after a reboot** (the vendor's mark) — Y axis max. speed in manual control | ⛔ do not write |
| #537 | **kept after a reboot** (the vendor's mark) — Z axis max. speed in manual control | ⛔ do not write |
| #538 | **kept after a reboot** (the vendor's mark) — 4th axis max. speed in manual control | ⛔ do not write |
| #539 | **kept after a reboot** (the vendor's mark) — 5th axis max. speed in manual control | ⛔ do not write |
| #540 | **kept after a reboot** (the vendor's mark) — HIGH speed of X-axis in manual control | ⛔ do not write |
| #541 | **kept after a reboot** (the vendor's mark) — HIGH speed of Y-axis in manual control | ⛔ do not write |
| #542 | **kept after a reboot** (the vendor's mark) — HIGH speed of Z-axis in manual control | ⛔ do not write |
| #543 | **kept after a reboot** (the vendor's mark) — HIGH speed of 4th axis in manual control | ⛔ do not write |
| #544 | **kept after a reboot** (the vendor's mark) — HIGH speed of 5th-axis in manual control | ⛔ do not write |
| #545 | **kept after a reboot** (the vendor's mark) — LOW speed of X-axis in manual control | ⛔ do not write |
| #546 | **kept after a reboot** (the vendor's mark) — LOW speed of Y-axis in manual control | ⛔ do not write |
| #547 | **kept after a reboot** (the vendor's mark) — LOW speed of Z-axis in manual control | ⛔ do not write |
| #548 | **kept after a reboot** (the vendor's mark) — LOW speed of 4th axis in manual control | ⛔ do not write |
| #549 | **kept after a reboot** (the vendor's mark) — LOW speed of 5th axis in manual control | ⛔ do not write |
| #550 | **kept after a reboot** (the vendor's mark) — X-axis start acceleration in manual control | ⛔ do not write |
| #551 | **kept after a reboot** (the vendor's mark) — Y-axis start acceleration in manual control | ⛔ do not write |
| #552 | **kept after a reboot** (the vendor's mark) — Z-axis start acceleration in manual control | ⛔ do not write |
| #553 | **kept after a reboot** (the vendor's mark) — 4th-axis start acceleration in manual control | ⛔ do not write |
| #554 | **kept after a reboot** (the vendor's mark) — 5th-axis start acceleration in manual control | ⛔ do not write |
| #555 | **kept after a reboot** (the vendor's mark) — X-axis Stop acceleration in manual control | ⛔ do not write |
| #556 | **kept after a reboot** (the vendor's mark) — Y-axis Stop acceleration in manual control | ⛔ do not write |
| #557 | **kept after a reboot** (the vendor's mark) — Z-axis Stop acceleration in manual control | ⛔ do not write |
| #558 | **kept after a reboot** (the vendor's mark) — 4th axis Stop acceleration in manual control | ⛔ do not write |
| #559 | **kept after a reboot** (the vendor's mark) — 5th axis Stop acceleration in manual control | ⛔ do not write |
| #560 | **kept after a reboot** (the vendor's mark) — Speed Selection: 0: G Code / 1: Default | ⛔ do not write |
| #561 | **kept after a reboot** (the vendor's mark) — default operation speed — Unit: mm/min | ⛔ do not write |
| #562 | **kept after a reboot** (the vendor's mark) — linear feed moves (G01) acceleration speed — Unit: mm/min | ⛔ do not write |
| #563 | **kept after a reboot** (the vendor's mark) — Move as quickly as possible to a given point (G0) Speed — Unit: mm/min | ⛔ do not write |
| #564 | **kept after a reboot** (the vendor's mark) — Maximum Machining speed — Unit: mm/min | ⛔ do not write |
| #565 | **kept after a reboot** (the vendor's mark) — Z-axis lifting protection speed — Unit: mm/min | ⛔ do not write |
| #566 | **kept after a reboot** (the vendor's mark) — Z-axis dropping protection speed — Unit: mm/min | ⛔ do not write |
| #567 | **kept after a reboot** (the vendor's mark) — X-axis protection speed — Unit: mm/min | ⛔ do not write |
| #568 | **kept after a reboot** (the vendor's mark) — Y-axis protection speed — Unit: mm/min | ⛔ do not write |
| #569 | **kept after a reboot** (the vendor's mark) — Z axis safe height — Unit: mm | ⛔ do not write |
| #570 | **kept after a reboot** (the vendor's mark) — Z-axis retraction distance when paused — Unit: mm, Z lift distance on pause | ⛔ do not write |
| #573 | **kept after a reboot** (the vendor's mark) — Arc-interpolation algorithm 0: Hard algorithm 1: Soft algorithm | ⛔ do not write |
| #574 | **kept after a reboot** (the vendor's mark) — Soft-arc algorithm linear precision Unit: mm | ⛔ do not write |
| #575 | **kept after a reboot** (the vendor's mark) — Circular centrifugal acceleration | ⛔ do not write |
| #576 | **kept after a reboot** (the vendor's mark) — Macro scan switch 0: Close 1: Open — Changing this setting should work without reboot | ⛔ do not write |
| #577 | **kept after a reboot** (the vendor's mark) — macro program file main program no. | ⛔ do not write |
| #578 | **kept after a reboot** (the vendor's mark) — Displays the number of the current working SC. 1-G54; 6-G59; 0-G92. Toggles them if the controller is restarted after the change. | ⛔ do not write, measured |
| #579 | **kept after a reboot** (the vendor's mark) — Spindle interface type: 0: Analog 1: Plu&Dir 2: Multi-Speed | ⛔ do not write |
| #580 | **kept after a reboot** (the vendor's mark) — Servo Spindle mapping axis: X / Y / Z / 4th / 5th Axis | ⛔ do not write |
| #581 | **kept after a reboot** (the vendor's mark) — Spindle start delay Unit: S | ⛔ do not write |
| #582 | **kept after a reboot** (the vendor's mark) — Maximum spindle speed | ⛔ do not write |
| #583 | **kept after a reboot** (the vendor's mark) — Ignore the S command 0:No 1:Yes | ⛔ do not write |
| #584 | **kept after a reboot** (the vendor's mark) — Stop spindle when program is paused 0:No 1:Yes — Whether to pause the spindle | ⛔ do not write |
| #585 | **kept after a reboot** (the vendor's mark) — Default spindle speed Unit: rpm | ⛔ do not write |
| #588 | **kept after a reboot** (the vendor's mark) — Multi-speed section counts 2-8 | ⛔ do not write |
| #589 | **kept after a reboot** (the vendor's mark) — Spindle stop delay | ⛔ do not write |
| #590 | **kept after a reboot** (the vendor's mark) — Action selection before starting 0: No Action 1: To Safety Z | ⛔ do not write |
| #591 | **kept after a reboot** (the vendor's mark) — Z-axis movement mode during pause 0: No action 1：ZZ lift distance (#70) — Whether to raise Z on pause. | ⛔ do not write |
| #592 | **kept after a reboot** (the vendor's mark) — Delay of M8/M9 commands Unit: S | ⛔ do not write |
| #593 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #594 | **kept after a reboot** (the vendor's mark) — Delay of M10/M11 commands Unit: S | ⛔ do not write |
| #595 | **kept after a reboot** (the vendor's mark) — IO input filter time width Unit: ms | ⛔ do not write |
| #596 | **kept after a reboot** (the vendor's mark) — Reset IO Configuration bit 01-16 — Changes the state of the outputs after a reboot | ⛔ do not write |
| #597 | **kept after a reboot** (the vendor's mark) — Reset IO Configuration bit 17-21 — Changes the state of the outputs after a reboot | ⛔ do not write |
| #598 | **kept after a reboot** (the vendor's mark) — Alarm output status configuration bit 01-16 | ⛔ do not write |
| #599 | **kept after a reboot** (the vendor's mark) — Alarm output status configuration bit 17-21 | ⛔ do not write |
| #600 | **kept after a reboot** (the vendor's mark) — Home mode 0: switch 1: Absolute — The function of determining the absolute position of the axes during loading (according to the encoder), instead of homing. If you set it to 1, it will try to determine the coordinates from the encoder after loading (of course, unsuccessfully). | ⛔ do not write |
| #601 | **kept after a reboot** (the vendor's mark) — Servo absolute laps at the X-axis Home — The absolute position of the encoder in the X-axis, at the home position of the X-axis. Used in the M110 command (subroutine O10110 in slib-m) | ⛔ do not write |
| #602 | **kept after a reboot** (the vendor's mark) — Servo absolute laps at the Y-axis Home — Absolute position of the encoder in the Y-axis, with the home position of the Y-axis. Used in the M111 command (subroutine O10111 in slib-m) | ⛔ do not write |
| #603 | **kept after a reboot** (the vendor's mark) — Servo absolute laps at the Z-axis Home — The absolute position of the encoder in the Z axis, with the home position of the Z axis. Used in the M112 command (subroutine O10112 in slib-m) | ⛔ do not write |
| #604 | **kept after a reboot** (the vendor's mark) — Servo absolute laps at the 4th-axis Home — Absolute position of the encoder in the A-axis, with the home position of the A-axis. Used in the M113 command (subroutine O10113 in slib-m) | ⛔ do not write |
| #605 | **kept after a reboot** (the vendor's mark) — Servo absolute laps at the 5th-axis Home — Absolute encoder position on the B-axis, when the B-axis is in the home position. Used in the M114 command (subroutine O10114 in slib-m) | ⛔ do not write |
| #606 | **kept after a reboot** (the vendor's mark) — Home detection times 1-5 — Number of visits to the homing sensor. You can not put 6. A glitch is possible. There will be a variable conflict. | ⛔ do not write |
| #607 | **kept after a reboot** (the vendor's mark) — X-axis homing speed Unit: mm/min | ⛔ do not write |
| #608 | **kept after a reboot** (the vendor's mark) — Y-axis homing speed Unit: mm/min | ⛔ do not write |
| #609 | **kept after a reboot** (the vendor's mark) — Z-axis homing speed Unit: mm/min | ⛔ do not write |
| #610 | **kept after a reboot** (the vendor's mark) — 4th-axis homing speed Unit: mm/min | ⛔ do not write |
| #611 | **kept after a reboot** (the vendor's mark) — 5th-axis homing speed Unit: mm/min | ⛔ do not write |
| #612 | **kept after a reboot** (the vendor's mark) — X-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #613 | **kept after a reboot** (the vendor's mark) — Y-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #614 | **kept after a reboot** (the vendor's mark) — Z-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #615 | **kept after a reboot** (the vendor's mark) — 4th-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #616 | **kept after a reboot** (the vendor's mark) — 5th-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #617 | **kept after a reboot** (the vendor's mark) — Maximum error of home switch | ⛔ do not write |
| #618 | **kept after a reboot** (the vendor's mark) — Second precision positioning speed | ⛔ do not write |
| #619-#621 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #622 | **kept after a reboot** (the vendor's mark) — Mach position after X go home | ⛔ do not write |
| #623 | **kept after a reboot** (the vendor's mark) — Mach position after Y go home | ⛔ do not write |
| #624 | **kept after a reboot** (the vendor's mark) — Mach position after Z go home | ⛔ do not write |
| #625 | **kept after a reboot** (the vendor's mark) — Mach position after 4th go home | ⛔ do not write |
| #626 | **kept after a reboot** (the vendor's mark) — Mach position after 5th go home | ⛔ do not write |
| #627 | **kept after a reboot** (the vendor's mark) — Home after booting 0：Yes 1: No | ⛔ do not write |
| #628 | **kept after a reboot** (the vendor's mark) — Is the Floating tool set valid? | ⛔ do not write |
| #629 | **kept after a reboot** (the vendor's mark) — Floating tool set thickness | ⛔ do not write |
| #630 | **kept after a reboot** (the vendor's mark) — Is the fixed tool set valid? | ⛔ do not write |
| #631 | **kept after a reboot** (the vendor's mark) — Probe detection times 1-5 — Probing cycle count | ⛔ do not write |
| #632 | **kept after a reboot** (the vendor's mark) — Initial speed of Probing Unit: mm/min | ⛔ do not write, measured |
| #634 | **kept after a reboot** (the vendor's mark) — Setting the tool to its original position — 对刀初始位置 | ⛔ do not write |
| #635 | **kept after a reboot** (the vendor's mark) — Fixed probe X mach pos Unit: mm | ⛔ do not write |
| #636 | **kept after a reboot** (the vendor's mark) — Fixed probe Y mach pos Unit: mm | ⛔ do not write |
| #637 | **kept after a reboot** (the vendor's mark) — Fixed probe Z mach pos Unit: mm | ⛔ do not write |
| #638 | **kept after a reboot** (the vendor's mark) — Fixed probe 4th mach pos Unit: mm | ⛔ do not write |
| #639 | **kept after a reboot** (the vendor's mark) — Fixed probe 5th mach pos Unit: mm | ⛔ do not write |
| #640 | **kept after a reboot** (the vendor's mark) — Retraction distance after probe action is finished Unit: mm — Retraction distance after the end of probe | ⛔ do not write |
| #641-#649 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #650 | **kept after a reboot** (the vendor's mark) — Stop mode when X-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #651 | **kept after a reboot** (the vendor's mark) — Stop mode when Y-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #652 | **kept after a reboot** (the vendor's mark) — Stop mode when Z-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #653 | **kept after a reboot** (the vendor's mark) — Stop mode when 4th-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #654 | **kept after a reboot** (the vendor's mark) — Stop mode when 5th-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #655 | **kept after a reboot** (the vendor's mark) — Enable software limits 0: No 1: Yes | ⛔ do not write |
| #656 | **kept after a reboot** (the vendor's mark) — Stop mode when X-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #657 | **kept after a reboot** (the vendor's mark) — Stop mode when Y-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #658 | **kept after a reboot** (the vendor's mark) — Stop mode when Z-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #659 | **kept after a reboot** (the vendor's mark) — Stop mode when 4th-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #660 | **kept after a reboot** (the vendor's mark) — Stop mode when 5th-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #661 | **kept after a reboot** (the vendor's mark) — X Soft limit of Negative position | ⛔ do not write |
| #662 | **kept after a reboot** (the vendor's mark) — Y Soft limit of negative position | ⛔ do not write |
| #663 | **kept after a reboot** (the vendor's mark) — Z Soft limit of Negative position | ⛔ do not write |
| #664 | **kept after a reboot** (the vendor's mark) — 4th Soft limit of Negative position | ⛔ do not write |
| #665 | **kept after a reboot** (the vendor's mark) — 5th Soft limit of Negative position | ⛔ do not write |
| #666 | **kept after a reboot** (the vendor's mark) — X Soft limit of positive position | ⛔ do not write |
| #667 | **kept after a reboot** (the vendor's mark) — Y Soft limit of positive position | ⛔ do not write |
| #668 | **kept after a reboot** (the vendor's mark) — Z Soft limit of positive position | ⛔ do not write |
| #669 | **kept after a reboot** (the vendor's mark) — 4th Soft limit of Positive position | ⛔ do not write |
| #670 | **kept after a reboot** (the vendor's mark) — 5th Soft limit of Positive position | ⛔ do not write |
| #671 | **kept after a reboot** (the vendor's mark) — MPG precision Mode control mode: 0:Active 1:Disable | ⛔ do not write |
| #672 | **kept after a reboot** (the vendor's mark) — MPG X1 distance 0.001-0.01 | ⛔ do not write |
| #673 | **kept after a reboot** (the vendor's mark) — MPG Estop signal enable 0：Disable 1：Enable | ⛔ do not write |
| #674 | **kept after a reboot** (the vendor's mark) — MPG Estop signal Level define 0：Low 1：High | ⛔ do not write |
| #675 | **kept after a reboot** (the vendor's mark) — MPG working direction 0:Positive 1：Negative | ⛔ do not write |
| #676 | **kept after a reboot** (the vendor's mark) — MPG X1 Working speed | ⛔ do not write |
| #677 | **kept after a reboot** (the vendor's mark) — MPG X10 Working speed | ⛔ do not write |
| #678 | **kept after a reboot** (the vendor's mark) — MPG X100 Working speed | ⛔ do not write |
| #679 | **kept after a reboot** (the vendor's mark) — Magnification adjustment increment value when Handwheel guide stopped | ⛔ do not write |
| #680 | **kept after a reboot** (the vendor's mark) — MPG Acceleration/deceleration rate adjustment increment value | ⛔ do not write |
| #681 | **kept after a reboot** (the vendor's mark) — MPG Movement acceleration of X axis | ⛔ do not write |
| #682 | **kept after a reboot** (the vendor's mark) — MPG Movement acceleration of Y axis | ⛔ do not write |
| #683 | **kept after a reboot** (the vendor's mark) — MPG Movement acceleration of Z axis | ⛔ do not write |
| #684 | **kept after a reboot** (the vendor's mark) — MPG Movement acceleration of 4th axis | ⛔ do not write |
| #685 | **kept after a reboot** (the vendor's mark) — MPG Movement acceleration of 5th axis | ⛔ do not write |
| #686-#689 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #690 | **kept after a reboot** (the vendor's mark) — X axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #691 | **kept after a reboot** (the vendor's mark) — Y axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #692 | **kept after a reboot** (the vendor's mark) — axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #693 | **kept after a reboot** (the vendor's mark) — 4th axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #694 | **kept after a reboot** (the vendor's mark) — 5th axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #695 | **kept after a reboot** (the vendor's mark) — X axis ballscrew Backlash unit: mm | ⛔ do not write |
| #696 | **kept after a reboot** (the vendor's mark) — Y axis ballscrew Backlash unit: mm | ⛔ do not write |
| #697 | **kept after a reboot** (the vendor's mark) — Z axis ballscrew Backlash unit: mm | ⛔ do not write |
| #698 | **kept after a reboot** (the vendor's mark) — 4th axis ballscrew Backlash unit: mm | ⛔ do not write |
| #699 | **kept after a reboot** (the vendor's mark) — 5th axis ballscrew Backlash unit: mm | ⛔ do not write |
| #700 | **kept after a reboot** (the vendor's mark) — Backlash compensation speed 单位:mm/min | ⛔ do not write |
| #701 | **kept after a reboot** (the vendor's mark) — Total number of processed pieces — The total number of completed programs. Upgrades with the completion of M30. Doesn't reset on reboot. | ⛔ do not write |
| #703 | **kept after a reboot** (the vendor's mark) — Number of cyclic processing pieces (Planned number of processed pieces) — Number of repetitions for command M47 | ⛔ do not write |
| #704-#709 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #710 | **kept after a reboot** (the vendor's mark) — K1 key function definition — If you set #710=2536, this will hang the controller when K1 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #711 | **kept after a reboot** (the vendor's mark) — K2 key function definition — If you set #711=2537, this will hang the controller when K2 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #712 | **kept after a reboot** (the vendor's mark) — K3 key function definition — If you set #712=2538, this will hang the controller when K3 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #713 | **kept after a reboot** (the vendor's mark) — K4 key function definition — If you set #713=2539, this will hang the controller when K4 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #714 | **kept after a reboot** (the vendor's mark) — K5 key function definition — If you set #714=2540, this will hang the controller when K5 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #715 | **kept after a reboot** (the vendor's mark) — K6 key function definition — If you set #715=2541, this will hang the controller when K6 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #716 | **kept after a reboot** (the vendor's mark) — K7 key function definition — If you set #716=2541, this will hang the controller when K7 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #717 | **kept after a reboot** (the vendor's mark) — K8 key function definition — If you set #717=2542, this will hang the controller when K8 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #718 | **kept after a reboot** (the vendor's mark) — K9 key function definition — If you set #718=2543, this will hang the controller when K9 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #719 | **kept after a reboot** (the vendor's mark) — K10 key function definition — If you set #719=2544, this will hang the controller when K10 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #720 | **kept after a reboot** (the vendor's mark) — Go Home Before Processing? 0：No 1：Yes | ⛔ do not write |
| #721 | **kept after a reboot** (the vendor's mark) — References speed when working on radius 5mm Arc | ⛔ do not write |
| #722 | **kept after a reboot** (the vendor's mark) — The 4th Axis processing protection speed | ⛔ do not write |
| #723 | **kept after a reboot** (the vendor's mark) — The 5th Axis processing protection speed | ⛔ do not write |
| #724 | **kept after a reboot** (the vendor's mark) — Set the Drilling cycle Retraction amount | ⛔ do not write |
| #730 | **kept after a reboot** (the vendor's mark) — After processing finished — the machine: 0：Don't move 1：Back to References Point 2：Back to Zero | ⛔ do not write |
| #731 | **kept after a reboot** (the vendor's mark) — K11 key function definition — If #731=2545 is set, this will hang the controller when K11 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #732 | **kept after a reboot** (the vendor's mark) — K12 key function definition — If you set #732=2546, this will hang the controller when K12 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #733 | **kept after a reboot** (the vendor's mark) — K13 key function definition — If you set #733=2547, this will hang the controller when K13 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #734 | **kept after a reboot** (the vendor's mark) — K14 key function definition — If you set #734=2548, this will hang the controller when K14 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #735 | **kept after a reboot** (the vendor's mark) — Home offset of X axis | ⛔ do not write |
| #736 | **kept after a reboot** (the vendor's mark) — Home offset of Y axis | ⛔ do not write |
| #737 | **kept after a reboot** (the vendor's mark) — Home offset of Z axis | ⛔ do not write |
| #738 | **kept after a reboot** (the vendor's mark) — Home offset of 4th axis | ⛔ do not write |
| #739 | **kept after a reboot** (the vendor's mark) — Home offset of 5th axis | ⛔ do not write |
| #740 | **kept after a reboot** (the vendor's mark) — Language Setting 0:English 1:Chinese 2:Russian | ⛔ do not write |
| #741 | **kept after a reboot** (the vendor's mark) — Buzzer enable 0:Forbit 1:Enable | ⛔ do not write |
| #743 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #744 | **kept after a reboot** (the vendor's mark) — Toolpath Realtime Display 0 :Forbit 1:Enable | ⛔ do not write |
| #745 | **kept after a reboot** (the vendor's mark) — Toolpath display mode 0:Status 1:Line 2: 3D | ⛔ do not write |
| #747 | **kept after a reboot** (the vendor's mark) — Interpolation period Second 0.001-0.01 | ⛔ do not write |
| #748 | **kept after a reboot** (the vendor's mark) — User self-design LOGO Display time | ⛔ do not write |
| #751 | **kept after a reboot** (the vendor's mark) — K15 key function definition — If you set #751=2549, this will hang the controller when K15 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #752 | **kept after a reboot** (the vendor's mark) — K16 key function definition — If you set #752=2550, this will hang the controller when K16 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #753 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #764 | **kept after a reboot** (the vendor's mark) — Alarm Enable output configuration 01-16 | ⛔ do not write |
| #765 | **kept after a reboot** (the vendor's mark) — Alarm Enable output configuration 17-32 | ⛔ do not write |
| #766 | **kept after a reboot** (the vendor's mark) — Serial 1 baud rate | ⛔ do not write |
| #767 | **kept after a reboot** (the vendor's mark) — Serial 2 baud rate | ⛔ do not write |
| #768 | **kept after a reboot** (the vendor's mark) — External keyboard type | ⛔ do not write |
| #769 | **kept after a reboot** (the vendor's mark) — Debug printing enable — Enables feature #1503 - Prompt information | ⛔ do not write |
| #771 | **kept after a reboot** (the vendor's mark) — maximum contour | ⛔ do not write |
| #772 | **kept after a reboot** (the vendor's mark) — extern key 1 Function | ⛔ do not write |
| #773 | **kept after a reboot** (the vendor's mark) — extern key 2 Function | ⛔ do not write |
| #774 | **kept after a reboot** (the vendor's mark) — external key 3 Function | ⛔ do not write |
| #775 | **kept after a reboot** (the vendor's mark) — extern key 4 Function | ⛔ do not write |
| #776 | **kept after a reboot** (the vendor's mark) — extern key 5 Function | ⛔ do not write |
| #777 | **kept after a reboot** (the vendor's mark) — extern key 6 Function | ⛔ do not write |
| #778 | **kept after a reboot** (the vendor's mark) — USB keyboard type 0：Close 1：Standard keyboard 2：USB Scanner | ⛔ do not write |
| #779 | **kept after a reboot** (the vendor's mark) — Modbus RTU (The Barcode processing file storage) | ⛔ do not write |
| #780 | **kept after a reboot** (the vendor's mark) — Line corner acceleration | ⛔ do not write |
| #781 | **kept after a reboot** (the vendor's mark) — J parameter of S-type acceleration curve | ⛔ do not write |
| #782 | **kept after a reboot** (the vendor's mark) — G00 Acceleration | ⛔ do not write |
| #783 | **kept after a reboot** (the vendor's mark) — Barcode scanning processing enable | ⛔ do not write |
| #784 | **kept after a reboot** (the vendor's mark) — network boot mode | ⛔ do not write |
| #785 | **kept after a reboot** (the vendor's mark) — X-axis max. Acceleration G00 | ⛔ do not write |
| #786 | **kept after a reboot** (the vendor's mark) — Y axis max. Acceleration G00 | ⛔ do not write |
| #787 | **kept after a reboot** (the vendor's mark) — Z axis max. Acceleration G00 | ⛔ do not write |
| #788 | **kept after a reboot** (the vendor's mark) — 4th axis max. Acceleration G00 | ⛔ do not write |
| #789 | **kept after a reboot** (the vendor's mark) — 5th axis max. Acceleration G00 | ⛔ do not write |
| #795-#799 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #800 | **kept after a reboot** (the vendor's mark) — G92 Coordinates Home Offset X — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #801 | **kept after a reboot** (the vendor's mark) — G92 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #802 | **kept after a reboot** (the vendor's mark) — G92 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #803 | **kept after a reboot** (the vendor's mark) — G92 Coordinates Home Offset A — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #804 | **kept after a reboot** (the vendor's mark) — G92 Coordinates Home Offset B — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #805 | **kept after a reboot** (the vendor's mark) — G54 Coordinates Home Offset X — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #806 | **kept after a reboot** (the vendor's mark) — G54 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #807 | **kept after a reboot** (the vendor's mark) — G54 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #808 | **kept after a reboot** (the vendor's mark) — G54 Coordinates Home Offset A — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #809 | **kept after a reboot** (the vendor's mark) — G54 Coordinates Home Offset B — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #810 | **kept after a reboot** (the vendor's mark) — G55 Coordinates Home Offset X — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #811 | **kept after a reboot** (the vendor's mark) — G55 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #812 | **kept after a reboot** (the vendor's mark) — G55 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #813 | **kept after a reboot** (the vendor's mark) — G55 Coordinates Home Offset A — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #814 | **kept after a reboot** (the vendor's mark) — G55 Coordinates Home Offset B — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #815 | **kept after a reboot** (the vendor's mark) — G56 Coordinates Home Offset X — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #816 | **kept after a reboot** (the vendor's mark) — G56 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #817 | **kept after a reboot** (the vendor's mark) — G56 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #818 | **kept after a reboot** (the vendor's mark) — G56 Coordinates Home Offset A — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #819 | **kept after a reboot** (the vendor's mark) — G56 Coordinates Home Offset B — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #820 | **kept after a reboot** (the vendor's mark) — G57 Coordinates Home Offset X — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #821 | **kept after a reboot** (the vendor's mark) — G57 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #822 | **kept after a reboot** (the vendor's mark) — G57 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #823 | **kept after a reboot** (the vendor's mark) — G57 Coordinates Home Offset A — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #824 | **kept after a reboot** (the vendor's mark) — G57 Coordinates Home Offset B — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #825 | **kept after a reboot** (the vendor's mark) — G58 Coordinates Home Offset X — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #826 | **kept after a reboot** (the vendor's mark) — G58 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #827 | **kept after a reboot** (the vendor's mark) — G58 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #828 | **kept after a reboot** (the vendor's mark) — G58 Coordinates Home Offset A — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #829 | **kept after a reboot** (the vendor's mark) — G58 Coordinates Home Offset B — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #830 | **kept after a reboot** (the vendor's mark) — G59 Coordinates Home Offset X — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #831 | **kept after a reboot** (the vendor's mark) — G59 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #832 | **kept after a reboot** (the vendor's mark) — G59 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #833 | **kept after a reboot** (the vendor's mark) — G59 Coordinates Home Offset A — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #834 | **kept after a reboot** (the vendor's mark) — G59 Coordinates Home Offset B — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #840 | **kept after a reboot** (the vendor's mark) — X-axis Coordinate offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #841 | **kept after a reboot** (the vendor's mark) — Y-axis Coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #842 | **kept after a reboot** (the vendor's mark) — Z axis coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #843 | **kept after a reboot** (the vendor's mark) — 4th axis coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #844 | **kept after a reboot** (the vendor's mark) — 5th axis Coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #845-#849 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #890-#899 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #900 | **kept after a reboot** (the vendor's mark) — H01 offset mm | ⛔ do not write, measured |
| #901 | **kept after a reboot** (the vendor's mark) — H02 offset mm | ⛔ do not write, measured |
| #902 | **kept after a reboot** (the vendor's mark) — H03 offset mm | ⛔ do not write, measured |
| #903 | **kept after a reboot** (the vendor's mark) — H04 offset mm | ⛔ do not write, measured |
| #904 | **kept after a reboot** (the vendor's mark) — H05 offset mm | ⛔ do not write, measured |
| #905 | **kept after a reboot** (the vendor's mark) — H06 offset mm | ⛔ do not write, measured |
| #906 | **kept after a reboot** (the vendor's mark) — H07 offset mm | ⛔ do not write, measured |
| #907 | **kept after a reboot** (the vendor's mark) — H08 offset mm | ⛔ do not write, measured |
| #908 | **kept after a reboot** (the vendor's mark) — H09 offset mm | ⛔ do not write, measured |
| #909 | **kept after a reboot** (the vendor's mark) — H10 offset mm | ⛔ do not write, measured |
| #910 | **kept after a reboot** (the vendor's mark) — H11 offset mm | ⛔ do not write, measured |
| #911 | **kept after a reboot** (the vendor's mark) — H12 offset mm | ⛔ do not write, measured |
| #912 | **kept after a reboot** (the vendor's mark) — H13 offset mm | ⛔ do not write, measured |
| #913 | **kept after a reboot** (the vendor's mark) — H14 offset mm | ⛔ do not write, measured |
| #914 | **kept after a reboot** (the vendor's mark) — H15 offset mm | ⛔ do not write, measured |
| #915 | **kept after a reboot** (the vendor's mark) — H16 offset mm | ⛔ do not write, measured |
| #916 | **kept after a reboot** (the vendor's mark) — Possibly H17 offset mm | ⛔ do not write |
| #917 | **kept after a reboot** (the vendor's mark) — Possibly H18 offset mm | ⛔ do not write |
| #918 | **kept after a reboot** (the vendor's mark) — Possibly H19 offset mm | ⛔ do not write |
| #919 | **kept after a reboot** (the vendor's mark) — Possibly H20 offset mm | ⛔ do not write |
| #920 | **kept after a reboot** (the vendor's mark) — D01 offset mm | ⛔ do not write |
| #921 | **kept after a reboot** (the vendor's mark) — D02 offset mm | ⛔ do not write |
| #922 | **kept after a reboot** (the vendor's mark) — D03 offset mm | ⛔ do not write |
| #923 | **kept after a reboot** (the vendor's mark) — D04 offset mm | ⛔ do not write |
| #924 | **kept after a reboot** (the vendor's mark) — D05 offset mm | ⛔ do not write |
| #925 | **kept after a reboot** (the vendor's mark) — D06 offset mm | ⛔ do not write |
| #926 | **kept after a reboot** (the vendor's mark) — D07 offset mm | ⛔ do not write |
| #927 | **kept after a reboot** (the vendor's mark) — D08 offset mm | ⛔ do not write |
| #928 | **kept after a reboot** (the vendor's mark) — D09 offset mm | ⛔ do not write |
| #929 | **kept after a reboot** (the vendor's mark) — D10 offset mm | ⛔ do not write |
| #930 | **kept after a reboot** (the vendor's mark) — D11 offset mm | ⛔ do not write |
| #931 | **kept after a reboot** (the vendor's mark) — D12 offset mm | ⛔ do not write |
| #932 | **kept after a reboot** (the vendor's mark) — D13 offset mm | ⛔ do not write |
| #933 | **kept after a reboot** (the vendor's mark) — D14 offset mm | ⛔ do not write |
| #934 | **kept after a reboot** (the vendor's mark) — D15 offset mm | ⛔ do not write |
| #935 | **kept after a reboot** (the vendor's mark) — D16 offset mm | ⛔ do not write |
| #936 | **kept after a reboot** (the vendor's mark) — Possibly D17 offset mm | ⛔ do not write |
| #937 | **kept after a reboot** (the vendor's mark) — Possibly D18 offset mm | ⛔ do not write |
| #938 | **kept after a reboot** (the vendor's mark) — Possibly D19 offset mm | ⛔ do not write |
| #939 | **kept after a reboot** (the vendor's mark) — Possibly D20 offset mm | ⛔ do not write |
| #943 | **kept after a reboot** (the vendor's mark) — 4th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #944 | **kept after a reboot** (the vendor's mark) — 5th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #945 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #949 | **kept after a reboot** (the vendor's mark) — 4th axis type 0: Linear axis; 1: Rotation axis | ⛔ do not write |
| #950 | **kept after a reboot** (the vendor's mark) — 5th axis type 0: Linear axis; 1: Rotation axis | ⛔ do not write |
| #951-#954 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #955 | **kept after a reboot** (the vendor's mark) — 4th axis Offset X | ⛔ do not write |
| #956 | **kept after a reboot** (the vendor's mark) — 5th-axis Offset X | ⛔ do not write |
| #957-#960 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #961 | **kept after a reboot** (the vendor's mark) — 4th-axis Offset Y | ⛔ do not write |
| #962 | **kept after a reboot** (the vendor's mark) — 5th axis Offset Y | ⛔ do not write |
| #963-#966 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #967 | **kept after a reboot** (the vendor's mark) — 4th axis Offset Z | ⛔ do not write |
| #968 | **kept after a reboot** (the vendor's mark) — 5th axis Offset Z | ⛔ do not write |
| #969-#972 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #973 | **kept after a reboot** (the vendor's mark) — 4th-axis Vector U | ⛔ do not write |
| #974 | **kept after a reboot** (the vendor's mark) — 5th-axis Vector U | ⛔ do not write |
| #975-#978 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #979 | **kept after a reboot** (the vendor's mark) — 4th axis Vector V | ⛔ do not write |
| #980 | **kept after a reboot** (the vendor's mark) — 5th axis Vector V | ⛔ do not write |
| #981-#984 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #985 | **kept after a reboot** (the vendor's mark) — 4th-axis Vector W | ⛔ do not write |
| #986 | **kept after a reboot** (the vendor's mark) — 5th-axis Vector W | ⛔ do not write |
| #987 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #988 | **kept after a reboot** (the vendor's mark) — Programming axis of physical axis 1 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #989 | **kept after a reboot** (the vendor's mark) — Programming axis of physical axis 2 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #990 | **kept after a reboot** (the vendor's mark) — Programming axis of physical axis 3 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #991 | **kept after a reboot** (the vendor's mark) — Programming axis of physical axis 4 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #992 | **kept after a reboot** (the vendor's mark) — Programming axis of physical axis 5 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #993-#999 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1000 | **kept after a reboot** (the vendor's mark) — X-axis servo alarm signal | ⛔ do not write |
| #1001 | **kept after a reboot** (the vendor's mark) — X axis servor Alarm signal Enable | ⛔ do not write |
| #1002 | **kept after a reboot** (the vendor's mark) — axis servo Alarm signal electronic level | ⛔ do not write |
| #1003 | **kept after a reboot** (the vendor's mark) — Y-axis servo alarm signal | ⛔ do not write |
| #1004 | **kept after a reboot** (the vendor's mark) — Y axis servo Alarm signal Enable | ⛔ do not write |
| #1005 | **kept after a reboot** (the vendor's mark) — Y axis servo Alarm signal electronic level | ⛔ do not write |
| #1006 | **kept after a reboot** (the vendor's mark) — Z-axis servo alarm signal | ⛔ do not write |
| #1007 | **kept after a reboot** (the vendor's mark) — Z axis servo Alarm signal enable | ⛔ do not write |
| #1008 | **kept after a reboot** (the vendor's mark) — Z axis servo Alarm signal electronic level | ⛔ do not write |
| #1009 | **kept after a reboot** (the vendor's mark) — 4th axis servo Alarm signal Input Signal (Spindle alarm signal) | ⛔ do not write |
| #1010 | **kept after a reboot** (the vendor's mark) — 4th axis servo Alarm signal Enable | ⛔ do not write |
| #1011 | **kept after a reboot** (the vendor's mark) — 4th axis servo Alarm signal electronic level | ⛔ do not write |
| #1012 | **kept after a reboot** (the vendor's mark) — 5th axis servo alarm signal | ⛔ do not write |
| #1013 | **kept after a reboot** (the vendor's mark) — 5th axis servo Alarm signal Enable | ⛔ do not write |
| #1014 | **kept after a reboot** (the vendor's mark) — 5th axis servo Alarm signal electronic level | ⛔ do not write |
| #1015 | **kept after a reboot** (the vendor's mark) — Negative X-axis hard limit signal | ⛔ do not write |
| #1016 | **kept after a reboot** (the vendor's mark) — Negative X-axis hard limit signal ENABLE | ⛔ do not write |
| #1017 | **kept after a reboot** (the vendor's mark) — Negative X-axis hard limit signal Effective level | ⛔ do not write |
| #1018 | **kept after a reboot** (the vendor's mark) — Negative Y-axis hard limit signal | ⛔ do not write |
| #1019 | **kept after a reboot** (the vendor's mark) — Negative Y-axis hard limit signal ENABLE | ⛔ do not write |
| #1020 | **kept after a reboot** (the vendor's mark) — Negative Y-axis hard limit signal Effective level | ⛔ do not write |
| #1021 | **kept after a reboot** (the vendor's mark) — Negative Z-axis hard limit signal | ⛔ do not write |
| #1022 | **kept after a reboot** (the vendor's mark) — Negative Z-axis hard limit signal ENABLE | ⛔ do not write |
| #1023 | **kept after a reboot** (the vendor's mark) — Negative Z-axis hard limit signal Effective level | ⛔ do not write |
| #1024 | **kept after a reboot** (the vendor's mark) — Negative 4th-axis hard limit signal | ⛔ do not write |
| #1025 | **kept after a reboot** (the vendor's mark) — Negative 4th-axis hard limit signal ENABLE | ⛔ do not write |
| #1026 | **kept after a reboot** (the vendor's mark) — Negative 4th-axis hard limit signal Effective level | ⛔ do not write |
| #1027 | **kept after a reboot** (the vendor's mark) — Negative 5th axis hard limit signal | ⛔ do not write |
| #1028 | **kept after a reboot** (the vendor's mark) — Negative 5th-axis hard limit signal ENABLE | ⛔ do not write |
| #1029 | **kept after a reboot** (the vendor's mark) — Negative 5th-axis hard limit signal Effective level | ⛔ do not write |
| #1030 | **kept after a reboot** (the vendor's mark) — Positive X-axis hard limit signal | ⛔ do not write |
| #1031 | **kept after a reboot** (the vendor's mark) — Positive X-axis hard limit signal ENABLE | ⛔ do not write |
| #1032 | **kept after a reboot** (the vendor's mark) — Positive X-axis hard limit signal Effective level | ⛔ do not write |
| #1033 | **kept after a reboot** (the vendor's mark) — Positive Y-axis hard limit signal | ⛔ do not write |
| #1034 | **kept after a reboot** (the vendor's mark) — Positive Y-axis hard limit signal ENABLE | ⛔ do not write |
| #1035 | **kept after a reboot** (the vendor's mark) — Positive Y-axis hard limit signal Effective level | ⛔ do not write |
| #1036 | **kept after a reboot** (the vendor's mark) — Positive Z-axis hard limit signal | ⛔ do not write |
| #1037 | **kept after a reboot** (the vendor's mark) — Positive Z-axis hard limit signal ENABLE | ⛔ do not write |
| #1038 | **kept after a reboot** (the vendor's mark) — Positive Z-axis hard limit signal Effective level | ⛔ do not write |
| #1039 | **kept after a reboot** (the vendor's mark) — Positive 4th axis hard limit signal | ⛔ do not write |
| #1040 | **kept after a reboot** (the vendor's mark) — Positive 4th-axis hard limit signal ENABLE | ⛔ do not write |
| #1041 | **kept after a reboot** (the vendor's mark) — Positive 4th-axis hard limit signal Effective level | ⛔ do not write |
| #1042 | **kept after a reboot** (the vendor's mark) — Positive 5th axis hard limit signal | ⛔ do not write |
| #1043 | **kept after a reboot** (the vendor's mark) — Positive 5th-axis hard limit signal ENABLE | ⛔ do not write |
| #1044 | **kept after a reboot** (the vendor's mark) — Positive 5th-axis hard limit signal Effective level | ⛔ do not write |
| #1045 | **kept after a reboot** (the vendor's mark) — X-axis zero signal | ⛔ do not write |
| #1046 | **kept after a reboot** (the vendor's mark) — X-axis zero signal ENABLE | ⛔ do not write |
| #1047 | **kept after a reboot** (the vendor's mark) — X-axis zero signal effective level | ⛔ do not write |
| #1048 | **kept after a reboot** (the vendor's mark) — Y-axis zero signal | ⛔ do not write |
| #1049 | **kept after a reboot** (the vendor's mark) — Y-axis zero signal ENABLE | ⛔ do not write |
| #1050 | **kept after a reboot** (the vendor's mark) — Y-axis zero signal effective level | ⛔ do not write |
| #1051 | **kept after a reboot** (the vendor's mark) — Z-axis zero signal | ⛔ do not write |
| #1052 | **kept after a reboot** (the vendor's mark) — Z-axis zero signal ENABLE | ⛔ do not write |
| #1053 | **kept after a reboot** (the vendor's mark) — Z-axis zero signal effective level | ⛔ do not write |
| #1054 | **kept after a reboot** (the vendor's mark) — 4th axis zero signal | ⛔ do not write |
| #1055 | **kept after a reboot** (the vendor's mark) — 4th axis zero signal ENABLE | ⛔ do not write |
| #1056 | **kept after a reboot** (the vendor's mark) — 4th axis zero signal effective level | ⛔ do not write |
| #1057 | **kept after a reboot** (the vendor's mark) — 5th axis zero signal | ⛔ do not write |
| #1058 | **kept after a reboot** (the vendor's mark) — 5th axis zero signal ENABLE | ⛔ do not write |
| #1059 | **kept after a reboot** (the vendor's mark) — 5th axis zero signal effective level | ⛔ do not write |
| #1060-#1074 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1075 | **kept after a reboot** (the vendor's mark) — Fixed probe signal | ⛔ do not write |
| #1076 | **kept after a reboot** (the vendor's mark) — Fixed probe signal ENABLE | ⛔ do not write |
| #1077 | **kept after a reboot** (the vendor's mark) — Fixed probe signal effective level | ⛔ do not write |
| #1078 | **kept after a reboot** (the vendor's mark) — Floating probe signal | ⛔ do not write |
| #1079 | **kept after a reboot** (the vendor's mark) — Floating Probe signal ENABLE | ⛔ do not write |
| #1080 | **kept after a reboot** (the vendor's mark) — Floating Probe signal effective level | ⛔ do not write |
| #1081 | **kept after a reboot** (the vendor's mark) — External Key 1 Signal | ⛔ do not write |
| #1082 | **kept after a reboot** (the vendor's mark) — External Key 1 Enable | ⛔ do not write |
| #1083 | **kept after a reboot** (the vendor's mark) — External Key 1 effective level | ⛔ do not write |
| #1084 | **kept after a reboot** (the vendor's mark) — External Key 2Signal | ⛔ do not write |
| #1085 | **kept after a reboot** (the vendor's mark) — External Key 2 Enable | ⛔ do not write |
| #1086 | **kept after a reboot** (the vendor's mark) — External Key 2 effective level | ⛔ do not write |
| #1087 | **kept after a reboot** (the vendor's mark) — External Key 3 Signal | ⛔ do not write |
| #1088 | **kept after a reboot** (the vendor's mark) — External Key 3 Enable | ⛔ do not write |
| #1089 | **kept after a reboot** (the vendor's mark) — External Key 3 effective level | ⛔ do not write |
| #1090 | **kept after a reboot** (the vendor's mark) — External Key 4Signal | ⛔ do not write |
| #1091 | **kept after a reboot** (the vendor's mark) — External Key 4 Enable | ⛔ do not write |
| #1092 | **kept after a reboot** (the vendor's mark) — External Key 4 effective level | ⛔ do not write |
| #1093 | **kept after a reboot** (the vendor's mark) — External Key 5 Signal | ⛔ do not write |
| #1094 | **kept after a reboot** (the vendor's mark) — External Key 5 Enable | ⛔ do not write |
| #1095 | **kept after a reboot** (the vendor's mark) — External Key 5 effective level | ⛔ do not write |
| #1096 | **kept after a reboot** (the vendor's mark) — External Key 6Signal | ⛔ do not write |
| #1097 | **kept after a reboot** (the vendor's mark) — External Key 6 Enable | ⛔ do not write |
| #1098 | **kept after a reboot** (the vendor's mark) — External Key 6 effective level | ⛔ do not write |
| #1099 | **kept after a reboot** (the vendor's mark) — External start signal Signal | ⛔ do not write |
| #1100 | **kept after a reboot** (the vendor's mark) — External start signal enable | ⛔ do not write |
| #1101 | **kept after a reboot** (the vendor's mark) — External Start signal Effective level | ⛔ do not write |
| #1102 | **kept after a reboot** (the vendor's mark) — External Pause Signal | ⛔ do not write |
| #1103 | **kept after a reboot** (the vendor's mark) — External Pause Signal Enable | ⛔ do not write |
| #1104 | **kept after a reboot** (the vendor's mark) — External Pause signal Effective level | ⛔ do not write |
| #1105 | **kept after a reboot** (the vendor's mark) — External Estop signal Signal | ⛔ do not write |
| #1106 | **kept after a reboot** (the vendor's mark) — External Estop Signal Enable | ⛔ do not write |
| #1107 | **kept after a reboot** (the vendor's mark) — External Estop signal Effective level | ⛔ do not write |
| #1108-#1119 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1120 | **kept after a reboot** (the vendor's mark) — Spindle stop signal(M300) Port Number | ⛔ do not write |
| #1121 | **kept after a reboot** (the vendor's mark) — Spindle Stop detection Enable | ⛔ do not write |
| #1122 | **kept after a reboot** (the vendor's mark) — Spindle Stop detection effective level | ⛔ do not write |
| #1123 | **kept after a reboot** (the vendor's mark) — Tool release input signal port number | ⛔ do not write |
| #1124 | **kept after a reboot** (the vendor's mark) — Tool Release Detection Enable | ⛔ do not write |
| #1125 | **kept after a reboot** (the vendor's mark) — Tool release detection effective level | ⛔ do not write |
| #1126 | **kept after a reboot** (the vendor's mark) — Tool lock input signal(M302) Port Number | ⛔ do not write |
| #1127 | **kept after a reboot** (the vendor's mark) — Tool lock detection signal Enable | ⛔ do not write |
| #1128 | **kept after a reboot** (the vendor's mark) — Tool lock detection signal effective level | ⛔ do not write |
| #1129 | **kept after a reboot** (the vendor's mark) — Tool open input signal(M303) Port Number | ⛔ do not write |
| #1130 | **kept after a reboot** (the vendor's mark) — Tool open detection signal Enable | ⛔ do not write |
| #1131 | **kept after a reboot** (the vendor's mark) — Tool open detection effective level | ⛔ do not write |
| #1132 | **kept after a reboot** (the vendor's mark) — Dust cover open/close input signal(M305/M306) Port Number | ⛔ do not write |
| #1133 | **kept after a reboot** (the vendor's mark) — Dust cover open/close signal enable | ⛔ do not write |
| #1134 | **kept after a reboot** (the vendor's mark) — Dust cover open/close signal effective level | ⛔ do not write |
| #1135 | **kept after a reboot** (the vendor's mark) — Inverter alarm input signal port Number | ⛔ do not write |
| #1136 | **kept after a reboot** (the vendor's mark) — Inverter alarm input signal enable | ⛔ do not write |
| #1137 | **kept after a reboot** (the vendor's mark) — Inverter alarm input signal effective level | ⛔ do not write |
| #1138 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 1 detection input Port | ⛔ do not write |
| #1139 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 1 ENABLE | ⛔ do not write |
| #1140 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 1 Effective level | ⛔ do not write |
| #1141 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 2 detection input Port | ⛔ do not write |
| #1142 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 2 ENABLE | ⛔ do not write |
| #1143 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 2 Effective level | ⛔ do not write |
| #1144 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 3 detection input Port | ⛔ do not write |
| #1145 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 3 ENABLE | ⛔ do not write |
| #1146 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 3 Effective level | ⛔ do not write |
| #1147 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 4 detection input Port | ⛔ do not write |
| #1148 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 4 ENABLE | ⛔ do not write |
| #1149 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 4 Effective level | ⛔ do not write |
| #1150 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 5 detection input Port | ⛔ do not write |
| #1151 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 5 ENABLE | ⛔ do not write |
| #1152 | **kept after a reboot** (the vendor's mark) — Self-define Alarm signal 5 Effective level | ⛔ do not write |
| #1153 | **kept after a reboot** (the vendor's mark) — User storage — Safe park X position | ⛔ do not write |
| #1154 | **kept after a reboot** (the vendor's mark) — User storage — Safe park Y position | ⛔ do not write |
| #1155 | **kept after a reboot** (the vendor's mark) — User storage — Tool change park X position | ⛔ do not write |
| #1156 | **kept after a reboot** (the vendor's mark) — User storage — Tool change park Y position | ⛔ do not write |
| #1157-#1169 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1170-#1175 | **kept after a reboot** (the vendor's mark) — User storage — Probe config | ⛔ do not write |
| #1176-#1193 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1194 | **kept after a reboot** (the vendor's mark) — Input port for M307 command | ⛔ do not write |
| #1195 | **kept after a reboot** (the vendor's mark) — Enable input for command M307 | ⛔ do not write |
| #1196 | **kept after a reboot** (the vendor's mark) — Active level for command M307 | ⛔ do not write |
| #1197 | **kept after a reboot** (the vendor's mark) — Tool close input signal(M304) Port Number | ⛔ do not write |
| #1198 | **kept after a reboot** (the vendor's mark) — Tool close input signal Enable | ⛔ do not write |
| #1199 | **kept after a reboot** (the vendor's mark) — Tool close input signal Effective level | ⛔ do not write |
| #1200-#1214 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1215 | **kept after a reboot** (the vendor's mark) — Spindle Forward Rotation Signal Port Number | ⛔ do not write |
| #1216 | **kept after a reboot** (the vendor's mark) — Spindle forward signal ENABLE | ⛔ do not write |
| #1217 | **kept after a reboot** (the vendor's mark) — Spindle forward signal effective level | ⛔ do not write |
| #1218 | **kept after a reboot** (the vendor's mark) — Spindle Reverse Rotation Signal Port Number | ⛔ do not write |
| #1219 | **kept after a reboot** (the vendor's mark) — Spindle Reverse signal ENABLE | ⛔ do not write |
| #1220 | **kept after a reboot** (the vendor's mark) — Spindle Reverse signal effective level | ⛔ do not write |
| #1221 | **kept after a reboot** (the vendor's mark) — Spindle speed section 1 signal Port Number | ⛔ do not write |
| #1222 | **kept after a reboot** (the vendor's mark) — Spindle speed section 1 signal enable | ⛔ do not write |
| #1223 | **kept after a reboot** (the vendor's mark) — Spindle speed section 1 signal Effective level | ⛔ do not write |
| #1224 | **kept after a reboot** (the vendor's mark) — Spindle speed section 2 signal Port Number | ⛔ do not write |
| #1225 | **kept after a reboot** (the vendor's mark) — Spindle speed section 2 signal enable | ⛔ do not write |
| #1226 | **kept after a reboot** (the vendor's mark) — Spindle speed section 2 signal Effective level | ⛔ do not write |
| #1227 | **kept after a reboot** (the vendor's mark) — Spindle speed section 3 signal Port Number | ⛔ do not write |
| #1228 | **kept after a reboot** (the vendor's mark) — Spindle speed section 3 signal enable | ⛔ do not write |
| #1229 | **kept after a reboot** (the vendor's mark) — Spindle speed section 3 signal Effective level | ⛔ do not write |
| #1230 | **kept after a reboot** (the vendor's mark) — Cooling M8/M9 control signal Port Number | ⛔ do not write |
| #1231 | **kept after a reboot** (the vendor's mark) — Cooling M8/M9 control signal enable | ⛔ do not write |
| #1232 | **kept after a reboot** (the vendor's mark) — Cooling M8/M9 control signal Effective level | ⛔ do not write |
| #1233 | **kept after a reboot** (the vendor's mark) — Lubrication M10/M11 control signal Port Number | ⛔ do not write |
| #1234 | **kept after a reboot** (the vendor's mark) — Lubrication M10/M11 control signal enable | ⛔ do not write |
| #1235 | **kept after a reboot** (the vendor's mark) — Lubrication M10/M11 control signal Effective level | ⛔ do not write |
| #1236 | **kept after a reboot** (the vendor's mark) — System alarm signal Output Port Number | ⛔ do not write |
| #1237 | **kept after a reboot** (the vendor's mark) — System alarm signal Output Enable | ⛔ do not write |
| #1238 | **kept after a reboot** (the vendor's mark) — System alarm signal Output Effective level | ⛔ do not write |
| #1239 | **kept after a reboot** (the vendor's mark) — System working indicator signal output port number | ⛔ do not write |
| #1240 | **kept after a reboot** (the vendor's mark) — System working indicator signal output Enable | ⛔ do not write |
| #1241 | **kept after a reboot** (the vendor's mark) — System working indicator signal output effective level | ⛔ do not write |
| #1242 | **kept after a reboot** (the vendor's mark) — System brake signal output port number — Electromagnetic axle brake (e.g. to prevent Z from dropping) | ⛔ do not write |
| #1243 | **kept after a reboot** (the vendor's mark) — System brake signal output Enable — Electromagnetic axle brake (e.g. to prevent Z from dropping) | ⛔ do not write |
| #1244 | **kept after a reboot** (the vendor's mark) — System brake signal output Effective level — Electromagnetic axle brake (e.g. to prevent Z from dropping) | ⛔ do not write |
| #1245 | **kept after a reboot** (the vendor's mark) — System ready indicator signal output port number | ⛔ do not write |
| #1246 | **kept after a reboot** (the vendor's mark) — System ready indicator signal output Enable | ⛔ do not write |
| #1247 | **kept after a reboot** (the vendor's mark) — System ready indicator signal output Effective level | ⛔ do not write |
| #1248-#1249 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1253 | **kept after a reboot** (the vendor's mark) — Tool launch/retract signal(M152/M153) output port number | ⛔ do not write |
| #1254 | **kept after a reboot** (the vendor's mark) — Tool launch/retract signal(M152/M153) output Enable | ⛔ do not write |
| #1255 | **kept after a reboot** (the vendor's mark) — Tool launch/retract signal(M152/M153) output Effective level | ⛔ do not write |
| #1256 | **kept after a reboot** (the vendor's mark) — Front positioning on/off signal(M156/M157) output port number | ⛔ do not write |
| #1257 | **kept after a reboot** (the vendor's mark) — Front positioning on/off signal(M156/M157) output Enable | ⛔ do not write |
| #1258 | **kept after a reboot** (the vendor's mark) — Front positioning on/off signal(M156/M157) output Effective level | ⛔ do not write |
| #1259 | **kept after a reboot** (the vendor's mark) — Vacuum pump on/off output signal(M158/M159) output port number | ⛔ do not write |
| #1260 | **kept after a reboot** (the vendor's mark) — Vacuum pump on/off output signal(M158/M159) output enable | ⛔ do not write |
| #1261 | **kept after a reboot** (the vendor's mark) — Vacuum pump on/off output signal(M158/M159) output Effective level | ⛔ do not write |
| #1262 | **kept after a reboot** (the vendor's mark) — Dust cover open/close output signal (M150/M151) output port number | ⛔ do not write |
| #1263 | **kept after a reboot** (the vendor's mark) — Dust cover open/close output signal (M150/M151) output Enable | ⛔ do not write |
| #1264 | **kept after a reboot** (the vendor's mark) — Dust cover open/close output signal (M150/M151) output Effective level | ⛔ do not write |
| #1265 | **kept after a reboot** (the vendor's mark) — Push cylinder open/close signal(M160/M161) output port number | ⛔ do not write |
| #1266 | **kept after a reboot** (the vendor's mark) — Push cylinder open/close signal(M160/M161) output Enable | ⛔ do not write |
| #1267 | **kept after a reboot** (the vendor's mark) — Push cylinder open/close signal(M160/M161) output Effective level | ⛔ do not write |
| #1268 | **kept after a reboot** (the vendor's mark) — Vacuum cleaner on/off signal (M162/M163) output port number | ⛔ do not write |
| #1269 | **kept after a reboot** (the vendor's mark) — Vacuum cleaner on/off signal (M162/M163) output Enable | ⛔ do not write |
| #1270 | **kept after a reboot** (the vendor's mark) — Vacuum cleaner on/off signal (M162/M163) output Effective level | ⛔ do not write |
| #1271 | **kept after a reboot** (the vendor's mark) — Left positioning on/off signal(M164/M165) output port number | ⛔ do not write |
| #1272 | **kept after a reboot** (the vendor's mark) — Left positioning on/off signal(M164/M165) Enable | ⛔ do not write |
| #1273 | **kept after a reboot** (the vendor's mark) — Left positioning on/off signal(M164/M165) Effective level | ⛔ do not write |
| #1274 | **kept after a reboot** (the vendor's mark) — Vacuum valve open/close signal(M1166/M167) output port number | ⛔ do not write |
| #1275 | **kept after a reboot** (the vendor's mark) — Vacuum valve open/close signal(M1166/M167) Enable | ⛔ do not write |
| #1276 | **kept after a reboot** (the vendor's mark) — Vacuum valve open/close signal(M1166/M167) Effective level | ⛔ do not write |
| #1277 | **kept after a reboot** (the vendor's mark) — Multi-process 1 open/close signal(M168/M169) output port number | ⛔ do not write |
| #1278 | **kept after a reboot** (the vendor's mark) — Multi-process 1 open/close signal(M168/M169) Enable | ⛔ do not write |
| #1279 | **kept after a reboot** (the vendor's mark) — Multi-process 1 open/close signal(M168/M169) Effective level | ⛔ do not write |
| #1280 | **kept after a reboot** (the vendor's mark) — Multi-process 2 open/close signal(M170/M171) output port number | ⛔ do not write |
| #1281 | **kept after a reboot** (the vendor's mark) — Multi-process 2 open/close signal(M170/M171) Enable | ⛔ do not write |
| #1282 | **kept after a reboot** (the vendor's mark) — Multi-process 2 open/close signal(M170/M171) Effective level | ⛔ do not write |
| #1283 | **kept after a reboot** (the vendor's mark) — Multi-process 3 open/close signal(M170/M171) output port number | ⛔ do not write |
| #1284 | **kept after a reboot** (the vendor's mark) — Multi-process 3 open/close signal(M170/M171) Enable | ⛔ do not write |
| #1285 | **kept after a reboot** (the vendor's mark) — Multi-process 3 open/close signal(M170/M171) Effective level | ⛔ do not write |
| #1286 | **kept after a reboot** (the vendor's mark) — Multi-process 4 open/close signal(M170/M171) output port number | ⛔ do not write |
| #1287 | **kept after a reboot** (the vendor's mark) — Multi-process 4 open/close signal(M170/M171) Enable | ⛔ do not write |
| #1288 | **kept after a reboot** (the vendor's mark) — Multi-process 4 open/close signal(M170/M171) Effective level | ⛔ do not write |
| #1289 | **kept after a reboot** (the vendor's mark) — Cooling 1 on/off signal(M176/M177) output port number | ⛔ do not write |
| #1290 | **kept after a reboot** (the vendor's mark) — Cooling 1 on/off signal(M176/M177) Enable | ⛔ do not write |
| #1291 | **kept after a reboot** (the vendor's mark) — Cooling 1 on/off signal(M176/M177) Effective level | ⛔ do not write |
| #1292 | **kept after a reboot** (the vendor's mark) — Cooling 2 on/off signal(M176/M177) output port number | ⛔ do not write |
| #1293 | **kept after a reboot** (the vendor's mark) — Cooling 2 on/off signal(M176/M177) Enable | ⛔ do not write |
| #1294 | **kept after a reboot** (the vendor's mark) — Cooling 2 on/off signal(M176/M177) Effective level | ⛔ do not write |
| #1295 | **kept after a reboot** (the vendor's mark) — Servo spindle enable output port — Switches analog/servo spindle (for machines with autochange) | ⛔ do not write |
| #1296 | **kept after a reboot** (the vendor's mark) — Enable servo spindle enable output port — Switches analog/servo spindle (for machines with autochange) | ⛔ do not write |
| #1297 | **kept after a reboot** (the vendor's mark) — Servo spindle enable port output level — Switches analog/servo spindle (for machines with autochange) | ⛔ do not write |
| #1298-#1299 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1300 | **kept after a reboot** (the vendor's mark) — current tool number — The current tool number. You can change. Then change the tool number on the main screen. | ⛔ do not write |
| #1301 | **kept after a reboot** (the vendor's mark) — Tool capacity | ⛔ do not write |
| #1302 | **kept after a reboot** (the vendor's mark) — Tool magazine type 0:NULL 1：Multiple 2：Follow row 3：Fixed row 4：Disk | ⛔ do not write |
| #1303 | **kept after a reboot** (the vendor's mark) — The virtual Tool function turned on | ⛔ do not write |
| #1304 | **kept after a reboot** (the vendor's mark) — Is the tool change prompt valid? — Whether to pause and prompt to change the tool when the tool change. | ⛔ do not write |
| #1305 | **kept after a reboot** (the vendor's mark) — Automatic tool probe after tool change | ⛔ do not write |
| #1306 | **kept after a reboot** (the vendor's mark) — The highest pos when chang Tool | ⛔ do not write |
| #1307 | **kept after a reboot** (the vendor's mark) — The lowest pos when chang Tool | ⛔ do not write |
| #1308 | **kept after a reboot** (the vendor's mark) — X-axis Front position for Tool change in Mach coordinate | ⛔ do not write |
| #1309 | **kept after a reboot** (the vendor's mark) — Y-axis Front position for Tool change in Mach coordinate | ⛔ do not write |
| #1310 | **kept after a reboot** (the vendor's mark) — Z-axis Front position for Tool change in Mach coordinate | ⛔ do not write |
| #1311 | **kept after a reboot** (the vendor's mark) — Spindle move speed when changing the tool | ⛔ do not write |
| #1312 | **kept after a reboot** (the vendor's mark) — Z-axis lifting speed when changing the tool | ⛔ do not write |
| #1313 | **kept after a reboot** (the vendor's mark) — The magazine horizontally moving speed | ⛔ do not write |
| #1314 | **kept after a reboot** (the vendor's mark) — Spindle lock output delay | ⛔ do not write |
| #1315 | **kept after a reboot** (the vendor's mark) — After tool change — Back to the position before the tool change | ⛔ do not write |
| #1316 | **kept after a reboot** (the vendor's mark) — X mach pos when manually changing the tool | ⛔ do not write |
| #1317 | **kept after a reboot** (the vendor's mark) — Y mach pos when manually changing the tool | ⛔ do not write |
| #1318 | **kept after a reboot** (the vendor's mark) — Z mach pos when manually changing the tool | ⛔ do not write |
| #1319 | **kept after a reboot** (the vendor's mark) — Z axis downward movement speed | ⛔ do not write |
| #1320 | **kept after a reboot** (the vendor's mark) — Pushing start X mach pos | ⛔ do not write |
| #1321 | **kept after a reboot** (the vendor's mark) — Pushing start Y mach pos | ⛔ do not write |
| #1322 | **kept after a reboot** (the vendor's mark) — push delay | ⛔ do not write |
| #1323 | **kept after a reboot** (the vendor's mark) — Pushing end X mach pos | ⛔ do not write |
| #1324 | **kept after a reboot** (the vendor's mark) — Pushing end Y mach pos | ⛔ do not write |
| #1325-#1326 | **kept after a reboot** (the vendor's mark) — Pushing completed X mach pos | ⛔ do not write |
| #1327 | **kept after a reboot** (the vendor's mark) — pushing speed | ⛔ do not write |
| #1328 | **kept after a reboot** (the vendor's mark) — 4th-Axis Mach position before tool change | ⛔ do not write |
| #1329 | **kept after a reboot** (the vendor's mark) — 5th-Axis Mach position before tool change | ⛔ do not write |
| #1330 | **kept after a reboot** (the vendor's mark) — X coordinate of No.1 Tool | ⛔ do not write |
| #1331 | **kept after a reboot** (the vendor's mark) — X coordinate of No.2 Tool | ⛔ do not write |
| #1332 | **kept after a reboot** (the vendor's mark) — X coordinate of No.3 Tool | ⛔ do not write |
| #1333 | **kept after a reboot** (the vendor's mark) — X coordinate of No.4 Tool | ⛔ do not write |
| #1334 | **kept after a reboot** (the vendor's mark) — X coordinate of No.5 Tool | ⛔ do not write |
| #1335 | **kept after a reboot** (the vendor's mark) — X coordinate of No.6 Tool | ⛔ do not write |
| #1336 | **kept after a reboot** (the vendor's mark) — X coordinate of No.7 Tool | ⛔ do not write |
| #1337 | **kept after a reboot** (the vendor's mark) — X coordinate of No.8 Tool | ⛔ do not write |
| #1338 | **kept after a reboot** (the vendor's mark) — X coordinate of No.9 Tool | ⛔ do not write |
| #1339 | **kept after a reboot** (the vendor's mark) — X coordinate of No.10 Tool | ⛔ do not write |
| #1340 | **kept after a reboot** (the vendor's mark) — X coordinate of No.11 Tool | ⛔ do not write |
| #1341 | **kept after a reboot** (the vendor's mark) — X coordinate of No.12 Tool | ⛔ do not write |
| #1342 | **kept after a reboot** (the vendor's mark) — X coordinate of No.13 Tool | ⛔ do not write |
| #1343 | **kept after a reboot** (the vendor's mark) — X coordinate of No.14 Tool | ⛔ do not write |
| #1344 | **kept after a reboot** (the vendor's mark) — X coordinate of No.15 Tool | ⛔ do not write |
| #1345 | **kept after a reboot** (the vendor's mark) — X coordinate of No.16 Tool | ⛔ do not write |
| #1346 | **kept after a reboot** (the vendor's mark) — X coordinate of No.17 Tool | ⛔ do not write |
| #1347 | **kept after a reboot** (the vendor's mark) — X coordinate of No.18 Tool | ⛔ do not write |
| #1348 | **kept after a reboot** (the vendor's mark) — X coordinate of No.19 Tool | ⛔ do not write |
| #1349 | **kept after a reboot** (the vendor's mark) — X coordinate of No.20 Tool | ⛔ do not write |
| #1350 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.1 Tool | ⛔ do not write |
| #1351 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.2 Tool | ⛔ do not write |
| #1352 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.3 Tool | ⛔ do not write |
| #1353 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.4 Tool | ⛔ do not write |
| #1354 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.5 Tool | ⛔ do not write |
| #1355 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.6 Tool | ⛔ do not write |
| #1356 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.7 Tool | ⛔ do not write |
| #1357 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.8 Tool | ⛔ do not write |
| #1358 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.9 Tool | ⛔ do not write |
| #1359 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.10 Tool | ⛔ do not write |
| #1360 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.11 Tool | ⛔ do not write |
| #1361 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.12 Tool | ⛔ do not write |
| #1362 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.13 Tool | ⛔ do not write |
| #1363 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.14 Tool | ⛔ do not write |
| #1364 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.15 Tool | ⛔ do not write |
| #1365 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.16 Tool | ⛔ do not write |
| #1366 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.17 Tool | ⛔ do not write |
| #1367 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.18 Tool | ⛔ do not write |
| #1368 | **kept after a reboot** (the vendor's mark) — coordinate of No.19 Tool | ⛔ do not write |
| #1369 | **kept after a reboot** (the vendor's mark) — Y coordinate of No.20 Tool | ⛔ do not write |
| #1370 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.1 Tool | ⛔ do not write |
| #1371 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.2 Tool | ⛔ do not write |
| #1372 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.3 Tool | ⛔ do not write |
| #1373 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.4 Tool | ⛔ do not write |
| #1374 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.5 Tool | ⛔ do not write |
| #1375 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.6 Tool | ⛔ do not write |
| #1376 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.7 Tool | ⛔ do not write |
| #1377 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.8 Tool | ⛔ do not write |
| #1378 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.9 Tool | ⛔ do not write |
| #1379 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.10 Tool | ⛔ do not write |
| #1380 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.11 Tool | ⛔ do not write |
| #1381 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.12 Tool | ⛔ do not write |
| #1382 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.13 Tool | ⛔ do not write |
| #1383 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.14 Tool | ⛔ do not write |
| #1384 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.15 Tool | ⛔ do not write |
| #1385 | **kept after a reboot** (the vendor's mark) — coordinate of No.16 Tool | ⛔ do not write |
| #1386 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.17 Tool | ⛔ do not write |
| #1387 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.18 Tool | ⛔ do not write |
| #1388 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.19 Tool | ⛔ do not write |
| #1389 | **kept after a reboot** (the vendor's mark) — Z coordinate of No.20 Tool | ⛔ do not write |
| #1390 | **kept after a reboot** (the vendor's mark) — X offset of No.1 Tool | ⛔ do not write |
| #1391 | **kept after a reboot** (the vendor's mark) — X offset of No.2 Tool | ⛔ do not write |
| #1392 | **kept after a reboot** (the vendor's mark) — X offset of No.3 Tool | ⛔ do not write |
| #1393 | **kept after a reboot** (the vendor's mark) — X offset of No.4 Tool | ⛔ do not write |
| #1394 | **kept after a reboot** (the vendor's mark) — X offset of No.5 Tool | ⛔ do not write |
| #1395 | **kept after a reboot** (the vendor's mark) — X offset of No.6 Tool | ⛔ do not write |
| #1396 | **kept after a reboot** (the vendor's mark) — X offset of No.7 Tool | ⛔ do not write |
| #1397 | **kept after a reboot** (the vendor's mark) — X offset of No.8 Tool | ⛔ do not write |
| #1398 | **kept after a reboot** (the vendor's mark) — X offset of No.9 Tool | ⛔ do not write |
| #1399 | **kept after a reboot** (the vendor's mark) — X offset of No.10 Tool | ⛔ do not write |
| #1400 | **kept after a reboot** (the vendor's mark) — X offset of No.11 Tool | ⛔ do not write |
| #1401 | **kept after a reboot** (the vendor's mark) — X offset of No.12 Tool | ⛔ do not write |
| #1402 | **kept after a reboot** (the vendor's mark) — X offset of No.13 Tool | ⛔ do not write |
| #1403 | **kept after a reboot** (the vendor's mark) — X offset of No.14 Tool | ⛔ do not write |
| #1404 | **kept after a reboot** (the vendor's mark) — X offset of No.15 Tool | ⛔ do not write |
| #1405 | **kept after a reboot** (the vendor's mark) — X offset of No.16 Tool | ⛔ do not write |
| #1406 | **kept after a reboot** (the vendor's mark) — X offset of No.17 Tool | ⛔ do not write |
| #1407 | **kept after a reboot** (the vendor's mark) — X offset of No.18 Tool | ⛔ do not write |
| #1408 | **kept after a reboot** (the vendor's mark) — X offset of No.19 Tool | ⛔ do not write |
| #1409 | **kept after a reboot** (the vendor's mark) — X offset of No.20 Tool | ⛔ do not write |
| #1410 | **kept after a reboot** (the vendor's mark) — Y offset of No.1 Tool | ⛔ do not write |
| #1411 | **kept after a reboot** (the vendor's mark) — Y offset of No.2 Tool | ⛔ do not write |
| #1412 | **kept after a reboot** (the vendor's mark) — Y offset of No.3 Tool | ⛔ do not write |
| #1413 | **kept after a reboot** (the vendor's mark) — Y offset of No.4 Tool | ⛔ do not write |
| #1414 | **kept after a reboot** (the vendor's mark) — Y offset of No.5 Tool | ⛔ do not write |
| #1415 | **kept after a reboot** (the vendor's mark) — Y offset of No.6 Tool | ⛔ do not write |
| #1416 | **kept after a reboot** (the vendor's mark) — Y offset of No.7 Tool | ⛔ do not write |
| #1417 | **kept after a reboot** (the vendor's mark) — Y offset of No.8 Tool | ⛔ do not write |
| #1418 | **kept after a reboot** (the vendor's mark) — Y offset of No.9 Tool | ⛔ do not write |
| #1419 | **kept after a reboot** (the vendor's mark) — Y offset of No.10 Tool | ⛔ do not write |
| #1420 | **kept after a reboot** (the vendor's mark) — Y offset of No.11 Tool | ⛔ do not write |
| #1421 | **kept after a reboot** (the vendor's mark) — Yoffset of No.12 Tool | ⛔ do not write |
| #1422 | **kept after a reboot** (the vendor's mark) — Y offset of No.13 Tool | ⛔ do not write |
| #1423 | **kept after a reboot** (the vendor's mark) — Y offset of No.14 Tool | ⛔ do not write |
| #1424 | **kept after a reboot** (the vendor's mark) — Y offset of No.15 Tool | ⛔ do not write |
| #1425 | **kept after a reboot** (the vendor's mark) — offset of No.16 Tool | ⛔ do not write |
| #1426 | **kept after a reboot** (the vendor's mark) — Y offset of No.17 Tool | ⛔ do not write |
| #1427 | **kept after a reboot** (the vendor's mark) — Y offset of No.18 Tool | ⛔ do not write |
| #1428 | **kept after a reboot** (the vendor's mark) — Y offset of No.19 Tool | ⛔ do not write |
| #1429 | **kept after a reboot** (the vendor's mark) — Y offset of No.20 Tool | ⛔ do not write |
| #1430 | **kept after a reboot** (the vendor's mark) — offset of No.1 Tool | ⛔ do not write |
| #1431 | **kept after a reboot** (the vendor's mark) — Z offset of No.2 Tool | ⛔ do not write |
| #1432 | **kept after a reboot** (the vendor's mark) — Z offset of No.3 Tool | ⛔ do not write |
| #1433 | **kept after a reboot** (the vendor's mark) — Z offset of No.4 Tool | ⛔ do not write |
| #1434 | **kept after a reboot** (the vendor's mark) — Z offset of No.5 Tool | ⛔ do not write |
| #1435 | **kept after a reboot** (the vendor's mark) — Z offset of No.6 Tool | ⛔ do not write |
| #1436 | **kept after a reboot** (the vendor's mark) — Z offset of No.7 Tool | ⛔ do not write |
| #1437 | **kept after a reboot** (the vendor's mark) — Z offset of No.8 Tool | ⛔ do not write |
| #1438 | **kept after a reboot** (the vendor's mark) — Z offset of No.9 Tool | ⛔ do not write |
| #1439 | **kept after a reboot** (the vendor's mark) — Z offset of No.10 Tool | ⛔ do not write |
| #1440 | **kept after a reboot** (the vendor's mark) — Z offset of No.11 Tool | ⛔ do not write |
| #1441 | **kept after a reboot** (the vendor's mark) — Z offset of No.12 Tool | ⛔ do not write |
| #1442 | **kept after a reboot** (the vendor's mark) — Z offset of No.13 Tool | ⛔ do not write |
| #1443 | **kept after a reboot** (the vendor's mark) — Z offset of No.14 Tool | ⛔ do not write |
| #1444 | **kept after a reboot** (the vendor's mark) — Z offset of No.15 Tool | ⛔ do not write |
| #1445 | **kept after a reboot** (the vendor's mark) — Z offset of No.16 Tool | ⛔ do not write |
| #1446 | **kept after a reboot** (the vendor's mark) — Z offset of No.17 Tool | ⛔ do not write |
| #1447 | **kept after a reboot** (the vendor's mark) — Z offset of No.18 Tool | ⛔ do not write |
| #1448 | **kept after a reboot** (the vendor's mark) — Z offset of No.19 Tool | ⛔ do not write |
| #1449 | **kept after a reboot** (the vendor's mark) — Z offset of No.20 Tool | ⛔ do not write |
| #1450 | **kept after a reboot** (the vendor's mark) — T1 file no. | ⛔ do not write |
| #1451 | **kept after a reboot** (the vendor's mark) — T2 file no. | ⛔ do not write |
| #1452 | **kept after a reboot** (the vendor's mark) — Safety height Z | ⛔ do not write |
| #1453 | **kept after a reboot** (the vendor's mark) — Starting point Y | ⛔ do not write |
| #1454 | **kept after a reboot** (the vendor's mark) — Starting point Z | ⛔ do not write |
| #1455 | **kept after a reboot** (the vendor's mark) — Xpos1 — work coordinate value. | ⛔ do not write |
| #1456 | **kept after a reboot** (the vendor's mark) — Xpos2 — work coordinate value. | ⛔ do not write |
| #1457 | **kept after a reboot** (the vendor's mark) — Xpos3 — work coordinate value. | ⛔ do not write |
| #1458 | **kept after a reboot** (the vendor's mark) — Xpos4 — work coordinate value. | ⛔ do not write |
| #1459 | **kept after a reboot** (the vendor's mark) — Xpos5 — work coordinate value. | ⛔ do not write |
| #1460 | **kept after a reboot** (the vendor's mark) — Xpos6 — work coordinate value. | ⛔ do not write |
| #1461-#1464 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1465 | **kept after a reboot** (the vendor's mark) — pause between T1 and T2 | ⛔ do not write |
| #1466 | **kept after a reboot** (the vendor's mark) — T1 spindle speed | ⛔ do not write |
| #1467 | **kept after a reboot** (the vendor's mark) — T2 spindle speed | ⛔ do not write |
| #1468 | **kept after a reboot** (the vendor's mark) — NC file index no. | ⛔ do not write |
| #1469 | **kept after a reboot** (the vendor's mark) — Custom G-code number — 自定义G代码号 | ⛔ do not write |
| #1470-#1471 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1472 | **kept after a reboot** (the vendor's mark) — This feature was developed at the request of one of the clients. What it does is not clear. — May be -57.544. That is, it seems to work. | ⛔ do not write |
| #1473 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 1 | ⛔ do not write |
| #1474 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 2 | ⛔ do not write |
| #1475 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 3 | ⛔ do not write |
| #1476 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 4 | ⛔ do not write |
| #1477 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 5 | ⛔ do not write |
| #1478 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 6 | ⛔ do not write |
| #1479 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 7 | ⛔ do not write |
| #1480 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 8 | ⛔ do not write |
| #1481 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 9 | ⛔ do not write |
| #1482 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 10 | ⛔ do not write |
| #1483 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 11 | ⛔ do not write |
| #1484 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 12 | ⛔ do not write |
| #1485 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 13 | ⛔ do not write |
| #1486 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 14 | ⛔ do not write |
| #1487 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 15 | ⛔ do not write |
| #1488 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 16 | ⛔ do not write |
| #1489 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 17 | ⛔ do not write |
| #1490 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 18 | ⛔ do not write |
| #1491 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 19 | ⛔ do not write |
| #1492 | **kept after a reboot** (the vendor's mark) — Virtual tool Z offset 20 | ⛔ do not write |
| #1493-#1498 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* | ⛔ do not write |
| #1499 | **kept after a reboot** (the vendor's mark) — coordinate offset method | ⛔ do not write |
| #2500-#2599 | **kept after a reboot** (the vendor's mark) — *(no description in the table)* |  |
| #2600-#2999 | **kept after a reboot** (the vendor's mark) — These variables are passed the values of the parameters of the CAM function (1100-1499) — Read more about this in the instructions for the CAM function. Also, the values of these variables cannot be changed "manually". Only through the parameters of the CAM function. |  |

## Other [N, no mark]

| variables | description | note |
|---|---|---|
| #1991 | system mode |  |
| #2039-#2041 | *(no description in the table)* | ❔ not measured |
| #2043-#2071 | *(no description in the table)* | ❔ not measured, measured |
