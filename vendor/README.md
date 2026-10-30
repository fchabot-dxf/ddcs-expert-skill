# DDCS Expert M350 — vendor/: your controller's own files

> Every measured statement names the bench test that measured it (see evidence.md).

This folder is yours, and git ignores everything in it but this file: this repository never ships the vendor's files (they are the vendor's, referenced here, not re-hosted). Fill it with your own DDCS Expert M350's files and they are read FIRST; with it empty, the lines quoted below are used. Either way they are **not bench tests**: they show what the vendor's code does, not that a user program behaves the same, and where they disagree with this pack's measured files, **the measured files win**.

This pack's bench ran firmware 2025-06-19-00 for the rows before 2026-10-30; 2026-04-10-00 for the 2026-10-30 session; the lines quoted below are from firmware 2026-04-10-00. Yours may be newer: your own copy is what your controller runs.

## How to fill it

1. Copy them off your own controller over the network: its system share (SMB, guest login) is `SYSDISK`, and every file below sits at its root. Your controller may run its own edited copies of some of them: that is what it really runs, so prefer it.
2. Or take them from the vendor's GitHub release 2026-04-10-00, `V1_M350_20260410.zip` (V1 hardware; a `V2_M350_20260410.zip` sits beside it for V2 hardware, not compared here): https://github.com/foinnc/M350/releases/download/2026-04-10-00/V1_M350_20260410.zip, folder `V1_M350_20260410/install/`; newer releases are listed at https://github.com/foinnc/M350/releases; the vendor's documents are its `Docs/` folder (https://github.com/foinnc/M350/tree/9c4dabd20cb465407a1234fdd82b218765b4ac8f/Docs, the commit of 2026-10-23).
3. Put each `.nc` file in `vendor/macros/` and each `.rc` file in `vendor/dialogs/`, names unchanged, then write your controller's firmware version on one line in `vendor/VERSION.txt`, so whoever reads these files can say which version they came from.

How to read them: a key, a menu entry or an M-code runs a macro file; a macro calls a library subprogram with `M98 P<n>` (the O-number below). M<n> runs O<10000 + n> in `slib-m.nc`; O90nn in `slib-g.nc` is the subprogram behind Gnn. The vendor writes its comments in Chinese (re-encoded from the controller's GBK as UTF-8).

## Macros

- `macros/ALL_T.nc`: The tool magazine routine: M101 (slib-m.nc O10101). In the release: `V1_M350_20260410/install/ALL_T.nc`.
- `macros/CAM.nc`: The CAM menu entry: runs G#2038 (the CAM program the menu picked). In the release: `V1_M350_20260410/install/CAM.nc`.
- `macros/M10.nc`: Runs M10. In the release: `V1_M350_20260410/install/M10.nc`.
- `macros/M11.nc`: Runs M11. In the release: `V1_M350_20260410/install/M11.nc`.
- `macros/M19.nc`: Runs M19 (slib-m.nc O10019: spindle stop). In the release: `V1_M350_20260410/install/M19.nc`.
- `macros/M3.nc`: Spindle forward at the speed in #585 (M3S#585). In the release: `V1_M350_20260410/install/M3.nc`.
- `macros/M4.nc`: Spindle reverse at the speed in #585 (M4S#585). In the release: `V1_M350_20260410/install/M4.nc`.
- `macros/M5.nc`: Spindle stop. In the release: `V1_M350_20260410/install/M5.nc`.
- `macros/M8.nc`: Runs M8. In the release: `V1_M350_20260410/install/M8.nc`.
- `macros/M9.nc`: Runs M9. In the release: `V1_M350_20260410/install/M9.nc`.
- `macros/T.nc`: The T key: T#1504 (the tool number entered). In the release: `V1_M350_20260410/install/T.nc`.
- `macros/absA.nc`: Absolute-encoder homing of A: M108 (slib-m.nc O10108). In the release: `V1_M350_20260410/install/absA.nc`.
- `macros/absB.nc`: Absolute-encoder homing of B: M109 (slib-m.nc O10109). In the release: `V1_M350_20260410/install/absB.nc`.
- `macros/absX.nc`: Absolute-encoder homing of X: M105 (slib-m.nc O10105). In the release: `V1_M350_20260410/install/absX.nc`.
- `macros/absY.nc`: Absolute-encoder homing of Y: M106 (slib-m.nc O10106). In the release: `V1_M350_20260410/install/absY.nc`.
- `macros/absZ.nc`: Absolute-encoder homing of Z: M107 (slib-m.nc O10107). In the release: `V1_M350_20260410/install/absZ.nc`.
- `macros/abszero.nc`: Absolute-encoder homing of every axis: M105 to M109. In the release: `V1_M350_20260410/install/abszero.nc`.
- `macros/error.nc`: The error hook: empty here. In the release: `V1_M350_20260410/install/error.nc`.
- `macros/ext_button.nc`: An external button: runs M#1996 (the M-code assigned to it). In the release: `V1_M350_20260410/install/ext_button.nc`.
- `macros/extnc0-1.nc`: External program key 1: M120 (slib-m.nc O10120). In the release: `V1_M350_20260410/install/extnc0-1.nc`.
- `macros/extnc1-1.nc`: External program key 2: M121 (slib-m.nc O10121). In the release: `V1_M350_20260410/install/extnc1-1.nc`.
- `macros/extnc2-1.nc`: External program key 3: M122 (slib-m.nc O10122). In the release: `V1_M350_20260410/install/extnc2-1.nc`.
- `macros/fndA.nc`: Home A: M98 P501 X3 (slib-g.nc O501, the single-axis home search). In the release: `V1_M350_20260410/install/fndA.nc`.
- `macros/fndB.nc`: Home B: M98 P501 X4 (slib-g.nc O501). In the release: `V1_M350_20260410/install/fndB.nc`.
- `macros/fndX.nc`: Home X: M98 P501 X0 (slib-g.nc O501). In the release: `V1_M350_20260410/install/fndX.nc`.
- `macros/fndY.nc`: The vendor's stock Y homing: M98 P501 X1 (slib-g.nc O501). Machines often customise this file; this is the stock text. In the release: `V1_M350_20260410/install/fndY.nc`.
- `macros/fndZ.nc`: Home Z: M98 P501 X2 (slib-g.nc O501). In the release: `V1_M350_20260410/install/fndZ.nc`.
- `macros/fndzero.nc`: The vendor's stock homing macro: Z, X, Y, A, B one at a time (M98 P501 X2, X0, X1, X3, X4). Machines often customise this file; this is the stock text. In the release: `V1_M350_20260410/install/fndzero.nc`.
  - quoted (firmware 2026-04-10-00), cited by `ex-hw-call`: `M98P501X2`
- `macros/gotozero.nc`: Go to work zero: up to Z#569 unless already above it, then X0 Y0, then A0 B0, then Z#569. In the release: `V1_M350_20260410/install/gotozero.nc`.
- `macros/key-1.nc`: The vendor's stock K1 key macro: #1509 and #1948 set to 1 for one second, then back to 0. Machines often customise the key files; this is the stock text. In the release: `V1_M350_20260410/install/key-1.nc`.
- `macros/mulprobe.nc`: Several tools in a row: for each tool number listed from #1970 (at most 20), change to it and set its length on the fixed setter (M98 P502 with #1502 = 2; its own comments). In the release: `V1_M350_20260410/install/mulprobe.nc`.
- `macros/pause.nc`: The pause hook: M119 (slib-m.nc O10119). In the release: `V1_M350_20260410/install/pause.nc`.
- `macros/probe.nc`: Tool setting: M98 P502 (slib-g.nc O502). In the release: `V1_M350_20260410/install/probe.nc`.
- `macros/slib-g.nc`: The G-code library: the subprograms behind G-codes (O90nn runs Gnn) and the shared routines (homing, tool setting, dual Y, the tool change); see the list below. In the release: `V1_M350_20260410/install/slib-g.nc`.
  - quoted (firmware 2026-04-10-00), cited by `ex-tap-g84-vendor`: `O9084`
  - quoted (firmware 2026-04-10-00), cited by `ex-tap-g84-vendor`: `G91G00X#0Y#1A#3B#4C#5`
  - quoted (firmware 2026-04-10-00), cited by `ex-tap-g84-vendor`: `G90G01Z#11`
- `macros/slib-m.nc`: The M-code library: O<10000 + n> runs M<n>; see the list below. In the release: `V1_M350_20260410/install/slib-m.nc`.
  - quoted (firmware 2026-04-10-00), cited by `ex-io-out-vendor`: `O10050`
  - quoted (firmware 2026-04-10-00), cited by `ex-io-out-vendor`: `#1552=1`
  - quoted (firmware 2026-04-10-00), cited by `ex-io-out-vendor`: `O10051`
  - quoted (firmware 2026-04-10-00), cited by `ex-io-out-vendor`: `#1552=0`
  - quoted (firmware 2026-04-10-00), cited by `ex-tap-m180-vendor`: `O10180`
  - quoted (firmware 2026-04-10-00), cited by `ex-tap-m180-vendor`: `#579 = 1`
  - quoted (firmware 2026-04-10-00), cited by `ex-tap-m180-vendor`: `O10181`
  - quoted (firmware 2026-04-10-00), cited by `ex-tap-m180-vendor`: `#579 = 0`
  - quoted (firmware 2026-04-10-00), cited by `ex-io-wait-vendor`: `O10300`
  - quoted (firmware 2026-04-10-00), cited by `ex-io-wait-vendor`: `WHILE [#[1520+#4-1] != #6] DO1`
  - quoted (firmware 2026-04-10-00), cited by `ex-hw-semicolon`: `IF #1301==0 GOTO4;`
  - quoted (firmware 2026-04-10-00), cited by `ex-hmi-1503-vendor`: `#1503 = 1(`
- `macros/slibuser.nc`: The user library: the vendor's example O9199 (G199). In the release: `V1_M350_20260410/install/slibuser.nc`.
- `macros/sysstart.nc`: The vendor's stock startup macro: M115 (slib-m.nc O10115). Machines often customise this file; this is the stock text. In the release: `V1_M350_20260410/install/sysstart.nc`.
- `macros/testf2.nc`: A test program: each axis 10 mm out and back, relative, at F200. In the release: `V1_M350_20260410/install/testf2.nc`.
- `macros/testf4.nc`: An output test: sets #1930 + n one at a time with a delay (its own comment: LED delay). In the release: `V1_M350_20260410/install/testf4.nc`.
- `macros/testf5.nc`: An output test: the spindle at S12000, then the outputs #1552 + n one at a time with a delay (its own comments). In the release: `V1_M350_20260410/install/testf5.nc`.

## Library subprograms (by O-number)

`macros/slib-g.nc`

- O9053: behind G53: a rapid to a machine position (its own comment)
- O9081: behind G81 (drilling)
- O9082: behind G82
- O9073: behind G73
- O9083: behind G83
- O9074: behind G74 (its own comment: left-hand tapping)
- O9084: behind G84 (its own comment: right-hand tapping)
- O9028: behind G28
- O9029: behind G29
- O500: not described yet (it branches on #578, the active coordinate system)
- O501: the single-axis home search, X = the axis (its own comment); fnd*.nc call it
- O502: tool setting (its own comment); probe.nc and mulprobe.nc call it
- O503: dual Y (its own comment)
- O20000: the tool change program (its own comment)

`macros/slib-m.nc`

- O10000: M0: a wait (G04 P-1)
- O10001: M1: the optional stop
- O10002: M2, empty here
- O10006: M6, empty here
- O10019: M19: spindle stop
- O10030: M30: the program end (M5 M9 M11)
- O10047: M47: counts the loop (#701, #702)
- O10050-O10091: M50-M91: the outputs on and off, one pair per output (#1552-#1572 = 1 / 0)
- O10100: M100: not described yet
- O10101: M101: the tool magazine (it exits when none is configured, #1302 = 0)
- O10102-O10104: M102-M104: the vacuum pump and the dust collector (their own comments)
- O10105-O10114: M105-M114: absolute-encoder homing, one per axis (their own comments: a 24-bit encoder, the pitch)
- O10115: M115: opens the Z-axis brake (its own comment); sysstart.nc runs it
- O10116-O10117: M116 / M117: #1994 = 1 / 0
- O10118: M118: waits for an input (its own comments: the input port, the active level)
- O10119: M119: the pause hook (its own comment: machining time paused)
- O10120-O10122: M120-M122: the external program keys
- O10129: M129: triggers a read (#2094 = 1, its own comment)
- O10130-O10133: M130-M133: not described yet
- O10136: M136: sets the current tool's Z offset from the machine Z (#[1430 + #1300 - 1] = #882, its own comment)
- O10140-O10141: not described yet
- O10150-O10181: M150-M181: an output through its configured port, on or off (their own comments: output port, port enable)
- O10250-O10281: M250-M281: the panel key handlers (#1997-#2028; their own comments)
- O10300-O10307: M300-M307: read a configured input (their own comments: input port, input enable)

`macros/slibuser.nc`

- O9199: G199: a G90 G01 move to X#6 Y#7 Z#8 at F#15

