# DDCS Expert M350 — examples

> Every measured statement names the bench test that measured it (see evidence.md).

Each file below is a bench probe that RAN on a real controller. Every code line is as it ran; comment lines about the authors' own tools are left out. "Result" is what was read back.

| file | controller | probe | date | result |
|---|---|---|---|---|
| `V10_operators.nc` | DDCS Expert M350 | V10_operators | 2026-06-23 | EQ branched (EQ_branched=1). |
| `V11_gotospace.nc` | DDCS Expert M350 | V11_gotospace | 2026-06-23 | IF #100==5 GOTO 1 (space before the label) branched. |
| `DWELL_units.nc` | DDCS Expert M350 | DWELL_units | 2026-06-23 | G04 P3000 waited about 3 s; G04 P3.0 was instant (P is milliseconds). |
| `V13f_atan_comma.nc` | DDCS Expert M350 | V13f_atan_comma | 2026-08-08 | ATAN[1, 2] = 26.565 degrees (popup ATANC=2657). |
| `IF_neg_test.nc` | DDCS Expert M350 | IF_neg_test | 2026-06-23 | Negative-decimal comparisons evaluated correctly. |
