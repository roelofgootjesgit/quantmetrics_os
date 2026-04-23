# Guard attribution — Level B (rerun compare)

## Runs

- **Baseline** (`qb_run_20260423T044439Z_6b7e5f37`): EXP-2025-baseline
- **Variant** (`qb_run_20260423T044453Z_04c07854`): EXP-2025-D1-risk

## Trade outcomes (realized)

| Metric | Baseline | Variant | Delta (var − base) |
|---|---:|---:|---:|
| Trades | 36 | 42 | 6 |
| Mean R | 0.33333333333333637 | 0.33333333333333487 | -0.0 |
| Sum R | 12.00000000000011 | 14.000000000000064 | 2.0 |
| Win rate % | 44.44444444444444 | 42.857142857142854 | -1.587302 |
| Max DD (R) | -9.0 | -4.499999999999971 | 4.5 |
| PF ~ | 1.6000000000000056 | 2.166666666666672 | 0.566667 |

## Guard blocks (BLOCK counts)

| Guard | Baseline | Variant | Δ blocks |
|---|---:|---:|---:|
| daily_loss_cap | 19 | 0 | -19 |
| regime_allowed_sessions | 76 | 85 | 9 |
| max_trades_per_session | 7 | 11 | 4 |
| regime_profile | 10 | 10 | 0 |

### Focus guard

- `regime_allowed_sessions`: blocks 76 → 85 (Δ 9)

## Notes

- Deltas use **variant − baseline**.
- Interpret together with config diff (single-guard experiments).

