# DDCS Expert M350 — hazards

> Every measured statement names the bench test that measured it (see evidence.md).



## Measured on this controller

- One bad line rejects the whole file. *(V13_trig.nc)*
- The whole-file refusal is Pr76 "Macro scan switch": with it OPEN (the normal setting) a bad line rejects the whole file before line 1; with it CLOSED the same file runs and stops at the bad line (all ten writes before it had landed), and a forward GOTO fails ("[N]was not found"), because the scan is also what builds the label table. Keep Pr76 open for any program with jumps or subprograms. *(the 2026-10-30 session on the machine, Pr76 open and closed (BAD_WORD, STOP_WRITES, CMP_WORDS))*
- G04 P is always MILLISECONDS: G04 P3000 is about 3 s, and a decimal G04 P3.0 is 3 ms (instant), never 3 seconds. *(DWELL_units.nc)*
- G10 L20 P6 X25 writes no offset and MOVES the X axis to X25 in the active work system: reproduced four months apart (in the second run work X ran 204.174 -> 25.000 while G59 X stayed 42.650). Never emit G10 L20/L2 with axis words; set a work offset by writing its register. *(V1_G10_WCS.nc, V29_g10_l20_motorless.nc)*
- The H word needs two digits: G43 H1 is silently ignored, G43 H01 works; G49 cancels; a bare H01 without G43 does not bind. *(V18 bench session, V18f, V18g)*
- The tool table's Z is always applied, so a program that also uses G43 H<n> with a filled H table applies the tool length twice. *(V18a-V18e)*
- System macros that ship in the firmware payload (sysstart.nc, fndzero.nc, slib-g.nc ...) are RESTORED at boot after an edit over the share; new files survive. *(power-cycle test)*
- A running or paused macro's writes are not visible over Modbus until it completes: mid-macro reads return stale values or 0. *(CALIBRATE.nc)*
- A syntax error LATCHES the controller: every later injection is ACKed but silently dropped until Reset at the pendant; an ACK proves delivery, not execution. *(photographed pendant error, 10-injection test)*
- MGETDATA wedged the analyzer and needed a reboot on firmware 2025-06-19-00; do not use it there. *(A9-b)*
- Reading #1630 (analyze-channel status) inside a running program wedges the analyzer until a reboot. *(PUSH_RUNSTATE.nc)*
- ⛔ M0 / M00 cannot be stepped past by Start on the Expert: after it the status reads READY (register 10002 = 0), and each later Start ran 1-2 s and stopped at the same M00 again. Inside a WHILE loop that is a program that never finishes (seven resumes, each stopped again on the same line; only Reset ended it). Never put M0 / M00 in a program that has to run on: use a #1505=3 dialog as the confirm. *(V25_1505_binary.nc (M0 version), V30_m00_in_loop.nc, V30b_m00_in_loop_counted.nc)*
- ⛔ G49 is not a safety net: the tool-setter offset #1430 stays in the work Z with H00 G49 active (work Z = machine Z - G54 Z - #1430, checked to the thousandth twice). Code that reasons about Z must count #1430 whatever the modal state. *(pendant arithmetic with H00 G49 (2026-09-22), FC03 position read against the pendant (2026-10-16))*
- ⛔ On the Expert the numpad is a jog pad: 8 / 2 / 6 / 4 / 9 / 1 move Y+ / Y- / X+ / X- / Z+ / Z-, and 0 / . move the 5th axis - / +. A stray numpad digit from a keyboard or a script moves an axis. *(USB HID dongle key sweep, jogs read on the DRO (DONGLE-TESTS.md))*
- Where a Start lands depends on the page: a Start sent while the screen shows the System Log is ignored, and a Start on the Program page runs the file under the cursor (it started a BMP screenshot file, which went to ERROR). Send Start from the Monitor page, and look at the screen first. *(V30 session (keyboard Starts, the System Log and captures))*
- ⛔ uservar is not a way into a RUNNING program on the Expert: a PC write into uservar mid-loop (#150 = 88) never reached the macro, which kept the old value. Give a running program nothing through uservar. *(A9-a)*
- ⛔ ! is not NOT: #v = !0 gave 0 with no error, and the program went on with that value. *(bench scripts (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4)*
- ⛔ Decimal equality fails silently: IF [0.3 - 0.2] == 0.1 is false (float32). Compare ROUND[[a] * 1000] == ROUND[[b] * 1000]. *(bench scripts (alias matrix, Pr76 open), FINDINGS 2026-10-30 §4)*
- ⛔ ANY O block without an M99 (M099, a bare O line, an O0001 header) makes the WHOLE file silently do nothing: READY, no error. End every O block with M99, and drop the O0001 header. *(the 2026-10-30 session on the machine, §4b (FLOW_M099, O_IN_FLOW))*
- ⛔ An O number the vendor library also uses (O10000+, O9xxx, O500-O503, O20000) silently calls the VENDOR's block, not yours: number your subs outside them. *(the 2026-10-30 session on the machine, §4b (SHADOW))*
- ⛔ M98 P<n> with no O<n> anywhere is silently a no-op: check every called number exists. *(the 2026-10-30 session on the machine, §4b (P_COMBINED))*
- ⛔ M98 P<n> L0 still runs the subprogram once: to skip a call, jump over it. *(the 2026-10-30 session on the machine, §4b-§4d (FINDINGS b51ec1443))*
- ⛔ M99 outside a subprogram, and M99 P<n>, are refused: end the main program with M30 and return from a sub with a plain M99. *(the 2026-10-30 session on the machine, §4b-§4d (FINDINGS b51ec1443))*
- ⛔ Never read a local your M98 call did not pass: it holds garbage, or a value left over from an earlier call. *(the 2026-10-30 session on the machine, §4b (DEC_ARGS, ARG_LETTERS))*
- Never replace the LOADED program while it runs: the controller reads its file from disk by position, so after a swap it went on reading the NEW file from the same offset, mid-line (syntax error!:L186[= 1058] and the like, 4 of 4, files of 2.6 KB to 573 KB, by a rename-over and by a network send alike). In a cutting job a fragment could be valid and wrong. Replace a program only when the controller is idle. *(the 2026-10-30 session on the machine, §6 (rename A, gateway A / B / X))*
- ⛔ An exponent in a coordinate (G0 X5E-1) is silently wrong: it moved X by 4 mm, not 0.5. Never write exponents. *(the 2026-10-30 session on the machine, motion (a Modbus position trace))*

## Not measured, but a machine-safety rule

- Whether a PC write to uservar BETWEEN runs (before a Start) reaches the next program is not measured on the Expert (A9-a measured a write during a run: it never arrived). On the V4.1 it does not: only a reboot loads the file. Do not write uservar from a PC; give a program its values in the program file. *(controllers/expert-m350/FINDINGS.md (A9-a, a mid-loop write only))*
- #500-#549: the uservar file has slots for them (450 doubles), but a macro's #N from #500 up reads the parameter Pr(N - 500) (#520 = Pr20, the 4th-axis pulse level). Which one #500-#549 are is not settled. Never write #500-#549, from a program or a PC. *(controllers/expert-m350/FINDINGS.md ([TO TEST] #500-#549: a uservar slot, or a setting; BACKLOG t3989))*
- Not measured as a hazard, but it follows from two measured rows: the Modbus variable reader writes the target into #915 (ex-modbus-reader), which is H16's tool length (ex-var-tool-h), and its last step writes 0, not the old value. A G43 H16 between those writes applies the target as a tool length, and an H16 that held a length loses it. Read #915 first and write that value back; never run the reader while a program can apply H16. *(ex-modbus-reader and ex-var-tool-h (both measured); the combination is not)*
- G81-G83 run the vendor subprogram (O9081 and its siblings in slib-g.nc), and the vendor O9081 begins G91 G00 X#0 Y#1: its X and Y words may be INCREMENTAL, not absolute as in a standard G81. Write a drill as plain moves instead. Do not hand-write one without a test move first *(controllers/expert-m350/FINDINGS.md (G81-G83 row, [TO TEST]); the captured SYSDISK slib-g.nc)*
- Community report (one user, V2.1 hardware): the K-key bound to function 1399 (the CAM page) opened the CAM page AND moved X 10 mm per press, and survived a factory reset. Not reproduced on a V1 and cause unknown. On some V2.1 controllers the CAM key also moves X: bind it and press it once with the X axis clear before relying on it. *(controllers/expert-m350/FINDINGS.md (community report, 1399))*
