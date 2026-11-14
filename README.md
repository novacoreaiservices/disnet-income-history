# disNet / Dispersed RENDER income history

Hourly log of Novacore's three disNet (Dispersed) nodes' RENDER earnings on
the Render Network — the disNet mirror of
[sogni-income-history](https://github.com/novacoreaiservices/sogni-income-history).

## Nodes

| Console name | Rig | GPU |
|---|---|---|
| ghelmen-sakpel | novacore-x570 (NFT #463) | RTX 5090 |
| hedmehg-megsegh | novacore-x870 (NFT #462) | RTX 5090 |
| drewwegh-domster | adam5070 (NFT #478) | RTX 5070 Ti |

## Files

- `hourly-income.csv` — one row per hourly audit run. Console columns are
  cumulative accruals for the current epoch as of `console_as_of_utc`; they
  only change when a fresh console read lands, so rows repeat them between
  reads. Job counts and the RENDER price refresh every run.
- `latest.json` — the most recent snapshot, machine-readable.
- Availability estimate columns (`avail_est_*`): the console shows
  availability as 0.00 until the epoch closes, so each row also carries the
  prorated estimate — 6 RENDER per node per epoch times the fraction of the
  epoch the node has been registered (fleet stood up 2026-11-11, ~49% into
  Epoch 65, so Epoch 65 expects ~8.9 RENDER fleet-wide at close).
  `epoch_expected_total_*` = console total so far + availability expected
  at close.
- **Accurate pay (`accurate_*` columns) — the headline figures.** The
  console pays execution at base rate 10 x compound multiplier 1 (a flat
  0.0595 RENDER per metered hour on every node, verified from the
  per-reward calculation inputs). RNP-021 (status: Implemented) sets the
  base rate to 25, and RNP-019 sets each node's compound multiplier —
  x870 ≈ 1.806, x570 ≈ 1.782, adam5070 ≈ 1.341 — so accurate execution =
  console execution x 2.5 x CM, per node. `accurate_total_*` = accurate
  execution + accrued availability estimate + preparation as recorded;
  `accurate_expected_close_*` = accurate execution + availability
  expected at close + preparation. Availability does not use the base
  rate or CM, so it is not repriced; preparation is kept as recorded
  (suspected under-metered, not repriced — see REWARDS.md).
  `exec_wedpel_x570_render` is execution the console credited to
  wedpel-sterbher — x570's node under its root identity on
  2026-11-13/14; it is folded into x570's accurate figure. The
  2026-11-11 seed row predates per-node splits, so its accurate cells
  are blank.
- `REWARDS.md` — the pay model this log is audited against (RNP-019/021):
  availability 6 RENDER/epoch/node at 100% uptime (credited at epoch close),
  execution 25 x Compound Multiplier x (hours worked / 168), preparation.

## Notes

- Epochs run Sunday 00:00 UTC to Sunday 00:00 UTC. Epoch 65 = 2026-11-08 to
  2026-11-15; the fleet stood up 2026-11-10/11, so Epoch 65 is a partial week.
- Accrued is not disbursed: wallet payouts queue 24-48h after an epoch
  finalizes and amounts under the disbursement threshold are held.
