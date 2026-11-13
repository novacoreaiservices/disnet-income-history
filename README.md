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
- `REWARDS.md` — the pay model this log is audited against (RNP-019/021):
  availability 6 RENDER/epoch/node at 100% uptime (credited at epoch close),
  execution 25 x Compound Multiplier x (hours worked / 168), preparation.

## Notes

- Epochs run Sunday 00:00 UTC to Sunday 00:00 UTC. Epoch 65 = 2026-11-08 to
  2026-11-15; the fleet stood up 2026-11-10/11, so Epoch 65 is a partial week.
- Accrued is not disbursed: wallet payouts queue 24-48h after an epoch
  finalizes and amounts under the disbursement threshold are held.
