# DDCS Expert M350 — community notes that disagree with measurements

> Every measured statement names the bench test that measured it (see evidence.md).

Each row is something the community notes say that our side contradicts. **The measured column is the truth on this controller** until the re-verification (a bench probe on the controller, run for both sides) says otherwise.

| | the notes say | measured | re-verification |
|---|---|---|---|
| goto-space | A space before the GOTO label is an error (write GOTO1, never GOTO 1). | IF #100==5 GOTO 1 (a space before the label) is accepted. *(V11_gotospace.nc)* | open: `AUDIT_XC_goto-space.nc` |
| inline-then | IF <condition> THEN <statement> is used as a valid form. | An inline IF <cond> THEN <assignment> is rejected; the Expert does IF <cond> GOTO <label> only. *(t2070)* | open: `AUDIT_XC_inline-then.nc` |
| mod | MOD is used as an operator. | MOD is a syntax error; there is no power operator (^ and ** are syntax errors). *(photographed pendant errors)* | open: `AUDIT_XC_mod.nc` |
| atan-slash | Two-operand ATAN is written ATAN[y]/[x]. | Two-operand ATAN must be the comma form ATAN[y, x] (ATAN[1, 2] = 26.565, degrees); the slash form ATAN[y]/[x] and one-operand ATAN[x] are syntax errors. *(V13_trig.nc, V13f_atan_comma.nc)* | open: `AUDIT_XC_atan-slash.nc` |
| param-below-500 | A machine parameter Pr<N> (N < 500) is read or set as macro #<N> (e.g. #118 homing speed, startup #107/#118). | Macro #N from #500 to #1499 is the Param page parameter Pr(N - 500): the same index as the setting file and the Modbus register 6500 + 2 x (N - 500). #805 is Pr305 (G54 X, register 7110) and #900 is Pr400 (H01 tool length, register 7300). A write there changes a machine setting. *(Modbus reads of the whole parameter table against the setting file (FINDINGS section 28))* | open: `AUDIT_XC_param-below-500.nc` |
