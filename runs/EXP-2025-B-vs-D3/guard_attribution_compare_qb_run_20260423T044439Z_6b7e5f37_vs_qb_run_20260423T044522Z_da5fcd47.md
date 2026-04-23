# Guard attribution — Level B (rerun compare)

## Runs

- **Baseline** (`qb_run_20260423T044439Z_6b7e5f37`): EXP-2025-baseline
- **Variant** (`qb_run_20260423T044522Z_da5fcd47`): EXP-2025-D3-trend-NY

## Trade outcomes (realized)

| Metric | Baseline | Variant | Delta (var − base) |
|---|---:|---:|---:|
| Trades | 36 | 26 | -10 |
| Mean R | 0.33333333333333637 | 0.7307692307692373 | 0.397436 |
| Sum R | 12.00000000000011 | 19.00000000000017 | 7.0 |
| Win rate % | 44.44444444444444 | 57.69230769230769 | 13.247863 |
| Max DD (R) | -9.0 | -3.0 | 6.0 |
| PF ~ | 1.6000000000000056 | 2.727272727272743 | 1.127273 |

## Guard blocks (BLOCK counts)

| Guard | Baseline | Variant | Δ blocks |
|---|---:|---:|---:|
| regime_allowed_sessions | 76 | 103 | 27 |
| daily_loss_cap | 19 | 3 | -16 |
| max_trades_per_session | 7 | 6 | -1 |
| regime_profile | 10 | 10 | 0 |

### Focus guard

- `regime_allowed_sessions`: blocks 76 → 103 (Δ 27)

## Notes

- Deltas use **variant − baseline**.
- Interpret together with config diff (single-guard experiments).

