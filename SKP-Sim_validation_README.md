# SKP-Sim — Reproducibility & Validation Artifact

Supplementary material for the paper. Contents:
- `SKP-Sim_test_cases.csv` — 29,100 cross-validation profiles and outputs.
- This README — base-value tables, scoring pseudocode, and the validation method.

## Scope of the encoding
- 208 catalogue entries (PS 61, OC 38, OP 61, PK 48), expanding the regulation's 190 numbered
  items (complexity-tiered items represented one entry per tier).
- Scores are by functional **jenjang** only (Ahli Utama/Madya/Muda/Pertama). The regulation does
  **not** differentiate Peneliti vs Perekayasa; every catalogue entry carries one score vector per level.
- Four competency items (further-study) are withheld for appraisees already holding a doctoral degree
  (a tool design choice; not a regulatory score rule).

## Scoring pipeline (pseudocode)
```
for each component k in {PS, OC, OP, PK}:
    Y_k   = sum over selected items i in k of  score_i[level] * qty_i
    c_k   = min(Y_k, cap_k[level])                 # per-component, per-level cap
    nd_k  = base_value[k][month][level]            # proportional base for the active period
    ndt_k = base_value[k][12][level]               # annual (12-month) base
    fk    = 0.5 if c_k < nd_k else 0.4             # two-regime correction coefficient
    Nplus_k = (c_k / nd_k - 1) * fk * nd_k          # = 0 if nd_k == 0
    Score_k = ndt_k + Nplus_k                        # baseline + correction (may be negative)
Total = round( sum_k Score_k , 2 )
z = (any Y_k == 0)                                   # zero-component guardrail
Rating = "Above" if (Total >= 110 and not z)
       else "Meets" if (Total >= 90)
       else "Below"
```
Base-value tables (12 months x 4 levels) and caps are transcribed from the regulation, Sublampiran 1.

## Cross-validation method
Two independent implementations of the pipeline — (i) the tool's engine and (ii) a separate
re-implementation transcribed independently from the regulation — were run over 29,100 profiles:
4 levels x 12 active-month settings x {0, below-base, at-base, at-cap, above-cap} per component.
This exercises both correction regimes, cap clamping, the zero-component guardrail, all three rating
bands, and every proportional-month base value.

**Result:** the implementations agreed on 29,096 / 29,100 profiles (99.99%). The four disagreements
all occur exactly at the 90.00 band edge: floating-point summation places the unrounded total an ULP
on either side of 90, flipping both the 2-decimal display and the band. **Mitigation:** round the total
to two decimals before the band comparison (or compare with a small epsilon). The agreement of the two
independent transcriptions also indicates the base-value tables are free of transcription errors.

## Anchor reference cases
| Profile | Level / months | Y (PS,OC,OP,PK) | Total | Rating |
|---|---|---|---|---|
| A | Madya / 12 | 25,30,80,25 | 120.00 | Above |
| B | Madya / 12 | 25,30,80,0  | 110.00 | Meets (guardrail) |
| C | Muda / 6   | 8,6,25,18   | 102.80 | Meets |
| D | Pertama/12 | 5,5,20,15   | 72.50  | Below |

## Catalogue audit (item-level)
The encoded catalogue (`SKP-Sim_catalog_audit.csv`) was audited for integrity and traceability:
- **208 entries** total — PS 61, OC 38, OP 61, PK 48 — each with a **unique identifier**.
- Every entry is a **well-formed level-indexed point vector** (Ahli Utama/Madya/Muda/Pertama).
- **21 entries** encode **per-level eligibility**: a `null` marks a level at which the activity does not apply
  (e.g. chairing a professor-council body applies only to senior levels; training-participant items only to
  junior levels). In scoring, a `null` contributes zero.
- **16 entries** are **complexity-tiered national-priority items** (four tiers — Very High / High / Medium /
  Simple — across the four components), carrying level-flat values 40/20/15/10. These are the main source of
  the expansion from the regulation's numbered items, and are the locus of a known modeling choice: they use
  fixed values rather than tracking the monthly base value (see paper limitations).
- **4 entries** (pk27–pk30: doctoral / further-study items) are withheld for appraisees already holding S3.
- **Value verification vs the regulation (expanded):** encoded point vectors were matched against the SK item tables for **179 of 208 entries — PS 52, OC 34, OP 53, PK 40 — with 0 discrepancies**. The remaining ~29 entries are the complexity-tiered national-priority extensions (no direct regulatory row) plus a few rows not cleanly machine-extractable; a full independent second-coder audit is the recommended next step.
- The **base-value and per-component cap tables** were verified line-by-line against the regulation
  (Sublampiran 1); an **independent sample of item point vectors** (e.g. ps01 5/5/5/5; ps04 5/5/5/5;
  oc01 35/35/30/30; op01 40/45/50/55; saksi-ahli 5/5/5/5) **matched the regulation with no discrepancies**.
- A **full second-coder value audit** of every entry against the 108-page regulation is **ongoing**; the
  traceability table above is provided to support it.

Columns of `SKP-Sim_catalog_audit.csv`: component, encoded_id, category, label, score_Utama, score_Madya,
score_Muda, score_Pertama, level_eligibility, doctoral_excluded.

## Human evaluation (updated)
- 14 questionnaire forms collected. Following **Option B**, the three paper authors' responses (developer + two co-authors) are excluded to remove self-assessment bias, leaving **n = 11** analysed.
- Overall pooled mean **4.56 / 5** (median 5; SD 0.67); highest: Efficiency 4.91; lowest: Anonymous-profile usefulness 3.91.
- Aggregates are reproduced (formula-driven) in `Agregasi_Respon_Manusia_SKP.xlsx`; results are essentially unchanged whether or not the authors' own responses are included.

## Unverified catalogue entries (appendix)
- `SKP-Sim_unverified_entries.csv` itemises the 29 entries not directly value-matched: **21 national-priority complexity-tier extensions** (flat 40/20/15/10 tier values; modelling extension) and **8 single-value/flat entries** whose regulatory rows wrap across lines in the source PDF (structurally checked; OP16 and OP46 value-confirmed manually).
