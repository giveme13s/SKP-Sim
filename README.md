# SKP-Sim

**SKP-Sim** is a lightweight, transparent *regulation-as-code* tool that simulates the
Sasaran Kinerja Pegawai (SKP) score for BRIN researchers and engineers under
**Kep. KaBRIN No. 15/I/HK/2026**.

It is a single, self-contained HTML file (React + Babel loaded from a CDN at runtime):
just open it in a modern web browser — no installation, build step, or server is required,
and no data leaves the browser.

## What it does
- Encodes the official activity catalogue (**208 entries** across Peran Strategis, Outcome,
  Output, and Peningkatan Kompetensi), the level-indexed weights, monthly base values, caps,
  the two-regime correction factor, rating bands, and the zero-component guardrail directly as
  inspectable data tables and deterministic scoring functions.
- Exposes **intermediate quantities** (per-component achievement, caps, correction terms,
  rating band) — not just the final score — so users can see *why* a score was produced.
- Supports *what-if* simulation and a gap-closing advisory heuristic for under-target components.

## How to use
1. Download `Simulasi Nilai SKP - PRSDI.html`.
2. Open it in a browser (Chrome, Edge, Firefox).
3. Select your functional level and active months, then add activity items to simulate the score.

## Validation artefacts (supplementary)
The accompanying paper reports a layered validation. The supplementary files are:
- `SKP-Sim_catalog_audit.csv` — 208-entry traceability / structural check.
- `SKP-Sim_unverified_entries.csv` — the 29 entries not directly value-matched, with reasons.
- `SKP-Sim_test_cases.csv` — synthetic cross-validation profiles.
- `Agregasi_Respon_Manusia_SKP.xlsx` — human-evaluation responses and aggregates.
- `SKP-Sim_validation_README.md` — methodology notes.

## Citation
If you use SKP-Sim, please cite the accompanying IC3INA 2026 paper:
*SKP-Sim: Transparent Regulation-as-Code for Public-Sector Researcher Performance Planning.*

## Disclaimer
SKP-Sim is an **unofficial self-service planning aid**, not an official appraisal system of
record. It does not adjudicate supporting evidence, appraiser judgement, or administrative
eligibility. Always confirm results against the regulation and official systems.

## License
For the source regulation see the BRIN JDIH portal
(https://jdih.brin.go.id/dokumen-hukum/peraturan/view/a19e030c-2ab3-4cc6-9c0e-744e8c2049c2).
