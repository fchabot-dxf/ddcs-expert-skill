# DDCS Expert M350 — variables

> Every measured statement names the bench test that measured it (see evidence.md).

Source: **Variables-ENG 01-04-2025.csv**, a variable table for the DDCS Expert M350. The descriptions are the table's own words.

⛔ Never write a variable you have not looked up here. A value of 0 does not mean unused.

## Beyond the table

What is declared or measured about these variables beyond the table, each with its evidence.

| variables | | what | evidence |
|---|---|---|---|
| #100-#499 | measured | The work share's uservar file holds them (slot = #var - 100; its slots run on to #549, but #500 up may be the parameters: not settled, never write them). A PC reads it; a PC write made during a run never reached the program, and one between runs is not measured: never write it. | operator test writes decoded over SMB |
| #500-#1499 | ⛔ do not write | The Param page, Pr0-Pr999: macro #N is Pr(N - 500). A write changes a machine setting. Never as scratch. | Modbus reads of the whole parameter table against the setting file (FINDINGS section 28) |
| #578 | measured | The active work system number; it does NOT follow a G54..G59 in the program: emit G54..G59 to select. | V4_active_wcs.nc, DIAG_g53setup.nc |
| #622-#626 | ⛔ do not write | The machine position after each axis goes home: a write moves the machine zero. Never as scratch. | the variable table names it |
| #655-#670 | ⛔ do not write | The software limits (enable, stop mode, the negative and positive limit of each axis): a write moves or disables a limit. Never as scratch. | the variable table names it |
| #735-#739 | ⛔ do not write | The home offset of each axis: a write moves the machine zero. Never as scratch. | the variable table names it |
| #766-#767 | ⛔ do not write | The serial baud rates: breaking them loses the Modbus link. Never as scratch. | the variable table names it |
| #779 | ⛔ do not write | The Modbus RTU setting: breaking it loses the Modbus link. Never as scratch. | the variable table names it |
| #796-#797 | ⛔ do not write | The serial stop bits / Modbus mode (the vendor table leaves them unnamed): breaking them loses the Modbus link. Never as scratch. | panel photo + live adapter test |
| #800-#804 | ⛔ do not write | The G92 offset of each axis: a write moves the G92 frame. Never as scratch. | the variable table names it |
| #805-#834 | ⛔ do not write | G54..G59 offsets, 5 per system: a write moves a work zero. Never as scratch. | panel read of all six systems |
| #900-#915 | ⛔ do not write | H01..H16 tool-length offsets: a stray value is a tool length. Never as scratch. | V6_set.nc, V6_restore.nc |
| #915 | measured | The Modbus variable reader borrows it (#915 = #<target>, read register 7330) and must restore 0: it is H16. | six-variable test |
| #916-#919 | ⛔ do not write | The vendor table names these "Possibly H17..H20 offset" (only H01-H16 are measured). Treat as tool lengths: never as scratch. | the variable table names it |
| #920-#939 | ⛔ do not write | D01..D20 cutter-compensation offsets: a stray value is a cutter radius. Never as scratch. | the variable table names it |
| #1505 | measured | #1505=1(message) is a blocking OK/Cancel dialog; #1505=-5000(message) a display-only message. | operator macros (READ_VAR.nc, COPY_WCS.nc) |
| #1630 | ⛔ do not write | Never READ it inside a running program either: it wedges the analyzer until a reboot. | PUSH_RUNSTATE.nc |
| #2037 | measured | #2037 = 65536 + [KeyValue - 1000] presses a panel key; it must run as code. | A7b_BUTTON_ONEWAY.nc, photographed pendant |
| #2070 | measured | #2070 = <var>(prompt) pauses for the operator to type a number into that variable. | operator macros |

## System [S, N]

| variables | description | note |
|---|---|---|
| #0-#9 | User variables independent for slib and user UE — This range is not suitable for storing axis coordinates, from variables #790-#844. If you store axis coordinates in this range of variable numbers, the values will be lost. Only this range is suitable for the seamless movement of the axes, with G0, G1, G2, G3 commands. |  |
| #10-#21 | User variables independent for slib and user UE |  |
| #22-#49 | User variables independent for slib and user UE — This range is not suitable for storing axis coordinates, from variables #790-#844. If you store axis coordinates in this range of variable numbers, the values will be lost. Only this range is suitable for the seamless movement of the axes, with G0, G1, G2, G3 commands. |  |
| #50-#55 | Used in G28 and G29 commands |  |
| #56-#100 | *(no description in the table)* | measured |
| #101 | Used in subroutine O503 (dual y-axis) from slib-g as port number — Subroutine O503 is obsolete | measured |
| #102-#499 | *(no description in the table)* | measured |
| #504-#505 | ??pulses per unit | ⛔ do not write |
| #530 | machine type | ⛔ do not write |
| #531 | RTCP Resolution | ⛔ do not write |
| #571 | Enabling motion scheduling of the G0 segment — G0段运动规划使能 | ⛔ do not write |
| #572 | G0 command motion characteristics — 0: Interpolation; 1:Independent, Interpolation:synergistic movement of each axis./Independent:each axis independently moves at G0 speed. | ⛔ do not write |
| #586 | Spindle PWM electric level | ⛔ do not write |
| #587 | PWM rising rate from 0V to 10V | ⛔ do not write |
| #702 | The number of processed pieces (Current number of processed pieces) — Number of completed programs in the current session. Upgrades with the completion of M30. Cleared on reboot. | ⛔ do not write |
| #725 | Feed rate change range — Magnification increment when handwheel exit and manual override adjustment. | ⛔ do not write |
| #726 | Magnification decrease in pause — Suspension rate change when decelerating to stop. | ⛔ do not write |
| #727 | Processing magnification change interval — Cooperate with parameter 225. | ⛔ do not write |
| #728 | Magnification change when resetting alarm | ⛔ do not write |
| #729 | Reset alarm rate change interval | ⛔ do not write |
| #742 | Main interface display scheme — 主界面显示方案 | ⛔ do not write |
| #746 | Max. polyline segments | ⛔ do not write |
| #749 | parameter gradient — J参数梯度 | ⛔ do not write |
| #750 | Maximum machining contour error | ⛔ do not write |
| #754 | Toolpath width (Area width (X)) — Set individually, for each program file name (NC) | ⛔ do not write |
| #755 | Toolpath Length (Height of area (Y)) — Set individually, for each program file name (NC) | ⛔ do not write |
| #756 | Toolpath thickness (Region depth (Z)) — Set individually, for each program file name (NC) | ⛔ do not write |
| #757 | The toolpath plane thickness in Statue mode — Set individually, for each program file name (NC) | ⛔ do not write |
| #758 | The X Origin point on plane — Set individually, for each program file name (NC) | ⛔ do not write |
| #759 | The Y Origin point on plane — Set individually, for each program file name (NC) | ⛔ do not write |
| #760 | The Z Origin point on plane — Set individually, for each program file name (NC) | ⛔ do not write |
| #761 | X-axis rotation angle in 3D toolpath mode | ⛔ do not write |
| #762 | Y-axis rotation angle in 3D toolpath mode | ⛔ do not write |
| #763 | Z-axis rotation angle in 3D toolpath mode | ⛔ do not write |
| #770 | user permission key — This number is just a reminder of the superuser password. Changing it does not change the password. | ⛔ do not write |
| #835 | G52 Temporary Coordinates offset X — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #836 | G52 Temporary Coordinates offset Y — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #837 | G52 Temporary coordinates offset Z — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #838 | G52 Temporary Coordinates offset A — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #839 | G52 Temporary Coordinates offset B — Sets the temporary displacement of the axes in all working CS. Added to the main offset of the SK. NOT displayed in the "Coord set" menu. NOT saved after reboot. It can only be changed if after #835=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #940 | 1th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #941 | 2th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #942 | 3th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #946-#948 | *(no description in the table)* | ⛔ do not write |
| #1500 | M1 Unconditional stop Status — Enable/disable for M1 |  |
| #1501 | Tapping mode: 0 Left-hand tapping thread(Counter-thread) — 1Right-hand tapping thread(Normal thread), Thread cutting mode. Right or left. |  |
| #1502 | Probe Mode — Sets the probing mode. 0-?; 1-movable sensor; 2-fixed sensor |  |
| #1503 | prompt information — Prints text to the status bar, only works if "#269 Debug printing enable" = 1 |  |
| #1504 | Target Tool number Temporary variables — Stores the tool number from the M6Txx command |  |
| #1505 | Dialog prompt message — Displays a window with a message number. | measured |
| #1506 | single piece processing finished mark — Saves how the last UE ended. M30 or M47 |  |
| #1507 | Processing time record switch — Stops and starts the NC execution timer (Work Time) |  |
| #1508 | 2nd probe operation switch |  |
| #1510 | Print Value 1 — Works in conjunction with #1503 |  |
| #1511 | Print Value 2 — Works in conjunction with #1503 |  |
| #1512 | Print Value 3 — Works in conjunction with #1503 |  |
| #1513 | Print Value 4 — Works in conjunction with #1503 |  |
| #1514 | Lift up enable when pause |  |
| #1515 | The Mark when X back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1516 | The Mark when Y back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1517 | The Mark when Z back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1518 | The Mark when 4th back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1519 | The Mark when 5th back to Home — Displays the status and toggles the "homeless" icon of the axis (when an axis is homemade, a dot appears next to its letter on the main screen) |  |
| #1544 | *(no description in the table)* |  |
| #1545 | Delay time (milliseconds) |  |
| #1546-#1551 | *(no description in the table)* |  |
| #1552 | Common Output port 01 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1553 | Common Output port 02 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1554 | Common Output port 03 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1555 | Common Output port 04 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1556 | Common Output port 05 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1557 | Common Output port 06 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1558 | Common Output port 07 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1559 | Common Output port 08 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1560 | Common Output port 09 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1561 | Common Output Port 10 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1562 | Common Output port 11 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1563 | Common Output port 12 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1564 | Common Output port 13 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1565 | Common Output port 14 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1566 | Common Output Port 15 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1567 | Common Output port 16 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1568 | Common Output port 17 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1569 | Common Output port 18 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1570 | Common Output port 19 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1571 | Common Output Port 20 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1572 | Common Output port 21 Status — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1573 | Common Output port 22 Status (unused) — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1574 | Common Output port 23 Status (unused) — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1575 | Common Output port 24 Status (unused) — Toggles the state of the output port. 1=closed; 0=open. Can only be 1 or 0. If any number > 1 is written, the value is converted to 1. |  |
| #1577-#1615 | *(no description in the table)* |  |
| #1616 | Current spindle speed — Displays how fast the spindle is currently turning. If the spindle is stopped (M5) then 0. |  |
| #1617 | *(no description in the table)* |  |
| #1618 | Home Start Sign 0:Start 1:End — When any axis is homed = 1. Otherwise 0. Homing progress indicator at start. |  |
| #1619 | label prompt information — Outputs text to a label printer (physical device) |  |
| #1627-#1629 | *(no description in the table)* |  |
| #1637-#1699 | *(no description in the table)* |  |
| #1719-#1749 | *(no description in the table)* |  |
| #1750-#1759 | modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1760 | modbus function macro address — accept data buffer address (e.g. 1790) |  |
| #1761 | modbus function macro address — Serial port number for receiving data (there are 0 and 1) |  |
| #1762 | modbus function macro address — ID number of the slave device |  |
| #1763 | modbus function macro address — encoder circle address |  |
| #1764 | modbus function macro address — Number of bytes |  |
| #1765 | modbus function macro address — function code 03 |  |
| #1766 | modbus function macro address — Receives some data |  |
| #1767 | modbus function macro address — laps return code (the value of this variable is displayed in a message in the status bar) |  |
| #1768 | modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1769 | modbus function macro address — Trigger for the end of data reception. It is set to 1, then WHILE [#1769 NE 0] DO1, a pause G04P10 is written in the loop body. When #1769=0, the data is accepted. |  |
| #1770 | modbus function macro address — This variable stores the result of data transformation |  |
| #1771 | modbus function macro address — row index (usually set to 1790 - data buffer address) |  |
| #1772 | modbus function macro address — Number of bytes |  |
| #1773 | modbus function macro address — Number type - 1 - 16-bit signed integer; 4 is a 32-bit unsigned integer. Accepts only 1, 2, 3, 4 |  |
| #1774 | modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1775 | modbus function macro address — Data conversion (1 - convert data) |  |
| #1776-#1789 | modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1790 | modbus function macro address — Used as a buffer for receiving data. Any free modbus variable can serve as a data buffer. |  |
| #1791-#1799 | modbus function macro address — Variable for COM port (modbus function). This RS232 control method is obsolete. Another one has now been implemented. |  |
| #1800 | Analyze channel X axis control mark 0:X axis is in analyzing 1:X axis is in manual mode — Utility function, manufacturer only. |  |
| #1801 | Analyze channel Y axis control mark 0:Y axis is in analyzing 1:Y axis is in manual mode — Utility function, manufacturer only. |  |
| #1802 | Analyze channel Z axis control mark 0:Z axis is in analyzing 1:Z axis is in manual mode — Utility function, manufacturer only. |  |
| #1803 | Analyze channel 4th axis control mark 0:4th axis is in analyzing 1:4th axis is in manual mode — Utility function, manufacturer only. |  |
| #1804 | Analyze channel 5th axis control mark 0:5th axis is in analyzing 1:5th axis is in manual mode — Utility function, manufacturer only. |  |
| #1805-#1894 | *(no description in the table)* |  |
| #1895 | G31 X axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1896 | G31 Y axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1897 | G31 Z axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1898 | G31 4th axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1899 | G31 5th axis detecting Speed — Remembers the speed of the axis (F), the last time G31 was executed |  |
| #1900 | G31 X axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1901 | G31 Y axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1902 | G31 Z axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1903 | G31 4th axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1904 | G31 5th axis Detecting signal number — Remembers the port number(P) when G31 was last executed |  |
| #1905 | G31 X detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1906 | G31 Y detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1907 | G31 Z detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1908 | G31 4th detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1909 | G31 5th detection signal stop mode 0: Decelerate to stop; 1: Emergency — Remembers the type of stop(Q) when last executed in G31 |  |
| #1910 | G31 X detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1911 | G31 Y detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1912 | G31 Z detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1913 | G31 4th detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1914 | G31 5th detection signal Effective level 0/1 — Remembers the trigger level (L) the last time a G31 was executed |  |
| #1915 | G31 X axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1916 | G31 Y axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1917 | G31 Z axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1918 | G31 4th axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1919 | G31 5th axis limit mode when probe 0:Ignore limit 1:Enable limit to Negative (-) direction 2：Enable limit to Positive direction (+) — Remembers the response to Hard Limit(K) the last time G31 was executed |  |
| #1920 | G31 X axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1921 | G31 Y axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1922 | G31 Z axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1923 | G31 4th axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1924 | G31 5th axis Probe result 0:Did not Probe 1:Initialize 2:Detected the signal 3:Triggered Negative limit signal 4:Triggered Positive limit signal — The result of the last execution of the G31 command. (how did it end) |  |
| #1925 | G31 The X mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1926 | G31 The Y mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1927 | G31 The Z mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1928 | G31 The 4th mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1929 | G31 The 5th mach coordinates after probe signal triggered — Stores the machine coordinates of the axis, the last time the encoder was triggered, in G31. Used to determine the error value of the sensor operation. |  |
| #1930 | Key Indicator 1 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1931 | Key Indicator 2 Output Control (Error) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1932 | Key Indicator 3 Output Control (Run) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1933 | Key Indicator 4 Output Control (USB) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1934 | Key Indicator 5 Output Control (ESC) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1935 | Key Indicator 6 Output Control (UP) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1936 | Key Indicator 7 Output Control (Enter) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1937 | Key Indicator 8 Output Control (LEFT) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1938 | Key Indicator 9 Output Control (DOWN) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1939 | Key Indicator 10 Output Control (RIGHT) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1940 | Key Indicator 11 Output Control (Hight/Low Speed) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1941 | Key Indicator 12 Output Control (Axis 5 -) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1942 | Key Indicator 13 Output Control (Brake point Resume) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1943 | Key Indicator 14 Output Control (Axis 5+) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1944 | Key Indicator 15 Output Control (K1) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1945 | Key Indicator 16 Output Control (K2) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1946 | Key Indicator 17 Output Control (K3) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1947 | Key Indicator 18 Output Control (K4) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1948 | Key Indicator 19 Output Control (K5) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1949 | Key Indicator 20 Output Control (K6) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1950 | Key Indicator 21 Output Control (K7) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1951 | Key Indicator 22 Output Control (SPINDLE) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1952 | Key Indicator 23 Output Control (TruCut) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1953 | Key Indicator 24 Output Control (CONT STEP MPG) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1954 | Key Indicator 25 Output Control (START) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1955 | Key Indicator 26 Output Control (PAUSE) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1956 | Key Indicator 27 Output Control (RESET) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1957 | Key Indicator 28 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1958 | Key Indicator 29 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1959 | Key Indicator 30 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1960 | Key Indicator 31 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1961 | Key Indicator 32 Output Control (not working) — Controls the button's LED. 0-disabled. Any number > 0 - enabled. |  |
| #1962-#1969 | *(no description in the table)* |  |
| #1970 | One Key Probe Tool No. 1 — Used in mulprobe.nc file |  |
| #1971 | One Key Probe Tool No. 2 — Used in mulprobe.nc file |  |
| #1972 | One Key Probe Tool No. 3 — Used in mulprobe.nc file |  |
| #1973 | One Key Probe Tool No. 4 — Used in mulprobe.nc file |  |
| #1974 | One Key Probe Tool No. 5 — Used in mulprobe.nc file |  |
| #1975 | One Key Probe Tool No. 6 — Used in mulprobe.nc file |  |
| #1976 | One Key Probe Tool No. 7 — Used in mulprobe.nc file |  |
| #1977 | One Key Probe Tool No. 8 — Used in mulprobe.nc file |  |
| #1978 | One Key Probe Tool No. 9 — Used in mulprobe.nc file |  |
| #1979 | One Key Probe Tool No. 10 — Used in mulprobe.nc file |  |
| #1980 | One Key Probe Tool No. eleven — Used in mulprobe.nc file |  |
| #1981 | One Key Probe Tool No. 12 — Used in mulprobe.nc file |  |
| #1982 | One Key Probe Tool No. 13 — Used in mulprobe.nc file |  |
| #1983 | One Key Probe Tool No. 14 — Used in mulprobe.nc file |  |
| #1984 | One Key Probe Tool No. 15 — Used in mulprobe.nc file |  |
| #1985 | One Key Probe Tool No. 16 — Used in mulprobe.nc file |  |
| #1986 | One Key Probe Tool No. 17 — Used in mulprobe.nc file |  |
| #1987 | One Key Probe Tool No. 18 — Used in mulprobe.nc file |  |
| #1988 | One Key Probe Tool No. 19 — Used in mulprobe.nc file |  |
| #1989 | One Key Probe Tool No. 20 — Used in mulprobe.nc file |  |
| #1990 | *(no description in the table)* |  |
| #1991 | system mode |  |
| #1992 | Home function enabled (does not work anymore) — Used to enable homing after downloading. Now doesn't work. |  |
| #1993 | Probe enabled after tool change |  |
| #1994 | RTCP Enable/Disable — Turning this setting on doesn't make sense. Because RTCP doesn't work. If enabled, the controller will fail. |  |
| #1995 | Current Max.AxisNumber — Number of controller axes |  |
| #1996 | Self-define M code Temporary variables (Custom M code number temporary variable) — used in ext_button.nc (substituted as command number M - M#1996). May be equal to 250-263. Enables the EXT KEY FUNC K01-K14 commands of the Monitor - Manual page |  |
| #1997 | Manually operation self-define Key 1 status — Transfers the state of the virtual button FUNC K01, (pressed or released), to the M250 command |  |
| #1998 | Manually operation self-define Key 2 status — Transfers the state of the virtual button FUNC K02, (pressed or released), to the command M251 |  |
| #1999 | Manually operation self-define Key 3 status — Transfers the state of the virtual button FUNC K03, (pressed or released), to the command M252 |  |
| #2000 | Manually operation self-define Key 4 status — Transfers the state of the virtual button FUNC K04, (pressed or released), to the command M253 |  |
| #2001 | Manually operation self-define Key 5 status — Transfers the state of the virtual button FUNC K05, (pressed or released), to the command M254 |  |
| #2002 | Manually operation self-define Key 6 status — Transfers the state of the virtual button FUNC K06, (pressed or released), to the command M255 |  |
| #2003 | Manually operation self-define Key 7 status — Transfers the state of the virtual button FUNC K07, (pressed or released), to the command M256 |  |
| #2004 | Manually operation self-define Key 8 status — Transfers the state of the virtual button FUNC K08, (pressed or released), to the command M257 |  |
| #2005 | Manually operation self-define Key 9 status — Transfers the state of the virtual button FUNC K09, (pressed or released), to the command M258 |  |
| #2006 | Manually operation self-define Key 10 status — Transfers the state of the FUNC K10 virtual button, (pressed or released), to the M259 command |  |
| #2007 | Manually operation self-define Key 11 status — Transfers the state of the FUNC K11 virtual button, (pressed or released), to the M260 command |  |
| #2008 | Manually operation self-define Key 12 status — Transfers the status of the FUNC K12 virtual button, (pressed or released), to the M261 command |  |
| #2009 | Manually operation self-define Key 13 status — Sends the state of the FUNC K13 virtual button, (pressed or released), to the M262 command |  |
| #2010 | Manually operation self-define Key 14 status — Transfers the status of the FUNC K14 virtual button, (pressed or released), to the M263 command |  |
| #2011 | Manually operation self-define Key 15 status — Missing code in slib-m. The FUNC K15 button is missing from the Manual page. |  |
| #2012 | Manually operation self-define Key 16 status — Missing code in slib-m. The FUNC K16 button is missing from the Manual page. |  |
| #2013 | Manually operation self-define Key 17 status — Missing code in slib-m. The FUNC K17 button is missing from the Manual page. |  |
| #2014 | Manually operation self-define Key 18 status — Missing code in slib-m. The FUNC K18 button is missing from the Manual page. |  |
| #2015 | Manually operation self-define Key 19 status — Missing code in slib-m. The FUNC K19 button is missing from the Manual page. |  |
| #2016 | Manually operation self-define Key 20 status — Missing code in slib-m. The FUNC K20 button is missing from the Manual page. |  |
| #2017 | Automatically operation self-define Key 1 status — Status of virtual button 1, (pressed or released), from command M270 |  |
| #2018 | Automatically operation self-define Key 2 status — Status of virtual button 2, (pressed or released), from command M271 |  |
| #2019 | Automatically operation self-define Key 3 status — Status of virtual button 3, (pressed or released), from command M272 |  |
| #2020 | Automatically operation self-define Key 4 status — Status of virtual button 4, (pressed or released), from command M273 |  |
| #2021 | Automatically operation self-define Key 5 status — Status of virtual button 5, (pressed or released), from command M274 |  |
| #2022 | Automatically operation self-define Key 6 status — Status of virtual button 6, (pressed or released), from command M275 |  |
| #2023 | Automatically operation self-define Key 7 status — Status of virtual button 7, (pressed or released), from command M276 |  |
| #2024 | Automatically operation self-define Key 8 status — Status of virtual button 8, (pressed or released), from command M277 |  |
| #2025 | Automatically operation self-define Key 9 status — Status of virtual button 9, (pressed or released), from command M278 |  |
| #2026 | Automatically operation self-define Key 10 status — Status of virtual button 10, (pressed or released), from command M279 |  |
| #2027 | Automatically operation self-define Key 11 status — Status of virtual button 11, (pressed or released), from command M280 |  |
| #2028 | Automatically operation self-define Key 12 status — Status of virtual button 12, (pressed or released), from command M280 |  |
| #2029-#2036 | *(no description in the table)* |  |
| #2037 | Virtual key trigger register 0x1017f 017f is the key number — 1 is the status when pressed down, Presses a virtual button, saves the entered value until reboot #2037=64536+function code | measured |
| #2038 | Possibly Self-define G code Temporary variables — Used in CAM.nc. Substituted as the G number of the team - G#2038 |  |
| #2042 | Beep sound for macro program — in milliseconds |  |
| #2072 | K1 function Key indicator Address — Control variable number, button LED K1 (#[1930+#2072]) |  |
| #2073 | K2 function Key indicator Address — Control variable number, button LED K2 (#[1930+#2073]) |  |
| #2074 | K3 function Key indicator Address — Control variable number, button LED K3 (#[1930+#2074]) |  |
| #2075 | K4 function Key indicator Address — Control variable number, button LED K4 (#[1930+#2075]) |  |
| #2076 | K5 function Key indicator Address — Control variable number, button LED K5 (#[1930+#2076]) |  |
| #2077 | K6 function Key indicator Address — Control variable number, button LED K6 (#[1930+#2077]) |  |
| #2078 | K7 function Key indicator Address — Control variable number, button LED K7 (#[1930+#2078]) |  |
| #2079 | K8 function Key indicator Address (not working) — does not work |  |
| #2080-#2091 | *(no description in the table)* |  |
| #2092 | Teach M118 input detection input port number — Used in M118 command. Subroutine O10118 in slib-m |  |
| #2093 | Teach M118 input port effective level — Used in M118 command. Subroutine O10118 in slib-m |  |
| #2094-#2095 | Used in the M129 command. |  |
| #2097 | Stores the result of the M129 command — values 1 or 0 |  |
| #2098-#2099 | *(no description in the table)* |  |
| #2100-#2104 | Used in slib-g — in O501 (homing) and O503, as an intermediate variable, With her participation, the function of calculating the accuracy of the operation of the homing sensor works |  |
| #2105 | Used in slib-g — in O501 (homing) and O503, as an intermediate variable, With her participation, the function of calculating the number of errors when entering the homing sensor works |  |
| #2106-#2499 | *(no description in the table)* |  |

## Read-only

| variables | description | note |
|---|---|---|
| #633 | Probe Tool block thickness — Useful floating tool | ⛔ do not write |
| #790 | axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #791 | Y axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #792 | Z axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #793 | 4th axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #794 | 5th axis realtime workpiece coordinate position — The current position of the axis, in the current working coordinate system | ⛔ do not write |
| #850 | G54 Coordinates X Absolute position — Current axis position in G54 | ⛔ do not write |
| #851 | G54 Coordinates Y Absolute position — Current axis position in G54 | ⛔ do not write |
| #852 | G54 Coordinates Z Absolute position — Current axis position in G54 | ⛔ do not write |
| #853 | G54 Coordinates A Absolute position — Current axis position in G54 | ⛔ do not write |
| #854 | G54 Coordinates B Absolute position — Current axis position in G54 | ⛔ do not write |
| #855 | G55 Coordinates X Absolute position — Current axis position in G55 | ⛔ do not write |
| #856 | G55 Coordinates Y Absolute position — Current axis position in G55 | ⛔ do not write |
| #857 | G55 Coordinates Z Absolute position — Current axis position in G55 | ⛔ do not write |
| #858 | G55 Coordinates A Absolute position — Current axis position in G55 | ⛔ do not write |
| #859 | G55 Coordinates B Absolute position — Current axis position in G55 | ⛔ do not write |
| #860 | G56 Coordinates X Absolute position — Current axis position in G56 | ⛔ do not write |
| #861 | G56 Coordinates Y Absolute position — Current axis position in G56 | ⛔ do not write |
| #862 | G56 Coordinates Z Absolute position — Current axis position in G56 | ⛔ do not write |
| #863 | G56 Coordinates A Absolute position — Current axis position in G56 | ⛔ do not write |
| #864 | G56 Coordinates B Absolute position — Current axis position in G56 | ⛔ do not write |
| #865 | G57 Coordinates X Absolute position — Current axis position in G57 | ⛔ do not write |
| #866 | G57 Coordinates Y Absolute position — Current axis position in G57 | ⛔ do not write |
| #867 | G57 Coordinates Z Absolute position — Current axis position in G57 | ⛔ do not write |
| #868 | G57 Coordinates A Absolute position — Current axis position in G57 | ⛔ do not write |
| #869 | G57 Coordinates B Absolute position — Current axis position in G57 | ⛔ do not write |
| #870 | G58 Coordinates X Absolute position — Current axis position in G58 | ⛔ do not write |
| #871 | G58 Coordinates Y Absolute position — Current axis position in G58 | ⛔ do not write |
| #872 | G58 Coordinates Z Absolute position — Current axis position in G58 | ⛔ do not write |
| #873 | G58 Coordinates A Absolute position — Current axis position in G58 | ⛔ do not write |
| #874 | G58 Coordinates B Absolute position — Current axis position in G58 | ⛔ do not write |
| #875 | G59 Coordinates X Absolute position — Current axis position in G59 | ⛔ do not write |
| #876 | G59 Coordinates Y Absolute position — Current axis position in G59 | ⛔ do not write |
| #877 | G59 Coordinates Z Absolute position — Current axis position in G59 | ⛔ do not write |
| #878 | G59 Coordinates A Absolute position — Current axis position in G59 | ⛔ do not write |
| #879 | G59 Coordinates B Absolute position — Current axis position in G59 | ⛔ do not write |
| #880 | MACH Coordinates X Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write |
| #881 | MACH Coordinates Y Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write |
| #882 | MACH Coordinates Z Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write |
| #883 | MACH Coordinates A Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write |
| #884 | MACH Coordinates B Absolute position — The current position of the axis in the machine CS. It can be edited (then the axis is "hoarse"). | ⛔ do not write |
| #885 | G92 Coordinates X Absolute position — Current axis position in G92 | ⛔ do not write |
| #886 | G92 Coordinates Y Absolute position — Current axis position in G92 | ⛔ do not write |
| #887 | G92 Coordinates Z Absolute position — Current axis position in G92 | ⛔ do not write |
| #888 | G92 Coordinates A Absolute position — Current axis position in G92 | ⛔ do not write |
| #889 | G92 Coordinates B Absolute position — Current axis position in G92 | ⛔ do not write |
| #1509 | buzzer control — Beeps with a squeaker |  |
| #1520 | Common Input port 01 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1521 | Common Input port 02 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1522 | Common Input port 03 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1523 | Common Input port 04 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1524 | Common Input port 05 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1525 | Common Input port 06 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1526 | Common Input port 07 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1527 | Common Input port 08 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1528 | Common Input port 09 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1529 | Common Input port 10 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1530 | Common Input port 11 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1531 | Common Input port 12 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1532 | Common Input port 13 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1533 | Common Input port 14 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1534 | Common Input port 15 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1535 | Common Input port 16 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1536 | Common Input port 17 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1537 | Common Input port 18 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1538 | Common Input port 19 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1539 | Common Input port 20 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1540 | Common Input port 21 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1541 | Common Input port 22 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1542 | Common Input port 23 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1543 | Common Input port 24 Status — Displays the status of the input port. Closed=0; open=1 |  |
| #1576 | *(no description in the table)* |  |
| #1620 | Analyze channel 1 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1621 | Analyze channel 2 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1622 | Analyze channel 3 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1623 | Analyze channel 4 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1624 | Analyze channel 5 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1625 | Analyze channel 6 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1626 | Analyze channel 7 Execution methods: 0：Request to Start or Restart 1:Request an Internal Pause 2:Request an external Pause — Utility function, manufacturer only. |  |
| #1630-#1636 | Analyze channel 1 status: -1:Idle 0:Working 1: Pause — Utility function, manufacturer only. | ⛔ do not write |
| #1700 | Function Key 1 Status |  |
| #1701 | Function Key 2 Status |  |
| #1702 | Function Key 3 Status |  |
| #1703 | Function Key 4 Status |  |
| #1704 | Function Key 5 Status |  |
| #1705 | Function Key 6 Status |  |
| #1706 | Function Key 7 Status |  |
| #1707 | Function Key 8 Status |  |
| #1708 | Function Key 9 Status |  |
| #1709 | Function Key 10 Status |  |
| #1710 | Function Key 11 Status |  |
| #1711 | Extended Key 1 Status |  |
| #1712 | Extended Key 2 Status |  |
| #1713 | Extended Key 3 Status |  |
| #1714 | Extended Key 4 Status |  |
| #1715 | Extended Key 5 Status |  |
| #1716 | Extended Key 6 Status |  |
| #1717 | Extended Key 7 Status |  |
| #1718 | Extended Key 8 Status |  |

## Parameter

| variables | description | note |
|---|---|---|
| #500 | Motor start speed mm/min | ⛔ do not write |
| #501 | X-axis pulses per mm p/mm | ⛔ do not write |
| #502 | Y-axis pulses per mm p/mm | ⛔ do not write |
| #503 | Z-axis pulses per mm p/mm | ⛔ do not write |
| #506 | 4th axis pulses per mm | ⛔ do not write |
| #507 | 4th-axis unit 0：pulse/degree — 1：pulse/revolution, If the axis is not used as a servo spindle, 507=1 cannot be set. Otherwise, the 4th axis will not work correctly during manual movements! | ⛔ do not write |
| #508 | pulse equivalent of the 5th Axis | ⛔ do not write |
| #509 | 5th-axis unit 0：pulse/degree — 1：pulse/revolution, If the axis is not used as a servo spindle, 509=1 cannot be set. Otherwise, the 5th axis will not work correctly during manual movements! | ⛔ do not write |
| #510 | *(no description in the table)* | ⛔ do not write |
| #511 | Time interval between DIR & PUL Unit: ns | ⛔ do not write |
| #512 | X-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #513 | Y-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #514 | Z-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #515 | 4th-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #516 | 5th-axis direction electric level 0: Low 1: High | ⛔ do not write |
| #517 | X-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #518 | Y-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #519 | Z-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #520 | 4th-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #521 | 5th-axis Pulse signal Electric Level 0: Low 1: High | ⛔ do not write |
| #522 | Enable axis mapping function — Note: When this function is enabled, the two axies are linked only/during machining. | ⛔ do not write |
| #523 | Primary axis selection | ⛔ do not write |
| #524 | Mapping axis selection | ⛔ do not write |
| #525-#529 | *(no description in the table)* | ⛔ do not write |
| #532-#534 | *(no description in the table)* | ⛔ do not write |
| #535 | X-axis max. speed in manual control | ⛔ do not write |
| #536 | Y axis max. speed in manual control | ⛔ do not write |
| #537 | Z axis max. speed in manual control | ⛔ do not write |
| #538 | 4th axis max. speed in manual control | ⛔ do not write |
| #539 | 5th axis max. speed in manual control | ⛔ do not write |
| #540 | HIGH speed of X-axis in manual control | ⛔ do not write |
| #541 | HIGH speed of Y-axis in manual control | ⛔ do not write |
| #542 | HIGH speed of Z-axis in manual control | ⛔ do not write |
| #543 | HIGH speed of 4th axis in manual control | ⛔ do not write |
| #544 | HIGH speed of 5th-axis in manual control | ⛔ do not write |
| #545 | LOW speed of X-axis in manual control | ⛔ do not write |
| #546 | LOW speed of Y-axis in manual control | ⛔ do not write |
| #547 | LOW speed of Z-axis in manual control | ⛔ do not write |
| #548 | LOW speed of 4th axis in manual control | ⛔ do not write |
| #549 | LOW speed of 5th axis in manual control | ⛔ do not write |
| #550 | X-axis start acceleration in manual control | ⛔ do not write |
| #551 | Y-axis start acceleration in manual control | ⛔ do not write |
| #552 | Z-axis start acceleration in manual control | ⛔ do not write |
| #553 | 4th-axis start acceleration in manual control | ⛔ do not write |
| #554 | 5th-axis start acceleration in manual control | ⛔ do not write |
| #555 | X-axis Stop acceleration in manual control | ⛔ do not write |
| #556 | Y-axis Stop acceleration in manual control | ⛔ do not write |
| #557 | Z-axis Stop acceleration in manual control | ⛔ do not write |
| #558 | 4th axis Stop acceleration in manual control | ⛔ do not write |
| #559 | 5th axis Stop acceleration in manual control | ⛔ do not write |
| #560 | Speed Selection: 0: G Code / 1: Default | ⛔ do not write |
| #561 | default operation speed — Unit: mm/min | ⛔ do not write |
| #562 | linear feed moves (G01) acceleration speed — Unit: mm/min | ⛔ do not write |
| #563 | Move as quickly as possible to a given point (G0) Speed — Unit: mm/min | ⛔ do not write |
| #564 | Maximum Machining speed — Unit: mm/min | ⛔ do not write |
| #565 | Z-axis lifting protection speed — Unit: mm/min | ⛔ do not write |
| #566 | Z-axis dropping protection speed — Unit: mm/min | ⛔ do not write |
| #567 | X-axis protection speed — Unit: mm/min | ⛔ do not write |
| #568 | Y-axis protection speed — Unit: mm/min | ⛔ do not write |
| #569 | Z axis safe height — Unit: mm | ⛔ do not write |
| #570 | Z-axis retraction distance when paused — Unit: mm, Z lift distance on pause | ⛔ do not write |
| #573 | Arc-interpolation algorithm 0: Hard algorithm 1: Soft algorithm | ⛔ do not write |
| #574 | Soft-arc algorithm linear precision Unit: mm | ⛔ do not write |
| #575 | Circular centrifugal acceleration | ⛔ do not write |
| #576 | Macro scan switch 0: Close 1: Open — Changing this setting should work without reboot | ⛔ do not write |
| #577 | macro program file main program no. | ⛔ do not write |
| #578 | Displays the number of the current working SC. 1-G54; 6-G59; 0-G92. Toggles them if the controller is restarted after the change. | ⛔ do not write, measured |
| #579 | Spindle interface type: 0: Analog 1: Plu&Dir 2: Multi-Speed | ⛔ do not write |
| #580 | Servo Spindle mapping axis: X / Y / Z / 4th / 5th Axis | ⛔ do not write |
| #581 | Spindle start delay Unit: S | ⛔ do not write |
| #582 | Maximum spindle speed | ⛔ do not write |
| #583 | Ignore the S command 0:No 1:Yes | ⛔ do not write |
| #584 | Stop spindle when program is paused 0:No 1:Yes — Whether to pause the spindle | ⛔ do not write |
| #585 | Default spindle speed Unit: rpm | ⛔ do not write |
| #588 | Multi-speed section counts 2-8 | ⛔ do not write |
| #589 | Spindle stop delay | ⛔ do not write |
| #590 | Action selection before starting 0: No Action 1: To Safety Z | ⛔ do not write |
| #591 | Z-axis movement mode during pause 0: No action 1：ZZ lift distance (#70) — Whether to raise Z on pause. | ⛔ do not write |
| #592 | Delay of M8/M9 commands Unit: S | ⛔ do not write |
| #593 | *(no description in the table)* | ⛔ do not write |
| #594 | Delay of M10/M11 commands Unit: S | ⛔ do not write |
| #595 | IO input filter time width Unit: ms | ⛔ do not write |
| #596 | Reset IO Configuration bit 01-16 — Changes the state of the outputs after a reboot | ⛔ do not write |
| #597 | Reset IO Configuration bit 17-21 — Changes the state of the outputs after a reboot | ⛔ do not write |
| #598 | Alarm output status configuration bit 01-16 | ⛔ do not write |
| #599 | Alarm output status configuration bit 17-21 | ⛔ do not write |
| #600 | Home mode 0: switch 1: Absolute — The function of determining the absolute position of the axes during loading (according to the encoder), instead of homing. If you set it to 1, it will try to determine the coordinates from the encoder after loading (of course, unsuccessfully). | ⛔ do not write |
| #601 | Servo absolute laps at the X-axis Home — The absolute position of the encoder in the X-axis, at the home position of the X-axis. Used in the M110 command (subroutine O10110 in slib-m) | ⛔ do not write |
| #602 | Servo absolute laps at the Y-axis Home — Absolute position of the encoder in the Y-axis, with the home position of the Y-axis. Used in the M111 command (subroutine O10111 in slib-m) | ⛔ do not write |
| #603 | Servo absolute laps at the Z-axis Home — The absolute position of the encoder in the Z axis, with the home position of the Z axis. Used in the M112 command (subroutine O10112 in slib-m) | ⛔ do not write |
| #604 | Servo absolute laps at the 4th-axis Home — Absolute position of the encoder in the A-axis, with the home position of the A-axis. Used in the M113 command (subroutine O10113 in slib-m) | ⛔ do not write |
| #605 | Servo absolute laps at the 5th-axis Home — Absolute encoder position on the B-axis, when the B-axis is in the home position. Used in the M114 command (subroutine O10114 in slib-m) | ⛔ do not write |
| #606 | Home detection times 1-5 — Number of visits to the homing sensor. You can not put 6. A glitch is possible. There will be a variable conflict. | ⛔ do not write |
| #607 | X-axis homing speed Unit: mm/min | ⛔ do not write |
| #608 | Y-axis homing speed Unit: mm/min | ⛔ do not write |
| #609 | Z-axis homing speed Unit: mm/min | ⛔ do not write |
| #610 | 4th-axis homing speed Unit: mm/min | ⛔ do not write |
| #611 | 5th-axis homing speed Unit: mm/min | ⛔ do not write |
| #612 | X-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #613 | Y-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #614 | Z-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #615 | 4th-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #616 | 5th-axis homing direction 0: Negative 1: Positive | ⛔ do not write |
| #617 | Maximum error of home switch | ⛔ do not write |
| #618 | Second precision positioning speed | ⛔ do not write |
| #619-#621 | *(no description in the table)* | ⛔ do not write |
| #622 | Mach position after X go home | ⛔ do not write |
| #623 | Mach position after Y go home | ⛔ do not write |
| #624 | Mach position after Z go home | ⛔ do not write |
| #625 | Mach position after 4th go home | ⛔ do not write |
| #626 | Mach position after 5th go home | ⛔ do not write |
| #627 | Home after booting 0：Yes 1: No | ⛔ do not write |
| #628 | Is the Floating tool set valid? | ⛔ do not write |
| #629 | Floating tool set thickness | ⛔ do not write |
| #630 | Is the fixed tool set valid? | ⛔ do not write |
| #631 | Probe detection times 1-5 — Probing cycle count | ⛔ do not write |
| #632 | Initial speed of Probing Unit: mm/min | ⛔ do not write |
| #634 | Setting the tool to its original position — 对刀初始位置 | ⛔ do not write |
| #635 | Fixed probe X mach pos Unit: mm | ⛔ do not write |
| #636 | Fixed probe Y mach pos Unit: mm | ⛔ do not write |
| #637 | Fixed probe Z mach pos Unit: mm | ⛔ do not write |
| #638 | Fixed probe 4th mach pos Unit: mm | ⛔ do not write |
| #639 | Fixed probe 5th mach pos Unit: mm | ⛔ do not write |
| #640 | Retraction distance after probe action is finished Unit: mm — Retraction distance after the end of probe | ⛔ do not write |
| #641-#649 | *(no description in the table)* | ⛔ do not write |
| #650 | Stop mode when X-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #651 | Stop mode when Y-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #652 | Stop mode when Z-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #653 | Stop mode when 4th-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #654 | Stop mode when 5th-axis hard limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #655 | Enable software limits 0: No 1: Yes | ⛔ do not write |
| #656 | Stop mode when X-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #657 | Stop mode when Y-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #658 | Stop mode when Z-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #659 | Stop mode when 4th-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #660 | Stop mode when 5th-axis soft limit trigger 0：Deceleration 1：Emergency | ⛔ do not write |
| #661 | X Soft limit of Negative position | ⛔ do not write |
| #662 | Y Soft limit of negative position | ⛔ do not write |
| #663 | Z Soft limit of Negative position | ⛔ do not write |
| #664 | 4th Soft limit of Negative position | ⛔ do not write |
| #665 | 5th Soft limit of Negative position | ⛔ do not write |
| #666 | X Soft limit of positive position | ⛔ do not write |
| #667 | Y Soft limit of positive position | ⛔ do not write |
| #668 | Z Soft limit of positive position | ⛔ do not write |
| #669 | 4th Soft limit of Positive position | ⛔ do not write |
| #670 | 5th Soft limit of Positive position | ⛔ do not write |
| #671 | MPG precision Mode control mode: 0:Active 1:Disable | ⛔ do not write |
| #672 | MPG X1 distance 0.001-0.01 | ⛔ do not write |
| #673 | MPG Estop signal enable 0：Disable 1：Enable | ⛔ do not write |
| #674 | MPG Estop signal Level define 0：Low 1：High | ⛔ do not write |
| #675 | MPG working direction 0:Positive 1：Negative | ⛔ do not write |
| #676 | MPG X1 Working speed | ⛔ do not write |
| #677 | MPG X10 Working speed | ⛔ do not write |
| #678 | MPG X100 Working speed | ⛔ do not write |
| #679 | Magnification adjustment increment value when Handwheel guide stopped | ⛔ do not write |
| #680 | MPG Acceleration/deceleration rate adjustment increment value | ⛔ do not write |
| #681 | MPG Movement acceleration of X axis | ⛔ do not write |
| #682 | MPG Movement acceleration of Y axis | ⛔ do not write |
| #683 | MPG Movement acceleration of Z axis | ⛔ do not write |
| #684 | MPG Movement acceleration of 4th axis | ⛔ do not write |
| #685 | MPG Movement acceleration of 5th axis | ⛔ do not write |
| #686-#689 | *(no description in the table)* | ⛔ do not write |
| #690 | X axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #691 | Y axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #692 | axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #693 | 4th axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #694 | 5th axis Backlash 0：Disable 1:Enable | ⛔ do not write |
| #695 | X axis ballscrew Backlash unit: mm | ⛔ do not write |
| #696 | Y axis ballscrew Backlash unit: mm | ⛔ do not write |
| #697 | Z axis ballscrew Backlash unit: mm | ⛔ do not write |
| #698 | 4th axis ballscrew Backlash unit: mm | ⛔ do not write |
| #699 | 5th axis ballscrew Backlash unit: mm | ⛔ do not write |
| #700 | Backlash compensation speed 单位:mm/min | ⛔ do not write |
| #701 | Total number of processed pieces — The total number of completed programs. Upgrades with the completion of M30. Doesn't reset on reboot. | ⛔ do not write |
| #703 | Number of cyclic processing pieces (Planned number of processed pieces) — Number of repetitions for command M47 | ⛔ do not write |
| #704-#709 | *(no description in the table)* | ⛔ do not write |
| #710 | K1 key function definition — If you set #710=2536, this will hang the controller when K1 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #711 | K2 key function definition — If you set #711=2537, this will hang the controller when K2 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #712 | K3 key function definition — If you set #712=2538, this will hang the controller when K3 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #713 | K4 key function definition — If you set #713=2539, this will hang the controller when K4 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #714 | K5 key function definition — If you set #714=2540, this will hang the controller when K5 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #715 | K6 key function definition — If you set #715=2541, this will hang the controller when K6 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #716 | K7 key function definition — If you set #716=2541, this will hang the controller when K7 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #717 | K8 key function definition — If you set #717=2542, this will hang the controller when K8 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #718 | K9 key function definition — If you set #718=2543, this will hang the controller when K9 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #719 | K10 key function definition — If you set #719=2544, this will hang the controller when K10 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #720 | Go Home Before Processing? 0：No 1：Yes | ⛔ do not write |
| #721 | References speed when working on radius 5mm Arc | ⛔ do not write |
| #722 | The 4th Axis processing protection speed | ⛔ do not write |
| #723 | The 5th Axis processing protection speed | ⛔ do not write |
| #724 | Set the Drilling cycle Retraction amount | ⛔ do not write |
| #730 | After processing finished — the machine: 0：Don't move 1：Back to References Point 2：Back to Zero | ⛔ do not write |
| #731 | K11 key function definition — If #731=2545 is set, this will hang the controller when K11 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #732 | K12 key function definition — If you set #732=2546, this will hang the controller when K12 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #733 | K13 key function definition — If you set #733=2547, this will hang the controller when K13 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #734 | K14 key function definition — If you set #734=2548, this will hang the controller when K14 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #735 | Home offset of X axis | ⛔ do not write |
| #736 | Home offset of Y axis | ⛔ do not write |
| #737 | Home offset of Z axis | ⛔ do not write |
| #738 | Home offset of 4th axis | ⛔ do not write |
| #739 | Home offset of 5th axis | ⛔ do not write |
| #740 | Language Setting 0:English 1:Chinese 2:Russian | ⛔ do not write |
| #741 | Buzzer enable 0:Forbit 1:Enable | ⛔ do not write |
| #743 | *(no description in the table)* | ⛔ do not write |
| #744 | Toolpath Realtime Display 0 :Forbit 1:Enable | ⛔ do not write |
| #745 | Toolpath display mode 0:Status 1:Line 2: 3D | ⛔ do not write |
| #747 | Interpolation period Second 0.001-0.01 | ⛔ do not write |
| #748 | User self-design LOGO Display time | ⛔ do not write |
| #751 | K15 key function definition — If you set #751=2549, this will hang the controller when K15 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #752 | K16 key function definition — If you set #752=2550, this will hang the controller when K16 is pressed. The button presses itself endlessly. | ⛔ do not write |
| #753 | *(no description in the table)* | ⛔ do not write |
| #764 | Alarm Enable output configuration 01-16 | ⛔ do not write |
| #765 | Alarm Enable output configuration 17-32 | ⛔ do not write |
| #766 | Serial 1 baud rate | ⛔ do not write |
| #767 | Serial 2 baud rate | ⛔ do not write |
| #768 | External keyboard type | ⛔ do not write |
| #769 | Debug printing enable — Enables feature #1503 - Prompt information | ⛔ do not write |
| #771 | maximum contour | ⛔ do not write |
| #772 | extern key 1 Function | ⛔ do not write |
| #773 | extern key 2 Function | ⛔ do not write |
| #774 | external key 3 Function | ⛔ do not write |
| #775 | extern key 4 Function | ⛔ do not write |
| #776 | extern key 5 Function | ⛔ do not write |
| #777 | extern key 6 Function | ⛔ do not write |
| #778 | USB keyboard type 0：Close 1：Standard keyboard 2：USB Scanner | ⛔ do not write |
| #779 | Modbus RTU (The Barcode processing file storage) | ⛔ do not write |
| #780 | Line corner acceleration | ⛔ do not write |
| #781 | J parameter of S-type acceleration curve | ⛔ do not write |
| #782 | G00 Acceleration | ⛔ do not write |
| #783 | Barcode scanning processing enable | ⛔ do not write |
| #784 | network boot mode | ⛔ do not write |
| #785 | X-axis max. Acceleration G00 | ⛔ do not write |
| #786 | Y axis max. Acceleration G00 | ⛔ do not write |
| #787 | Z axis max. Acceleration G00 | ⛔ do not write |
| #788 | 4th axis max. Acceleration G00 | ⛔ do not write |
| #789 | 5th axis max. Acceleration G00 | ⛔ do not write |
| #795-#799 | *(no description in the table)* | ⛔ do not write |
| #800 | G92 Coordinates Home Offset X — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #801 | G92 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #802 | G92 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #803 | G92 Coordinates Home Offset A — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #804 | G92 Coordinates Home Offset B — Offset, relative to machine coordinate, for G92 | ⛔ do not write |
| #805 | G54 Coordinates Home Offset X — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #806 | G54 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #807 | G54 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #808 | G54 Coordinates Home Offset A — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #809 | G54 Coordinates Home Offset B — Offset, relative to machine coordinate, for G54 | ⛔ do not write |
| #810 | G55 Coordinates Home Offset X — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #811 | G55 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #812 | G55 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #813 | G55 Coordinates Home Offset A — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #814 | G55 Coordinates Home Offset B — Offset, relative to machine coordinate, for G55 | ⛔ do not write |
| #815 | G56 Coordinates Home Offset X — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #816 | G56 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #817 | G56 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #818 | G56 Coordinates Home Offset A — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #819 | G56 Coordinates Home Offset B — Offset, relative to machine coordinate, for G56 | ⛔ do not write |
| #820 | G57 Coordinates Home Offset X — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #821 | G57 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #822 | G57 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #823 | G57 Coordinates Home Offset A — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #824 | G57 Coordinates Home Offset B — Offset, relative to machine coordinate, for G57 | ⛔ do not write |
| #825 | G58 Coordinates Home Offset X — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #826 | G58 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #827 | G58 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #828 | G58 Coordinates Home Offset A — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #829 | G58 Coordinates Home Offset B — Offset, relative to machine coordinate, for G58 | ⛔ do not write |
| #830 | G59 Coordinates Home Offset X — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #831 | G59 Coordinates Home Offset Y — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #832 | G59 Coordinates Home Offset Z — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #833 | G59 Coordinates Home Offset A — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #834 | G59 Coordinates Home Offset B — Offset, relative to machine coordinate, for G59 | ⛔ do not write |
| #840 | X-axis Coordinate offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #841 | Y-axis Coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #842 | Z axis coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #843 | 4th axis coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #844 | 5th axis Coordinates offset — Sets a constant offset of the axes in all working CS. Added to the main offset of the SK. Displayed in the "Coord set" menu. Retained after reboot. Can only be changed if after #840=0 there is a command to move the axes. Or via MDI. Perhaps other commands will do. | ⛔ do not write |
| #845-#849 | *(no description in the table)* | ⛔ do not write |
| #890-#899 | *(no description in the table)* | ⛔ do not write |
| #900 | H01 offset mm | ⛔ do not write |
| #901 | H02 offset mm | ⛔ do not write |
| #902 | H03 offset mm | ⛔ do not write |
| #903 | H04 offset mm | ⛔ do not write |
| #904 | H05 offset mm | ⛔ do not write |
| #905 | H06 offset mm | ⛔ do not write |
| #906 | H07 offset mm | ⛔ do not write |
| #907 | H08 offset mm | ⛔ do not write |
| #908 | H09 offset mm | ⛔ do not write |
| #909 | H10 offset mm | ⛔ do not write |
| #910 | H11 offset mm | ⛔ do not write |
| #911 | H12 offset mm | ⛔ do not write |
| #912 | H13 offset mm | ⛔ do not write |
| #913 | H14 offset mm | ⛔ do not write |
| #914 | H15 offset mm | ⛔ do not write |
| #915 | H16 offset mm | ⛔ do not write, measured |
| #916 | Possibly H17 offset mm | ⛔ do not write |
| #917 | Possibly H18 offset mm | ⛔ do not write |
| #918 | Possibly H19 offset mm | ⛔ do not write |
| #919 | Possibly H20 offset mm | ⛔ do not write |
| #920 | D01 offset mm | ⛔ do not write |
| #921 | D02 offset mm | ⛔ do not write |
| #922 | D03 offset mm | ⛔ do not write |
| #923 | D04 offset mm | ⛔ do not write |
| #924 | D05 offset mm | ⛔ do not write |
| #925 | D06 offset mm | ⛔ do not write |
| #926 | D07 offset mm | ⛔ do not write |
| #927 | D08 offset mm | ⛔ do not write |
| #928 | D09 offset mm | ⛔ do not write |
| #929 | D10 offset mm | ⛔ do not write |
| #930 | D11 offset mm | ⛔ do not write |
| #931 | D12 offset mm | ⛔ do not write |
| #932 | D13 offset mm | ⛔ do not write |
| #933 | D14 offset mm | ⛔ do not write |
| #934 | D15 offset mm | ⛔ do not write |
| #935 | D16 offset mm | ⛔ do not write |
| #936 | Possibly D17 offset mm | ⛔ do not write |
| #937 | Possibly D18 offset mm | ⛔ do not write |
| #938 | Possibly D19 offset mm | ⛔ do not write |
| #939 | Possibly D20 offset mm | ⛔ do not write |
| #943 | 4th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #944 | 5th Coordinate axis Name 0-5 : XYZAB | ⛔ do not write |
| #945 | *(no description in the table)* | ⛔ do not write |
| #949 | 4th axis type 0: Linear axis; 1: Rotation axis | ⛔ do not write |
| #950 | 5th axis type 0: Linear axis; 1: Rotation axis | ⛔ do not write |
| #951-#954 | *(no description in the table)* | ⛔ do not write |
| #955 | 4th axis Offset X | ⛔ do not write |
| #956 | 5th-axis Offset X | ⛔ do not write |
| #957-#960 | *(no description in the table)* | ⛔ do not write |
| #961 | 4th-axis Offset Y | ⛔ do not write |
| #962 | 5th axis Offset Y | ⛔ do not write |
| #963-#966 | *(no description in the table)* | ⛔ do not write |
| #967 | 4th axis Offset Z | ⛔ do not write |
| #968 | 5th axis Offset Z | ⛔ do not write |
| #969-#972 | *(no description in the table)* | ⛔ do not write |
| #973 | 4th-axis Vector U | ⛔ do not write |
| #974 | 5th-axis Vector U | ⛔ do not write |
| #975-#978 | *(no description in the table)* | ⛔ do not write |
| #979 | 4th axis Vector V | ⛔ do not write |
| #980 | 5th axis Vector V | ⛔ do not write |
| #981-#984 | *(no description in the table)* | ⛔ do not write |
| #985 | 4th-axis Vector W | ⛔ do not write |
| #986 | 5th-axis Vector W | ⛔ do not write |
| #987 | *(no description in the table)* | ⛔ do not write |
| #988 | Programming axis of physical axis 1 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #989 | Programming axis of physical axis 2 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #990 | Programming axis of physical axis 3 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #991 | Programming axis of physical axis 4 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #992 | Programming axis of physical axis 5 0:None 1:X 2:Y 3:Z 4:4th 5:5th | ⛔ do not write |
| #993-#999 | *(no description in the table)* | ⛔ do not write |
| #1000 | X-axis servo alarm signal | ⛔ do not write |
| #1001 | X axis servor Alarm signal Enable | ⛔ do not write |
| #1002 | axis servo Alarm signal electronic level | ⛔ do not write |
| #1003 | Y-axis servo alarm signal | ⛔ do not write |
| #1004 | Y axis servo Alarm signal Enable | ⛔ do not write |
| #1005 | Y axis servo Alarm signal electronic level | ⛔ do not write |
| #1006 | Z-axis servo alarm signal | ⛔ do not write |
| #1007 | Z axis servo Alarm signal enable | ⛔ do not write |
| #1008 | Z axis servo Alarm signal electronic level | ⛔ do not write |
| #1009 | 4th axis servo Alarm signal Input Signal (Spindle alarm signal) | ⛔ do not write |
| #1010 | 4th axis servo Alarm signal Enable | ⛔ do not write |
| #1011 | 4th axis servo Alarm signal electronic level | ⛔ do not write |
| #1012 | 5th axis servo alarm signal | ⛔ do not write |
| #1013 | 5th axis servo Alarm signal Enable | ⛔ do not write |
| #1014 | 5th axis servo Alarm signal electronic level | ⛔ do not write |
| #1015 | Negative X-axis hard limit signal | ⛔ do not write |
| #1016 | Negative X-axis hard limit signal ENABLE | ⛔ do not write |
| #1017 | Negative X-axis hard limit signal Effective level | ⛔ do not write |
| #1018 | Negative Y-axis hard limit signal | ⛔ do not write |
| #1019 | Negative Y-axis hard limit signal ENABLE | ⛔ do not write |
| #1020 | Negative Y-axis hard limit signal Effective level | ⛔ do not write |
| #1021 | Negative Z-axis hard limit signal | ⛔ do not write |
| #1022 | Negative Z-axis hard limit signal ENABLE | ⛔ do not write |
| #1023 | Negative Z-axis hard limit signal Effective level | ⛔ do not write |
| #1024 | Negative 4th-axis hard limit signal | ⛔ do not write |
| #1025 | Negative 4th-axis hard limit signal ENABLE | ⛔ do not write |
| #1026 | Negative 4th-axis hard limit signal Effective level | ⛔ do not write |
| #1027 | Negative 5th axis hard limit signal | ⛔ do not write |
| #1028 | Negative 5th-axis hard limit signal ENABLE | ⛔ do not write |
| #1029 | Negative 5th-axis hard limit signal Effective level | ⛔ do not write |
| #1030 | Positive X-axis hard limit signal | ⛔ do not write |
| #1031 | Positive X-axis hard limit signal ENABLE | ⛔ do not write |
| #1032 | Positive X-axis hard limit signal Effective level | ⛔ do not write |
| #1033 | Positive Y-axis hard limit signal | ⛔ do not write |
| #1034 | Positive Y-axis hard limit signal ENABLE | ⛔ do not write |
| #1035 | Positive Y-axis hard limit signal Effective level | ⛔ do not write |
| #1036 | Positive Z-axis hard limit signal | ⛔ do not write |
| #1037 | Positive Z-axis hard limit signal ENABLE | ⛔ do not write |
| #1038 | Positive Z-axis hard limit signal Effective level | ⛔ do not write |
| #1039 | Positive 4th axis hard limit signal | ⛔ do not write |
| #1040 | Positive 4th-axis hard limit signal ENABLE | ⛔ do not write |
| #1041 | Positive 4th-axis hard limit signal Effective level | ⛔ do not write |
| #1042 | Positive 5th axis hard limit signal | ⛔ do not write |
| #1043 | Positive 5th-axis hard limit signal ENABLE | ⛔ do not write |
| #1044 | Positive 5th-axis hard limit signal Effective level | ⛔ do not write |
| #1045 | X-axis zero signal | ⛔ do not write |
| #1046 | X-axis zero signal ENABLE | ⛔ do not write |
| #1047 | X-axis zero signal effective level | ⛔ do not write |
| #1048 | Y-axis zero signal | ⛔ do not write |
| #1049 | Y-axis zero signal ENABLE | ⛔ do not write |
| #1050 | Y-axis zero signal effective level | ⛔ do not write |
| #1051 | Z-axis zero signal | ⛔ do not write |
| #1052 | Z-axis zero signal ENABLE | ⛔ do not write |
| #1053 | Z-axis zero signal effective level | ⛔ do not write |
| #1054 | 4th axis zero signal | ⛔ do not write |
| #1055 | 4th axis zero signal ENABLE | ⛔ do not write |
| #1056 | 4th axis zero signal effective level | ⛔ do not write |
| #1057 | 5th axis zero signal | ⛔ do not write |
| #1058 | 5th axis zero signal ENABLE | ⛔ do not write |
| #1059 | 5th axis zero signal effective level | ⛔ do not write |
| #1060-#1074 | *(no description in the table)* | ⛔ do not write |
| #1075 | Fixed probe signal | ⛔ do not write |
| #1076 | Fixed probe signal ENABLE | ⛔ do not write |
| #1077 | Fixed probe signal effective level | ⛔ do not write |
| #1078 | Floating probe signal | ⛔ do not write |
| #1079 | Floating Probe signal ENABLE | ⛔ do not write |
| #1080 | Floating Probe signal effective level | ⛔ do not write |
| #1081 | External Key 1 Signal | ⛔ do not write |
| #1082 | External Key 1 Enable | ⛔ do not write |
| #1083 | External Key 1 effective level | ⛔ do not write |
| #1084 | External Key 2Signal | ⛔ do not write |
| #1085 | External Key 2 Enable | ⛔ do not write |
| #1086 | External Key 2 effective level | ⛔ do not write |
| #1087 | External Key 3 Signal | ⛔ do not write |
| #1088 | External Key 3 Enable | ⛔ do not write |
| #1089 | External Key 3 effective level | ⛔ do not write |
| #1090 | External Key 4Signal | ⛔ do not write |
| #1091 | External Key 4 Enable | ⛔ do not write |
| #1092 | External Key 4 effective level | ⛔ do not write |
| #1093 | External Key 5 Signal | ⛔ do not write |
| #1094 | External Key 5 Enable | ⛔ do not write |
| #1095 | External Key 5 effective level | ⛔ do not write |
| #1096 | External Key 6Signal | ⛔ do not write |
| #1097 | External Key 6 Enable | ⛔ do not write |
| #1098 | External Key 6 effective level | ⛔ do not write |
| #1099 | External start signal Signal | ⛔ do not write |
| #1100 | External start signal enable | ⛔ do not write |
| #1101 | External Start signal Effective level | ⛔ do not write |
| #1102 | External Pause Signal | ⛔ do not write |
| #1103 | External Pause Signal Enable | ⛔ do not write |
| #1104 | External Pause signal Effective level | ⛔ do not write |
| #1105 | External Estop signal Signal | ⛔ do not write |
| #1106 | External Estop Signal Enable | ⛔ do not write |
| #1107 | External Estop signal Effective level | ⛔ do not write |
| #1108-#1119 | *(no description in the table)* | ⛔ do not write |
| #1120 | Spindle stop signal(M300) Port Number | ⛔ do not write |
| #1121 | Spindle Stop detection Enable | ⛔ do not write |
| #1122 | Spindle Stop detection effective level | ⛔ do not write |
| #1123 | Tool release input signal port number | ⛔ do not write |
| #1124 | Tool Release Detection Enable | ⛔ do not write |
| #1125 | Tool release detection effective level | ⛔ do not write |
| #1126 | Tool lock input signal(M302) Port Number | ⛔ do not write |
| #1127 | Tool lock detection signal Enable | ⛔ do not write |
| #1128 | Tool lock detection signal effective level | ⛔ do not write |
| #1129 | Tool open input signal(M303) Port Number | ⛔ do not write |
| #1130 | Tool open detection signal Enable | ⛔ do not write |
| #1131 | Tool open detection effective level | ⛔ do not write |
| #1132 | Dust cover open/close input signal(M305/M306) Port Number | ⛔ do not write |
| #1133 | Dust cover open/close signal enable | ⛔ do not write |
| #1134 | Dust cover open/close signal effective level | ⛔ do not write |
| #1135 | Inverter alarm input signal port Number | ⛔ do not write |
| #1136 | Inverter alarm input signal enable | ⛔ do not write |
| #1137 | Inverter alarm input signal effective level | ⛔ do not write |
| #1138 | Self-define Alarm signal 1 detection input Port | ⛔ do not write |
| #1139 | Self-define Alarm signal 1 ENABLE | ⛔ do not write |
| #1140 | Self-define Alarm signal 1 Effective level | ⛔ do not write |
| #1141 | Self-define Alarm signal 2 detection input Port | ⛔ do not write |
| #1142 | Self-define Alarm signal 2 ENABLE | ⛔ do not write |
| #1143 | Self-define Alarm signal 2 Effective level | ⛔ do not write |
| #1144 | Self-define Alarm signal 3 detection input Port | ⛔ do not write |
| #1145 | Self-define Alarm signal 3 ENABLE | ⛔ do not write |
| #1146 | Self-define Alarm signal 3 Effective level | ⛔ do not write |
| #1147 | Self-define Alarm signal 4 detection input Port | ⛔ do not write |
| #1148 | Self-define Alarm signal 4 ENABLE | ⛔ do not write |
| #1149 | Self-define Alarm signal 4 Effective level | ⛔ do not write |
| #1150 | Self-define Alarm signal 5 detection input Port | ⛔ do not write |
| #1151 | Self-define Alarm signal 5 ENABLE | ⛔ do not write |
| #1152 | Self-define Alarm signal 5 Effective level | ⛔ do not write |
| #1153 | User storage — Safe park X position | ⛔ do not write |
| #1154 | User storage — Safe park Y position | ⛔ do not write |
| #1155 | User storage — Tool change park X position | ⛔ do not write |
| #1156 | User storage — Tool change park Y position | ⛔ do not write |
| #1157-#1169 | *(no description in the table)* | ⛔ do not write |
| #1170-#1175 | User storage — Probe config | ⛔ do not write |
| #1176-#1193 | *(no description in the table)* | ⛔ do not write |
| #1194 | Input port for M307 command | ⛔ do not write |
| #1195 | Enable input for command M307 | ⛔ do not write |
| #1196 | Active level for command M307 | ⛔ do not write |
| #1197 | Tool close input signal(M304) Port Number | ⛔ do not write |
| #1198 | Tool close input signal Enable | ⛔ do not write |
| #1199 | Tool close input signal Effective level | ⛔ do not write |
| #1200-#1214 | *(no description in the table)* | ⛔ do not write |
| #1215 | Spindle Forward Rotation Signal Port Number | ⛔ do not write |
| #1216 | Spindle forward signal ENABLE | ⛔ do not write |
| #1217 | Spindle forward signal effective level | ⛔ do not write |
| #1218 | Spindle Reverse Rotation Signal Port Number | ⛔ do not write |
| #1219 | Spindle Reverse signal ENABLE | ⛔ do not write |
| #1220 | Spindle Reverse signal effective level | ⛔ do not write |
| #1221 | Spindle speed section 1 signal Port Number | ⛔ do not write |
| #1222 | Spindle speed section 1 signal enable | ⛔ do not write |
| #1223 | Spindle speed section 1 signal Effective level | ⛔ do not write |
| #1224 | Spindle speed section 2 signal Port Number | ⛔ do not write |
| #1225 | Spindle speed section 2 signal enable | ⛔ do not write |
| #1226 | Spindle speed section 2 signal Effective level | ⛔ do not write |
| #1227 | Spindle speed section 3 signal Port Number | ⛔ do not write |
| #1228 | Spindle speed section 3 signal enable | ⛔ do not write |
| #1229 | Spindle speed section 3 signal Effective level | ⛔ do not write |
| #1230 | Cooling M8/M9 control signal Port Number | ⛔ do not write |
| #1231 | Cooling M8/M9 control signal enable | ⛔ do not write |
| #1232 | Cooling M8/M9 control signal Effective level | ⛔ do not write |
| #1233 | Lubrication M10/M11 control signal Port Number | ⛔ do not write |
| #1234 | Lubrication M10/M11 control signal enable | ⛔ do not write |
| #1235 | Lubrication M10/M11 control signal Effective level | ⛔ do not write |
| #1236 | System alarm signal Output Port Number | ⛔ do not write |
| #1237 | System alarm signal Output Enable | ⛔ do not write |
| #1238 | System alarm signal Output Effective level | ⛔ do not write |
| #1239 | System working indicator signal output port number | ⛔ do not write |
| #1240 | System working indicator signal output Enable | ⛔ do not write |
| #1241 | System working indicator signal output effective level | ⛔ do not write |
| #1242 | System brake signal output port number — Electromagnetic axle brake (e.g. to prevent Z from dropping) | ⛔ do not write |
| #1243 | System brake signal output Enable — Electromagnetic axle brake (e.g. to prevent Z from dropping) | ⛔ do not write |
| #1244 | System brake signal output Effective level — Electromagnetic axle brake (e.g. to prevent Z from dropping) | ⛔ do not write |
| #1245 | System ready indicator signal output port number | ⛔ do not write |
| #1246 | System ready indicator signal output Enable | ⛔ do not write |
| #1247 | System ready indicator signal output Effective level | ⛔ do not write |
| #1248-#1249 | *(no description in the table)* | ⛔ do not write |
| #1253 | Tool launch/retract signal(M152/M153) output port number | ⛔ do not write |
| #1254 | Tool launch/retract signal(M152/M153) output Enable | ⛔ do not write |
| #1255 | Tool launch/retract signal(M152/M153) output Effective level | ⛔ do not write |
| #1256 | Front positioning on/off signal(M156/M157) output port number | ⛔ do not write |
| #1257 | Front positioning on/off signal(M156/M157) output Enable | ⛔ do not write |
| #1258 | Front positioning on/off signal(M156/M157) output Effective level | ⛔ do not write |
| #1259 | Vacuum pump on/off output signal(M158/M159) output port number | ⛔ do not write |
| #1260 | Vacuum pump on/off output signal(M158/M159) output enable | ⛔ do not write |
| #1261 | Vacuum pump on/off output signal(M158/M159) output Effective level | ⛔ do not write |
| #1262 | Dust cover open/close output signal (M150/M151) output port number | ⛔ do not write |
| #1263 | Dust cover open/close output signal (M150/M151) output Enable | ⛔ do not write |
| #1264 | Dust cover open/close output signal (M150/M151) output Effective level | ⛔ do not write |
| #1265 | Push cylinder open/close signal(M160/M161) output port number | ⛔ do not write |
| #1266 | Push cylinder open/close signal(M160/M161) output Enable | ⛔ do not write |
| #1267 | Push cylinder open/close signal(M160/M161) output Effective level | ⛔ do not write |
| #1268 | Vacuum cleaner on/off signal (M162/M163) output port number | ⛔ do not write |
| #1269 | Vacuum cleaner on/off signal (M162/M163) output Enable | ⛔ do not write |
| #1270 | Vacuum cleaner on/off signal (M162/M163) output Effective level | ⛔ do not write |
| #1271 | Left positioning on/off signal(M164/M165) output port number | ⛔ do not write |
| #1272 | Left positioning on/off signal(M164/M165) Enable | ⛔ do not write |
| #1273 | Left positioning on/off signal(M164/M165) Effective level | ⛔ do not write |
| #1274 | Vacuum valve open/close signal(M1166/M167) output port number | ⛔ do not write |
| #1275 | Vacuum valve open/close signal(M1166/M167) Enable | ⛔ do not write |
| #1276 | Vacuum valve open/close signal(M1166/M167) Effective level | ⛔ do not write |
| #1277 | Multi-process 1 open/close signal(M168/M169) output port number | ⛔ do not write |
| #1278 | Multi-process 1 open/close signal(M168/M169) Enable | ⛔ do not write |
| #1279 | Multi-process 1 open/close signal(M168/M169) Effective level | ⛔ do not write |
| #1280 | Multi-process 2 open/close signal(M170/M171) output port number | ⛔ do not write |
| #1281 | Multi-process 2 open/close signal(M170/M171) Enable | ⛔ do not write |
| #1282 | Multi-process 2 open/close signal(M170/M171) Effective level | ⛔ do not write |
| #1283 | Multi-process 3 open/close signal(M170/M171) output port number | ⛔ do not write |
| #1284 | Multi-process 3 open/close signal(M170/M171) Enable | ⛔ do not write |
| #1285 | Multi-process 3 open/close signal(M170/M171) Effective level | ⛔ do not write |
| #1286 | Multi-process 4 open/close signal(M170/M171) output port number | ⛔ do not write |
| #1287 | Multi-process 4 open/close signal(M170/M171) Enable | ⛔ do not write |
| #1288 | Multi-process 4 open/close signal(M170/M171) Effective level | ⛔ do not write |
| #1289 | Cooling 1 on/off signal(M176/M177) output port number | ⛔ do not write |
| #1290 | Cooling 1 on/off signal(M176/M177) Enable | ⛔ do not write |
| #1291 | Cooling 1 on/off signal(M176/M177) Effective level | ⛔ do not write |
| #1292 | Cooling 2 on/off signal(M176/M177) output port number | ⛔ do not write |
| #1293 | Cooling 2 on/off signal(M176/M177) Enable | ⛔ do not write |
| #1294 | Cooling 2 on/off signal(M176/M177) Effective level | ⛔ do not write |
| #1295 | Servo spindle enable output port — Switches analog/servo spindle (for machines with autochange) | ⛔ do not write |
| #1296 | Enable servo spindle enable output port — Switches analog/servo spindle (for machines with autochange) | ⛔ do not write |
| #1297 | Servo spindle enable port output level — Switches analog/servo spindle (for machines with autochange) | ⛔ do not write |
| #1298-#1299 | *(no description in the table)* | ⛔ do not write |
| #1300 | current tool number — The current tool number. You can change. Then change the tool number on the main screen. | ⛔ do not write |
| #1301 | Tool capacity | ⛔ do not write |
| #1302 | Tool magazine type 0:NULL 1：Multiple 2：Follow row 3：Fixed row 4：Disk | ⛔ do not write |
| #1303 | The virtual Tool function turned on | ⛔ do not write |
| #1304 | Is the tool change prompt valid? — Whether to pause and prompt to change the tool when the tool change. | ⛔ do not write |
| #1305 | Automatic tool probe after tool change | ⛔ do not write |
| #1306 | The highest pos when chang Tool | ⛔ do not write |
| #1307 | The lowest pos when chang Tool | ⛔ do not write |
| #1308 | X-axis Front position for Tool change in Mach coordinate | ⛔ do not write |
| #1309 | Y-axis Front position for Tool change in Mach coordinate | ⛔ do not write |
| #1310 | Z-axis Front position for Tool change in Mach coordinate | ⛔ do not write |
| #1311 | Spindle move speed when changing the tool | ⛔ do not write |
| #1312 | Z-axis lifting speed when changing the tool | ⛔ do not write |
| #1313 | The magazine horizontally moving speed | ⛔ do not write |
| #1314 | Spindle lock output delay | ⛔ do not write |
| #1315 | After tool change — Back to the position before the tool change | ⛔ do not write |
| #1316 | X mach pos when manually changing the tool | ⛔ do not write |
| #1317 | Y mach pos when manually changing the tool | ⛔ do not write |
| #1318 | Z mach pos when manually changing the tool | ⛔ do not write |
| #1319 | Z axis downward movement speed | ⛔ do not write |
| #1320 | Pushing start X mach pos | ⛔ do not write |
| #1321 | Pushing start Y mach pos | ⛔ do not write |
| #1322 | push delay | ⛔ do not write |
| #1323 | Pushing end X mach pos | ⛔ do not write |
| #1324 | Pushing end Y mach pos | ⛔ do not write |
| #1325-#1326 | Pushing completed X mach pos | ⛔ do not write |
| #1327 | pushing speed | ⛔ do not write |
| #1328 | 4th-Axis Mach position before tool change | ⛔ do not write |
| #1329 | 5th-Axis Mach position before tool change | ⛔ do not write |
| #1330 | X coordinate of No.1 Tool | ⛔ do not write |
| #1331 | X coordinate of No.2 Tool | ⛔ do not write |
| #1332 | X coordinate of No.3 Tool | ⛔ do not write |
| #1333 | X coordinate of No.4 Tool | ⛔ do not write |
| #1334 | X coordinate of No.5 Tool | ⛔ do not write |
| #1335 | X coordinate of No.6 Tool | ⛔ do not write |
| #1336 | X coordinate of No.7 Tool | ⛔ do not write |
| #1337 | X coordinate of No.8 Tool | ⛔ do not write |
| #1338 | X coordinate of No.9 Tool | ⛔ do not write |
| #1339 | X coordinate of No.10 Tool | ⛔ do not write |
| #1340 | X coordinate of No.11 Tool | ⛔ do not write |
| #1341 | X coordinate of No.12 Tool | ⛔ do not write |
| #1342 | X coordinate of No.13 Tool | ⛔ do not write |
| #1343 | X coordinate of No.14 Tool | ⛔ do not write |
| #1344 | X coordinate of No.15 Tool | ⛔ do not write |
| #1345 | X coordinate of No.16 Tool | ⛔ do not write |
| #1346 | X coordinate of No.17 Tool | ⛔ do not write |
| #1347 | X coordinate of No.18 Tool | ⛔ do not write |
| #1348 | X coordinate of No.19 Tool | ⛔ do not write |
| #1349 | X coordinate of No.20 Tool | ⛔ do not write |
| #1350 | Y coordinate of No.1 Tool | ⛔ do not write |
| #1351 | Y coordinate of No.2 Tool | ⛔ do not write |
| #1352 | Y coordinate of No.3 Tool | ⛔ do not write |
| #1353 | Y coordinate of No.4 Tool | ⛔ do not write |
| #1354 | Y coordinate of No.5 Tool | ⛔ do not write |
| #1355 | Y coordinate of No.6 Tool | ⛔ do not write |
| #1356 | Y coordinate of No.7 Tool | ⛔ do not write |
| #1357 | Y coordinate of No.8 Tool | ⛔ do not write |
| #1358 | Y coordinate of No.9 Tool | ⛔ do not write |
| #1359 | Y coordinate of No.10 Tool | ⛔ do not write |
| #1360 | Y coordinate of No.11 Tool | ⛔ do not write |
| #1361 | Y coordinate of No.12 Tool | ⛔ do not write |
| #1362 | Y coordinate of No.13 Tool | ⛔ do not write |
| #1363 | Y coordinate of No.14 Tool | ⛔ do not write |
| #1364 | Y coordinate of No.15 Tool | ⛔ do not write |
| #1365 | Y coordinate of No.16 Tool | ⛔ do not write |
| #1366 | Y coordinate of No.17 Tool | ⛔ do not write |
| #1367 | Y coordinate of No.18 Tool | ⛔ do not write |
| #1368 | coordinate of No.19 Tool | ⛔ do not write |
| #1369 | Y coordinate of No.20 Tool | ⛔ do not write |
| #1370 | Z coordinate of No.1 Tool | ⛔ do not write |
| #1371 | Z coordinate of No.2 Tool | ⛔ do not write |
| #1372 | Z coordinate of No.3 Tool | ⛔ do not write |
| #1373 | Z coordinate of No.4 Tool | ⛔ do not write |
| #1374 | Z coordinate of No.5 Tool | ⛔ do not write |
| #1375 | Z coordinate of No.6 Tool | ⛔ do not write |
| #1376 | Z coordinate of No.7 Tool | ⛔ do not write |
| #1377 | Z coordinate of No.8 Tool | ⛔ do not write |
| #1378 | Z coordinate of No.9 Tool | ⛔ do not write |
| #1379 | Z coordinate of No.10 Tool | ⛔ do not write |
| #1380 | Z coordinate of No.11 Tool | ⛔ do not write |
| #1381 | Z coordinate of No.12 Tool | ⛔ do not write |
| #1382 | Z coordinate of No.13 Tool | ⛔ do not write |
| #1383 | Z coordinate of No.14 Tool | ⛔ do not write |
| #1384 | Z coordinate of No.15 Tool | ⛔ do not write |
| #1385 | coordinate of No.16 Tool | ⛔ do not write |
| #1386 | Z coordinate of No.17 Tool | ⛔ do not write |
| #1387 | Z coordinate of No.18 Tool | ⛔ do not write |
| #1388 | Z coordinate of No.19 Tool | ⛔ do not write |
| #1389 | Z coordinate of No.20 Tool | ⛔ do not write |
| #1390 | X offset of No.1 Tool | ⛔ do not write |
| #1391 | X offset of No.2 Tool | ⛔ do not write |
| #1392 | X offset of No.3 Tool | ⛔ do not write |
| #1393 | X offset of No.4 Tool | ⛔ do not write |
| #1394 | X offset of No.5 Tool | ⛔ do not write |
| #1395 | X offset of No.6 Tool | ⛔ do not write |
| #1396 | X offset of No.7 Tool | ⛔ do not write |
| #1397 | X offset of No.8 Tool | ⛔ do not write |
| #1398 | X offset of No.9 Tool | ⛔ do not write |
| #1399 | X offset of No.10 Tool | ⛔ do not write |
| #1400 | X offset of No.11 Tool | ⛔ do not write |
| #1401 | X offset of No.12 Tool | ⛔ do not write |
| #1402 | X offset of No.13 Tool | ⛔ do not write |
| #1403 | X offset of No.14 Tool | ⛔ do not write |
| #1404 | X offset of No.15 Tool | ⛔ do not write |
| #1405 | X offset of No.16 Tool | ⛔ do not write |
| #1406 | X offset of No.17 Tool | ⛔ do not write |
| #1407 | X offset of No.18 Tool | ⛔ do not write |
| #1408 | X offset of No.19 Tool | ⛔ do not write |
| #1409 | X offset of No.20 Tool | ⛔ do not write |
| #1410 | Y offset of No.1 Tool | ⛔ do not write |
| #1411 | Y offset of No.2 Tool | ⛔ do not write |
| #1412 | Y offset of No.3 Tool | ⛔ do not write |
| #1413 | Y offset of No.4 Tool | ⛔ do not write |
| #1414 | Y offset of No.5 Tool | ⛔ do not write |
| #1415 | Y offset of No.6 Tool | ⛔ do not write |
| #1416 | Y offset of No.7 Tool | ⛔ do not write |
| #1417 | Y offset of No.8 Tool | ⛔ do not write |
| #1418 | Y offset of No.9 Tool | ⛔ do not write |
| #1419 | Y offset of No.10 Tool | ⛔ do not write |
| #1420 | Y offset of No.11 Tool | ⛔ do not write |
| #1421 | Yoffset of No.12 Tool | ⛔ do not write |
| #1422 | Y offset of No.13 Tool | ⛔ do not write |
| #1423 | Y offset of No.14 Tool | ⛔ do not write |
| #1424 | Y offset of No.15 Tool | ⛔ do not write |
| #1425 | offset of No.16 Tool | ⛔ do not write |
| #1426 | Y offset of No.17 Tool | ⛔ do not write |
| #1427 | Y offset of No.18 Tool | ⛔ do not write |
| #1428 | Y offset of No.19 Tool | ⛔ do not write |
| #1429 | Y offset of No.20 Tool | ⛔ do not write |
| #1430 | offset of No.1 Tool | ⛔ do not write |
| #1431 | Z offset of No.2 Tool | ⛔ do not write |
| #1432 | Z offset of No.3 Tool | ⛔ do not write |
| #1433 | Z offset of No.4 Tool | ⛔ do not write |
| #1434 | Z offset of No.5 Tool | ⛔ do not write |
| #1435 | Z offset of No.6 Tool | ⛔ do not write |
| #1436 | Z offset of No.7 Tool | ⛔ do not write |
| #1437 | Z offset of No.8 Tool | ⛔ do not write |
| #1438 | Z offset of No.9 Tool | ⛔ do not write |
| #1439 | Z offset of No.10 Tool | ⛔ do not write |
| #1440 | Z offset of No.11 Tool | ⛔ do not write |
| #1441 | Z offset of No.12 Tool | ⛔ do not write |
| #1442 | Z offset of No.13 Tool | ⛔ do not write |
| #1443 | Z offset of No.14 Tool | ⛔ do not write |
| #1444 | Z offset of No.15 Tool | ⛔ do not write |
| #1445 | Z offset of No.16 Tool | ⛔ do not write |
| #1446 | Z offset of No.17 Tool | ⛔ do not write |
| #1447 | Z offset of No.18 Tool | ⛔ do not write |
| #1448 | Z offset of No.19 Tool | ⛔ do not write |
| #1449 | Z offset of No.20 Tool | ⛔ do not write |
| #1450 | T1 file no. | ⛔ do not write |
| #1451 | T2 file no. | ⛔ do not write |
| #1452 | Safety height Z | ⛔ do not write |
| #1453 | Starting point Y | ⛔ do not write |
| #1454 | Starting point Z | ⛔ do not write |
| #1455 | Xpos1 — work coordinate value. | ⛔ do not write |
| #1456 | Xpos2 — work coordinate value. | ⛔ do not write |
| #1457 | Xpos3 — work coordinate value. | ⛔ do not write |
| #1458 | Xpos4 — work coordinate value. | ⛔ do not write |
| #1459 | Xpos5 — work coordinate value. | ⛔ do not write |
| #1460 | Xpos6 — work coordinate value. | ⛔ do not write |
| #1461-#1464 | *(no description in the table)* | ⛔ do not write |
| #1465 | pause between T1 and T2 | ⛔ do not write |
| #1466 | T1 spindle speed | ⛔ do not write |
| #1467 | T2 spindle speed | ⛔ do not write |
| #1468 | NC file index no. | ⛔ do not write |
| #1469 | Custom G-code number — 自定义G代码号 | ⛔ do not write |
| #1470-#1471 | *(no description in the table)* | ⛔ do not write |
| #1472 | This feature was developed at the request of one of the clients. What it does is not clear. — May be -57.544. That is, it seems to work. | ⛔ do not write |
| #1473 | Virtual tool Z offset 1 | ⛔ do not write |
| #1474 | Virtual tool Z offset 2 | ⛔ do not write |
| #1475 | Virtual tool Z offset 3 | ⛔ do not write |
| #1476 | Virtual tool Z offset 4 | ⛔ do not write |
| #1477 | Virtual tool Z offset 5 | ⛔ do not write |
| #1478 | Virtual tool Z offset 6 | ⛔ do not write |
| #1479 | Virtual tool Z offset 7 | ⛔ do not write |
| #1480 | Virtual tool Z offset 8 | ⛔ do not write |
| #1481 | Virtual tool Z offset 9 | ⛔ do not write |
| #1482 | Virtual tool Z offset 10 | ⛔ do not write |
| #1483 | Virtual tool Z offset 11 | ⛔ do not write |
| #1484 | Virtual tool Z offset 12 | ⛔ do not write |
| #1485 | Virtual tool Z offset 13 | ⛔ do not write |
| #1486 | Virtual tool Z offset 14 | ⛔ do not write |
| #1487 | Virtual tool Z offset 15 | ⛔ do not write |
| #1488 | Virtual tool Z offset 16 | ⛔ do not write |
| #1489 | Virtual tool Z offset 17 | ⛔ do not write |
| #1490 | Virtual tool Z offset 18 | ⛔ do not write |
| #1491 | Virtual tool Z offset 19 | ⛔ do not write |
| #1492 | Virtual tool Z offset 20 | ⛔ do not write |
| #1493-#1498 | *(no description in the table)* | ⛔ do not write |
| #1499 | coordinate offset method | ⛔ do not write |
| #2500-#2599 | *(no description in the table)* |  |
| #2600-#2999 | These variables are passed the values of the parameters of the CAM function (1100-1499) — Read more about this in the instructions for the CAM function. Also, the values of these variables cannot be changed "manually". Only through the parameters of the CAM function. |  |

## Not marked in the table

| variables | description | note |
|---|---|---|
| #2039-#2041 | *(no description in the table)* |  |
| #2043-#2071 | *(no description in the table)* | measured |
