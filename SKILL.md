---
name: ddcs-expert
description: Rules for writing G-code and macros for the DDCS Expert / M350 CNC controller (not the DDCS V4.1, not Fanuc Macro B): syntax, jumps, #1505 dialogs, the #variables and Pr parameters (look one up before writing it), Modbus and the network share, hazards, the vendor stock macros; from bench-measured findings.
---

# DDCS Expert M350 — Rulebook

> For the DDCS V4.1, use the `ddcs-v41` skill; for the DDCS V3 / DM500, use the `ddcs-v3-dm500` skill instead: this one is for the DDCS Expert / M350 only.

> Every measured statement names the bench test that measured it (see evidence.md).


## The rules

0. This controller is NOT Fanuc Macro B and NOT the DDCS V4.1: do not carry habits from either (differences.md).
1. Jump with `IF #190<#191 GOTO5` and a label line `N5`.
2. Never write an inline `IF .. THEN` assignment: write a negated jump, the assignment, then the label (syntax.md).
3. Start every line at column 0.
4. A comment is `( text )` with no `(` or `)` inside it; a nested `)` can refuse the whole file.
5. G04 P is in milliseconds: 1.5 s is `G04 P1500`.
6. An OK/Cancel prompt is `#1505=1(Check the clamp)` (dialogs.md).
7. With Pr76 "Macro scan switch" open (the normal setting, and the one jumps need), one bad line rejects the whole file (V13_trig.nc): check every line before sending.
8. ⛔ ANY O block without an M99 (M099, a bare O line, an O0001 header) makes the WHOLE file silently do nothing: READY, no error. End every O block with M99, and drop the O0001 header.
9. ⛔ M98 P<n> with no O<n> anywhere is silently a no-op: check every called number exists.
10. ⛔ Pass every letter a sub reads on EVERY call, or zero its locals at the top: a letter left out silently keeps the value the SAME sub got on its previous call (a different sub starts at 0).
11. ⛔ A work system chosen in a program (G55 ...) stays active after M30: end the program with the G54 you want, or the next job runs in G55.
12. ⛔ A #790 read right after a move gives the position BEFORE the move: read #880 first, which waits for the move, and only then read #790.
13. ⛔ An exponent in a coordinate (G0 X5E-1) is silently wrong: it moved X by 4 mm, not 0.5. Never write exponents.
14. `vendor/` holds the vendor's own macro and dialog files ONLY when the person has put them there (a fresh copy of this repository has none; `vendor/README.md` says which files and where from). If `vendor/macros/<file>` or `vendor/dialogs/<file>` exists, read it FIRST and say which firmware version it came from (`vendor/VERSION.txt` if they wrote one, else ask: it may be newer than the firmware this pack was measured on); if it does not exist, use the lines this pack quotes from it (`vendor/README.md`, `evidence.md`). Neither is a bench test, and other people's community notes are not re-published here: where either disagrees with this pack's measured files, the measured files win (`community/conflicts.md` lists each known disagreement).

## Read next

- `differences.md`: habits from Fanuc Macro B or the other DDCS that are wrong here
- `syntax.md`: the code forms that run, are refused or silently misbehave; the aliases
- `variables.md`: every #variable and parameter: its type, who reads and writes it, what survives a reboot
- `dialogs.md`: operator prompts and number entry
- `hazards.md`: what silently goes wrong or does damage, measured and not measured
- `operation.md`: using the controller day to day: homing, jogging, zeroing, loading and running a file
- `ui.md`: keys, menus and screens
- `integration.md`: reaching the controller from a PC: which doors are open, shares, hook files
- `evidence.md`: every measured fact with the bench test behind it
- `examples/`: archived test programs, word for word, with their results
- `community/`: other people's notes, ranked below the measured files
- `vendor/`: the vendor's own files, when the person has put them there
