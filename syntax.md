# DDCS Expert M350 — syntax

> Every measured statement names the bench test that measured it (see evidence.md).


## Program structure

End a program with: `M30`

Write every line from column 0 (no indentation): a label with a leading space is a syntax error on the DDCS family.

Distance mode: `G90` absolute, `G91` incremental.

Dwell 1.5 s: `G04 P1500` (P is in milliseconds).

Move in machine coordinates (Z to the value in #190):
```gcode
G53 Z#190
```

## Jumps and loops

Conditional jump:
```gcode
IF #190<#191 GOTO5
GOTO6
N5
N6
```

The spaced form:
```gcode
IF #190 <= 0 GOTO3
```

Loop (three passes):
```gcode
#190=0
WHILE [#190 < 3] DO1
#190=[#190+1]
END1
```

A clamp ("if #190 > 3 then #190 = 3") — never an inline IF .. THEN on this controller:
```gcode
IF #190 <= 3 GOTO4
#190=3
N4
```

Expression functions (declared; the measured ones are in the Math section): FIX: bench; FUP: unattested.

## Math

- ROUND rounds a half away from zero (ROUND[-2.5] = -3); FUP is a true ceiling (FUP[2] = 2, FUP[2.1] = 3); FIX truncates toward zero (FIX[-2.5] = -2). *(firmware read: FIRMWARE-RE the parse.out read; not a bench test)*
- Precedence is standard (comparisons < + - < * / < unary minus; 2+3*4 = 14). SQRT of a negative and x/0 raise error 10; ^ raises error 21; there is no MOD, power, exponent, AND / OR / XOR; functions are upper-case only. *(firmware read: FIRMWARE-RE the parse.out read; not a bench test)*

Community formulas (translated, credited in the note): `community/sources/references/advanced-macro-mathematics.md` *(community note: references/advanced-macro-mathematics.md; not a bench test)*. Its MOD and ATAN[y]/[x] lines are marked DISPUTED in the note: both are syntax errors here (see the measured list below).

## Comments

- `( text )` — one comment, never nested: a `(` or `)` inside the text ends or breaks it. Keep both out of comment text.
- ( ) comments cannot nest: (text (inner) more) closes at the first ")" and the rest causes a syntax error. *(MGETDATA_TEST.nc)*

## Aliases and alternative forms

Each form an assistant may reach for, and what this controller does with it: works, refused at load (nothing runs), or silently wrong (it runs and computes something else: also in hazards.md). "not measured" means no bench test has tried it here: do not assume it works.

| | form | on this controller | bench test |
|---|---|---|---|
| Comparison | == (equal) | not measured |  |
| Comparison | EQ (equal, the word) | **works** | V10_operators.nc, Modbus injection test |
| Comparison | != (not equal) | not measured |  |
| Comparison | NE (not equal, the word) | **works** | V10_operators.nc, Modbus injection test |
| Comparison | <> (not equal) | not measured |  |
| Comparison | < > <= >= | not measured |  |
| Comparison | LT GT LE GE (the words) | **works** | V10_operators.nc, Modbus injection test |
| Logic | AND / OR / XOR | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Logic | && / \|\| | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Logic | NOT[x] | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Logic | !x (logical not) | **silently wrong** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Math | MOD[a, b] | **refused at load** | photographed pendant errors |
| Math | a MOD b (as an operator) | not measured |  |
| Math | % (remainder) | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Math | ^ (power) | **refused at load** | photographed pendant errors |
| Math | POW[a, b] | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Math | ** (power) | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Math | SQRT[x] | **works** | Modbus injection test (#915 readback), a bench script re-run 20/20 |
| Math | SQR[x] | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Math | TAN[x] | **works** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Math | ATAN[x] (one argument) | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Math | ATAN[y, x] | **works** | V13_trig.nc, V13f_atan_comma.nc |
| Math | ATAN[y]/[x] | **refused at load** | V13_trig.nc, V13f_atan_comma.nc |
| Words | G00 for G0 | not measured |  |
| Words | G4 for G04 | **works** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Words | M03 for M3 | not measured |  |
| Words | lowercase words (g0 x10) | not measured |  |
| Words | a space inside a word (G 0, X 10) | not measured |  |
| Words | a space after # (# 100 = 5) | **works** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Words | decimal forms (.5, 5., 5.0) | **works** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Words | leading zeros (X005) | **works** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Words | a leading + (X+5, +.5) | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Words | an exponent (1e2) | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Words | a decimal comma (0,5) | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |
| Words | H1 for H01 | **silently wrong** | V18 bench session, V18f, V18g |
| Words | G04 P3.0 (a decimal dwell) | **silently wrong** | DWELL_units.nc |
| Jumps | GOTO 5 (a space before the label) | **works** | V11_gotospace.nc |
| Jumps | GO TO (two words) | not measured |  |
| Jumps | a leading zero in a label (N010, GOTO010) | not measured |  |
| Jumps | M099 for M99 | not measured |  |
| Jumps | IF #a <= b GOTOn (spaces between the parts) | not measured |  |
| Jumps | IF … THEN … (inline) | **refused at load** | t2070 |
| Jumps | an indented N label | **refused at load** | t2070 (controlled A/B) |
| Comments | ; comment | not measured |  |
| Comments | ( a ( b ) c ) (a nested comment) | **refused at load** | MGETDATA_TEST.nc |
| Comments | / (block delete) | **refused at load** | a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4 |

## Measured on this controller

- N labels must start at column 0: a leading space before a label is a syntax error (indented statements are fine). *(t2070 (controlled A/B))*
- An inline IF <cond> THEN <assignment> is rejected; the Expert does IF <cond> GOTO <label> only. *(t2070)*
- Two-operand ATAN must be the comma form ATAN[y, x] (ATAN[1, 2] = 26.565, degrees); the slash form ATAN[y]/[x] and one-operand ATAN[x] are syntax errors. *(V13_trig.nc, V13f_atan_comma.nc)*
- The comparison word operators EQ NE LT GT LE GE all work (IF #100 EQ 5 GOTO1 branched). *(V10_operators.nc, Modbus injection test)*
- IF #100==5 GOTO 1 (a space before the label) is accepted. *(V11_gotospace.nc)*
- WHILE [cond] DOn ... ENDn loops parse; an N label inside the loop must still start at column 0. *(t2070 (controlled A/B))*
- Negative-decimal comparisons (IF #100>-4.99) evaluate correctly. *(IF_neg_test.nc)*
- SQRT, COS, SIN, TAN, ABS, ROUND and FIX work; trig arguments are in DEGREES; ROUND rounds, FIX truncates (FIX[2.9] = 2). *(Modbus injection test (#915 readback), a bench script re-run 20/20)*
- MOD is a syntax error; there is no power operator (^ and ** are syntax errors). *(photographed pendant errors)*
- G53 Z#var (no G0) and G53 G0 Z#var are both accepted. *(V3a, V3b)*
- The confirm between passes of a loop is a #1505=3 dialog, never M00: end the WHILE body with #1505 = 3(message) then IF #1505==0 GOTO9, with N9 after END1. The dialog blocks by itself (no Start needed), Enter runs the next pass, Esc jumps out of the loop cleanly to N9, and %.0f from #1510 is filled in inside the dialog text. *(V31_confirm_dialog_in_loop.nc (2 runs, every popup captured))*
- A label defined twice resolves to its FIRST definition in the file, wherever the GOTO sits: a forward jump and a backward jump (GOTO2 placed between the two N2) both landed on the first N2, with no refusal and no alarm. Do not rely on it: keep every label unique. *(V26_goto_dup_label.nc, V27_goto_between.nc (popups photographed))*
- Helical arcs work: a relative counter-clockwise full circle of radius 5 (centre I-5, J0, feed 300) that also moves Z down 1 mm lowered Z while it arced (two mid-move points on the radius-5 circle, Z already 0.72 and 0.87 mm down); not rejected, no alarm. *(V16_helical_arc.nc (live Modbus position trace))*
- On a move that mixes a linear and a rotary axis, F is deg/min on the rotary and the linear axis rides along to arrive with it: G91 G1 X10 B90 F100 took about 54 s, B at 98.6 deg/min and X at 10.9 mm/min. The rotary rapid measured about 1000 deg/min here (G0 B360, a steady 16.6 deg/s), and a combined G0 X B rapid is paced by the rotary rapid too. *(V28_rotary_feed.nc (work X and B polled, 226 samples))*
- Each of these rejects the whole file at analysis, so nothing runs: a malformed variable (#90 = #91k8: syntax error!), trailing junk after an expression (#90 = [1 + 2 k 8]: syntax error!), an unknown value on a word (G04 Pwidht: Unrecognized characters, then syntax error!). The message names the line (L<n>[text]); a keyboard Reset clears the ERROR. *(V17a_badvar.nc, V17b_trailing.nc, V17c_badword.nc (System Log captured))*
- An unclosed [ rejects the whole file at analysis (nothing ran), and the error names the NEXT non-blank line, not the one holding the [: #90 = [1 + 2 on line 26, with line 27 blank, was reported as L28, the comment after it. Look one statement back from the reported line. *(V17d_unclosed.nc (captured))*
- These forms are a syntax error (1 run each, the error photographed): SQR[16], ATAN[1] with one argument, POW[2, 3], 2 ** 3, 7 % 3, NOT[0], 1 && 1, 1 || 0, 1 AND 1, 1 OR 0, 1 XOR 0, a leading + on a number (+5, +.5), an exponent (1e2, 1E3), a decimal comma (0,5) and a / block delete line (syntax error!:L21[/#350 = 1]). With Pr76 (Macro scan switch) open, the default, a syntax error rejects the whole file before line 1; measured with Pr76 closed, where the same forms stop the run mid-way at that line. *(a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4)*
- ⚠ !0 is not a logical NOT: #v = !0 ran and gave 0, with no error (1 run). *(a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4)*
- These forms work, each read back with the value meant (1 run each): TAN[45] (= 1, degrees); the decimal forms 5, 5., 5.0, 0.5, .5, -.5 and -5; leading zeros (005 = 5); G4 P1 and G04 P1; a space after the # (# 3xx = 5). *(a bench script + a bench script (alias matrix, Pr76 closed), FINDINGS 2026-10-30 §4)*

## Writing by hand: forms the controller's own macros use

These lines are from the macros the controller ships with. They show a FORM the parser takes; they were not run as bench tests.

- M98 P<n> calls the subprogram O<n> from the loaded system libraries, with argument words: M98 P501 X2. *(vendor macro: fndzero.nc; not a bench test)*
- A ; after a statement starts an end-of-line comment: IF #1301==0 GOTO4; *(vendor macro: slib-m.nc; not a bench test)*
