# DDCS Expert M350 — integration

> Every measured statement names the bench test that measured it (see evidence.md).


## The doors this controller has

| door | | evidence |
|---|---|---|
| Network shares (CNCDISK + SYSDISK, SMB guest) | ✅ yes | SMB round-trip |
| Modbus (serial port 2, slave) | ✅ yes | panel photo + live adapter test |
| G-code injection over Modbus | ✅ yes | #1505 message shown on the pendant; a bench script A/B |
| Reading inputs / setting outputs from a program | ❔ unmeasured | the vendor's own slib-m.nc reads #[1520+N-1] and sets #1552 (attested: vendor macro, not a bench test); no bench run has read an input or switched an output from a program |
| Pressing a panel key from a macro | ✅ yes | A7b_BUTTON_ONEWAY.nc, photographed pendant |
| USB keyboard (jog pad, soft keys, dialogs, Print Screen; Start / Pause / Reset only by assigned K keys) | ✅ yes | USB HID dongle key sweep, jogs read on the DRO (DONGLE-TESTS.md) |

The keys, the pages and the dialogs as the operator sees them: `ui.md`.

## Menu and hook files

- The assignable K8-K16 USB keys do nothing until a parameter gives them a function code (the same code table #2037 uses), and an assignment works at once, no reboot: Pr252 = 1328 made numpad Enter Start, Pr234 = 1329 the Pause key Pause, Pr233 = 1327 Scroll Lock Reset, Pr232 = 1363 F12 Break run (each written as an injected #(500 + N) line, e.g. #752 = 1328, from 0, and read back: 4 of 4 took; an injected parameter line is not always taken, so read every one back: ex-modbus-fc16-param). A Pause from such a key resumes on Start, unlike an M0 stop. *(K-key assignment by Modbus injection (read back), keys sent by a USB HID dongle with V23 loaded)*
- System macros that ship in the firmware payload (sysstart.nc, fndzero.nc, slib-g.nc ...) are RESTORED at boot after an edit over the share; new files survive. *(power-cycle test)*

Not measured:

- Community report (one user, V2.1 hardware): the K-key bound to function 1399 (the CAM page) opened the CAM page AND moved X 10 mm per press, and survived a factory reset. Not reproduced on a V1 and cause unknown. On some V2.1 controllers the CAM key also moves X: bind it and press it once with the X axis clear before relying on it. *(controllers/expert-m350/FINDINGS.md (community report, 1399))*

The vendor's own hook files, each a vendor file, not a bench test (`vendor/README.md`):

- `sysstart.nc`: The vendor's stock startup macro: M115 (slib-m.nc O10115). Machines often customise this file; this is the stock text.
- `T.nc`: The T key: T#1504 (the tool number entered).
- `ALL_T.nc`: The tool magazine routine: M101 (slib-m.nc O10101).
- `probe.nc`: Tool setting: M98 P502 (slib-g.nc O502).
- `mulprobe.nc`: Several tools in a row: for each tool number listed from #1970 (at most 20), change to it and set its length on the fixed setter (M98 P502 with #1502 = 2; its own comments).
- `pause.nc`: The pause hook: M119 (slib-m.nc O10119).
- `error.nc`: The error hook: empty here.
- `CAM.nc`: The CAM menu entry: runs G#2038 (the CAM program the menu picked).
- `key-1.nc`: The vendor's stock K1 key macro: #1509 and #1948 set to 1 for one second, then back to 0. Machines often customise the key files; this is the stock text.
- `ext_button.nc`: An external button: runs M#1996 (the M-code assigned to it).
- `extnc0-1.nc`: External program key 1: M120 (slib-m.nc O10120).
- `extnc1-1.nc`: External program key 2: M121 (slib-m.nc O10121).
- `extnc2-1.nc`: External program key 3: M122 (slib-m.nc O10122).

## Network shares and files

- Two SMB shares (guest, writable): CNCDISK = /local/ and SYSDISK; read and write confirmed end to end. *(SMB round-trip)*
- A syntax error's text and line are written to no file on the shares: they exist only on the screen. *(sha256 diff of 193 files)*

## Variables and their files

- #578 is the active work system number (1 = G54 .. 6 = G59) and is writable, but it does NOT follow a G54..G59 command in the program; emit G54..G59 to select. *(V4_active_wcs.nc, DIAG_g53setup.nc)*
- G54..G59 offsets are #805..#834, 5 per system (#[800 + #578*5 + axis]); panel-verified on all six. *(panel read of all six systems)*
- H01..H16 tool-length offsets are #900..#915 and writable. *(V6_set.nc, V6_restore.nc)*
- Macro #N from #500 to #1499 is the Param page parameter Pr(N - 500): the same index as the setting file and the Modbus register 6500 + 2 x (N - 500). #805 is Pr305 (G54 X, register 7110) and #900 is Pr400 (H01 tool length, register 7300). A write there changes a machine setting. *(Modbus reads of the whole parameter table against the setting file (FINDINGS section 28))*
- The work share's uservar file holds 450 doubles, slot = #var - 100 (#100..#549 by that rule). Whether a macro's #500-#549 are these slots or the parameters Pr0-Pr49 (ex-var-param-address) is not settled: never write #500-#549. *(operator test writes decoded over SMB)*
- While a program runs, uservar and RAM are isolated both ways: a PC write made mid-loop never reached the program, and the program's own write was not in the file after its M30. A PC write BETWEEN runs, before a Start, is not measured on the Expert (un-ex-uservar-write). *(A9-a)*
- #790, #791 and #792 read inside a macro are the work X, Y and Z in the active work system: a macro printed them and they matched the pendant's Abs column to the thousandth on all three. *(V14_wcs_pos.nc (popups photographed))*

## Modbus

- The globals #0..#499 have no Modbus register: #100 = 12345 was not found anywhere in registers 0..20000 (65 blocks answered), while #1060 = #100 read it back. To see a global from the PC, copy it into a parameter (and put the parameter back), or read it with the injection reader (#915 = #N, register 7330). *(register scan for #100 = 12345, read back through #1060 (FINDINGS section 31))*
- Modbus runs on serial port 2 at 115200 8N1; the Modbus mode parameter (#279) must be set and the controller REBOOTED before it transacts. *(panel photo + live adapter test)*
- As a Modbus slave: register 7080 = work X, 7260 = machine X; 10002 is the run state (0 READY, 1 not idle, 2 RESET: not a running flag, see ex-modbus-10002); floats are word-swapped (low word first). *(FC03 read, V21_dwell.nc, V24_1503_persist.nc, V17a_badvar.nc)*
- Any parameter reads at register 6500 + 2 x (setting index): 21 of 21 exact. *(21-parameter read)*
- G-code injection: FC16 to register 3000, one ASCII line of at most 246 bytes, low byte first in each register, NO trailing newline; the controller runs it. *(#1505 message shown on the pendant; a bench script A/B)*
- Universal variable reader: inject #915 = #<target>, read register 7330, then restore #915 = 0. *(six-variable test)*
- A Modbus key press (#2037) does nothing while a program is held on a #1505 dialog (injected Enter twice and Reset once: the dialog stayed up, the line stayed) and nothing in ERROR with the E-stop in (twice). A USB keyboard still answers the dialog and still reaches the screen in ERROR. A program started over Modbus must therefore not wait on a #1505 dialog. (That held program later ended on its own with no key at the pendant; why is not known.) *(V24_1503_persist.nc held on its #1505=1 dialog; #2037 = 65914 injected with the E-stop in (2 trials))*
- Injected key presses can drop: of two injected Up presses one registered, and the first injected Start did nothing while the second ran the program. Check every injected press on the screen or a register; never count them. *(Modbus key injection, V24 session (photographed))*
- Register 10002 is not a running flag: 0 = READY (also while paused by the Pause key, and in a syntax-error ERROR at analysis), 1 = not idle (running, and one ERROR read 1 throughout), 2 = RESET (until a second Reset). The status cell on the screen is the only sure sign of ERROR. *(V24 (the G04 P15.0 run), V17a_badvar.nc, the K-key Reset and Pause session)*
- Register 16062 is the executing line, live: three 8 s dwells on lines 8, 11 and 14 read 8 -> 11 -> 14, then 0 after M30. It counts the file lines as the program pane numbers them, comments included, so 16062 divided by the line count is a progress bar. About 20 % of its reads come back empty: keep the last good value and retry, never read a miss as 0. *(V23_lineadvance.nc (polled every 0.5 s), V24_1503_persist.nc)*
- The position registers are float32, word-swapped, at even offsets X Y Z A B: work 7080 / 7082 / 7084 / 7086 (= #790-#793), machine 7260 / 7262 / 7264 / 7266 (= #880-#883). Z is at +4 (7084, 7264); +2 is Y. Checked to the thousandth against the pendant and the G54 table (work = machine - G54, and for Z minus #1430 as well); they track live during motion. *(read-only FC03 against the pendant photo and the WCS table; the V16, V28 and V29 motion traces)*
- A read sometimes gets no answer (about 3 in 40 requests at a 60 ms cadence, at any address in 7000-7400), and a reply can arrive behind 1-4 stray leading bytes: find the 01 03 header in what arrives, read the bytes still owed, check the CRC, and retry a failed read. Never mark a register dead or reuse a stale value as fresh. *(FC03 sweeps of 7000-7400 (idle, read-only))*
- A parameter, hidden ones included, is written reliably by FC16 straight to its own register 6500 + 2 x N (float32, low word first): three of three writes read back within 3 s. Pr N is also macro variable #(500 + N), and injected G-code lines of that form are not always taken: in the same session #761 = 30 and #762 = 30 took, #763 = 30 took on the second try, then four injected #762 lines putting it back (= 0, = 0, = 0.0, = 1) were ACKed and left Pr262 at 30; why is not known (the K-key lines of ex-kbd-k-keys took 4 of 4). Read back every injected parameter write, or use FC16. *(direct FC16 writes to 7024 and 7026 with read-backs; injected #761 / #762 / #763 lines)*
- A direct FC16 parameter write changes RAM only; it does not survive a power-off, and the pendant edit does: Pr132 "Initial speed of Probing" written 800 -> 300 by FC16 to 6764 read back 300 at once; after a power-off Modbus read 800 again. The pendant's own edit (400 on the Param page, Probe section) is what SYSDISK/setting holds (setting[132] = 400). Macro writes (#2503, the tool offset #1430 written by a macro) DID survive the same power-off. To change a parameter for good: the pendant, or a macro line; never FC16 alone. *(Pr132 written by FC16 (2026-10-28), read back after the owner's power-off, then the pendant edit and SYSDISK/setting decoded over SMB)*

## Inputs and outputs

- An input's active level 0 = "N" (active-low), 1 = "P" (active-high). *(live differential toggle (#577))*

Wait for input IN5 to go high (IN5 = #1524 = #[1520+5-1]: the literal and the vendor's computed form are the same variable):
```gcode
WHILE [#1524 != 1] DO1   ( wait IN5 = 1 )
G04 P10
END1
```
*(vendor macro: slib-m.nc O10300 (sensor wait); not a bench test)*

- The vendor's M50 sets output 1 with #1552 = 1 and M51 clears it with #1552 = 0. *(vendor macro: slib-m.nc O10050 (M50), O10051 (M51); not a bench test)*
- G84 (tapping) is the vendor subprogram O9084: it repeats K times a rapid to the hole (G91 G00 X Y: the X and Y words may be INCREMENTAL, as in the vendor's G81), a rapid to R, a G01 to the bottom and a G01 back to R (the servo spindle turns forward, then reverses), then a rapid to the retract level. The spindle must be a step/direction servo on an axis (Spindle interface type, Spindle mapping axis). *(vendor macro: slib-g.nc O9084 (G84); not a bench test)*
- The vendor's M180 switches the spindle to servo mode (#579 = 1, and drives the output set in #1295-#1297) and M181 switches it back to the analog spindle (#579 = 0, the output inverted). G84 tapping runs between them. *(vendor macro: slib-m.nc O10180 (M180), O10181 (M181); not a bench test)*
- The vendor's sensor wait reads the input indirectly: WHILE [#[1520+#4-1] != #6] DO1, a G04 P10 poll, END1 (the input number in #4, the level in #6). *(vendor macro: slib-m.nc O10300 (sensor wait); not a bench test)*

## Examples

Tested programs, verbatim: see `examples/README.md`.

Hazards that apply to any integration: `hazards.md`.
