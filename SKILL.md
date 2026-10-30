---
name: ddcs-expert
description: Rules for writing G-code and macros for the DDCS Expert / M350 CNC controller (not the DDCS V4.1, not Fanuc Macro B): syntax, jumps, #1505 dialogs, the #variables and Pr parameters (look one up before writing it), Modbus and the network share, hazards, the vendor stock macros; from bench-measured findings.
---

# DDCS Expert M350 — rules for an AI writing code

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
8. `vendor/` holds the vendor's own macro and dialog files ONLY when the person has put them there (a fresh copy of this repository has none; `vendor/README.md` says which files and where from). If `vendor/macros/<file>` or `vendor/dialogs/<file>` exists, read it FIRST and say which firmware version it came from (`vendor/VERSION.txt` if they wrote one, else ask: it may be newer than the firmware this pack was measured on); if it does not exist, use the lines this pack quotes from it (`vendor/README.md`, `evidence.md`). Neither is a bench test, and other people's community notes are not re-published here: where either disagrees with this pack's measured files, the measured files win (`community/conflicts.md` lists each known disagreement).

## Read next

- `differences.md`
- `syntax.md`
- `variables.md`
- `dialogs.md`
- `hazards.md`
- `integration.md`
- `evidence.md`
- `examples/`
- `community/`
- `vendor/`
