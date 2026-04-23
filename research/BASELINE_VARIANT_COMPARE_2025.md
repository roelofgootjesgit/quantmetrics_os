# Niveau B — baseline vs expansion-dominance varianten (2025)

Gegenereerd op basis van consolidated QuantLog runs (kalenderjaar 2025, strict prod baseline).

| Vergelijking | Baseline `run_id` | Variant `run_id` |
|----------------|-------------------|-------------------|
| B vs **D3** (trend NY only) | `qb_run_20260423T044439Z_6b7e5f37` | `qb_run_20260423T044522Z_da5fcd47` |
| B vs **D1** (risk uplift) | `qb_run_20260423T044439Z_6b7e5f37` | `qb_run_20260423T044453Z_04c07854` |

## Artefacten (Markdown + JSON)

- `runs/EXP-2025-B-vs-D3/guard_attribution_compare_qb_run_20260423T044439Z_6b7e5f37_vs_qb_run_20260423T044522Z_da5fcd47.md`
- `runs/EXP-2025-B-vs-D3/guard_attribution_compare_qb_run_20260423T044439Z_6b7e5f37_vs_qb_run_20260423T044522Z_da5fcd47.json`
- `runs/EXP-2025-B-vs-D1/guard_attribution_compare_qb_run_20260423T044439Z_6b7e5f37_vs_qb_run_20260423T044453Z_04c07854.md`
- `runs/EXP-2025-B-vs-D1/guard_attribution_compare_qb_run_20260423T044439Z_6b7e5f37_vs_qb_run_20260423T044453Z_04c07854.json`

## TL;DR (realized, variant − baseline)

### D3 — trend alleen New York

- **Trades:** 36 → 26 (−10)
- **Mean R:** +0.33 → +0.73 (**+0.40R**)
- **Sum R:** +12 → +19 (**+7R**)
- **WR:** 44.4% → 57.7% (**+13.2pp**)
- **Max DD (R-pad):** −9 → −3 (**+6R** gunstiger)
- **PF:** 1.60 → 2.73 (**+1.13**)

**Guard blocks:** `regime_allowed_sessions` 76 → 103 (+27) — meer blok-events in de variant-run (o.a. doordat meer cycli tegen de smallere trend-session-set aanlopen); combineer met funnel-analyse, niet als “minder strict” lezen.

### D1 — expansion 1.5× / trend 0.5× sizing

- **Trades:** 36 → 42 (+6) — interactie met **daily_loss** / sizing-pad
- **Mean R:** ~gelijk op 1R-basis per trade-sim (**~0**)
- **Sum R:** +12 → +14 (**+2R**)
- **WR:** iets omlaag (−1.6pp)
- **Max DD:** −9 → −4.5 (**+4.5R** gunstiger)
- **PF:** 1.60 → 2.17 (**+0.57**)

**Guard blocks:** `daily_loss_cap` 19 → 0 (−19) — sterke aanwijzing dat sizing/cumulatief pad de loss-cap minder vaak raakt.

## Opdracht voor vervolg

- D3: **promote-candidate** op 2025-data; daarna **slice-stabiliteit** (H1/H2 of kwartalen) en live/paper checklist.
- D1: **sum-R / PF / DD** verbetering zonder mean-R lift; nuttig als **risico-/pad-tuning**, minder als pure expectancy-upgrade.

Herhaal compare na elke nieuwe run-id pair met dezelfde CLI (`quantmetrics-guard-attribution-compare`).
