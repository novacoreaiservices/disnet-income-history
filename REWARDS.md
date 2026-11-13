# How Dispersed pays node operators (RNP-019 / RNP-021)

Sources: Render Network Proposals RNP-019 (passed May 2025, implemented
Mar/Apr 2025) and RNP-021 (enterprise GPUs); Messari "Understanding
Dispersed: A Comprehensive Overview" (July 2026, commissioned by the Render
Network Foundation). Secondary summaries agree on the structure below; the
exact per-job RENDER is set by the network's pricing, not a fixed table.

## The three reward components (weekly compute epochs)

- **Epoch**: Sunday 00:00 UTC to the following Sunday 00:00 UTC. Rewards are
  calculated per epoch. Current epoch at audit setup: Sun Nov 8 00:00 UTC to
  Sun Nov 15 00:00 UTC.
- **Availability**: **6 RENDER per weekly epoch per node at 100% uptime,
  prorated by uptime** -- the live Dispersed rate. RNP-019 set a BASELINE of
  2 RENDER/epoch/node and authorised the Foundation to raise it up to 3x;
  RNP-021 kept the 2 baseline text and reaffirmed the authority. The
  Foundation EXERCISED that authority: the live rate is 6/node (3x the
  baseline), NOT 2. Confirmed by the official @rendertoken account
  (Threads, 2026-03-18): operators "earn six RENDER tokens per epoch...
  simply for keeping their hardware online"; Messari (July 2026): "tops
  out at 6 RENDER per week at 100% uptime"; Dispersed docs (Oct 2026):
  "Maximum 6 RENDER per week at 100% uptime. A node online 50% of the
  week earns half that." The "2 RENDER" still quoted by trackers (CoinLaw)
  is the RNP baseline text, not the live rate. **Availability credits at
  epoch close, not intraday -- 0.00 mid-epoch is expected, not a shortfall.**
  His 3 nodes at ~100% uptime = ~18 RENDER per epoch total. The audit
  verifies the actual credit at the first epoch close (Nov 15): expected
  ~6 per node, ~18 total (slightly less if the Nov 11 restarts cost
  uptime). CORRECTION 2026-11-11: an earlier version of this file (and
  messages to the user) wrongly said 6 was only a ceiling and the live
  rate was 2 -- the user was right that it was increased, and it has NOT
  been changed back.
- **Job Execution**: formula **25 RENDER x Multiplier x (Hours Worked / 168)**
  per epoch (RNP-021 raised the 4090 baseline from 10 to 25, a 2.5x
  increase within the 3x remit). The Multiplier is a weighted composite,
  not a single per-GPU number: GPU architecture ~36%, VRAM ~26%, network
  bandwidth ~15% (removed as a factor by RNP-021), clock speed ~8%,
  memory bandwidth ~8%, CPU ~4%. GPU Model Scores from RNP-019 (the GPU
  component only): **RTX 5090 = 1.7, RTX 5070 Ti = 1.1, RTX 4090 = 1.0**.
  RNP-019's worked example gives an "upgraded" 4090 setup a compound
  multiplier of 1.8098. Exact per-rig compound multipliers are not
  published as single numbers -- the audit does not assume them.
- **Preparation**: time spent getting ready for jobs (model/image download,
  container setup). Small by nature (his Nov 11: 0.0003 RENDER).

## Settlement

- Consumers pay in fiat; credit converts to RENDER at the current rate.
  **5% goes to OTOY, 95% is burned** (Burn-Mint Equilibrium).
- **Earnings under $100 are held as an implicit deposit before
  disbursement** (RNP-019). At ~0.2 RENDER/day (~$0.50-0.80/day) the $100
  threshold is months away -- the audit tracks progress toward it.
- RENDER can be spent back on the network, or transferred to a wallet to
  stake / sell / trade.

## What the audit can and cannot verify

- Rig-side (no login needed): node up + connected, model loaded, jobs
  served (count, duration, HTTP status), errors, container/process restarts.
- Console-side (needs the operator API key, pk_/sk_ with HMAC auth, from
  the Dispersed Console's API keys section): per-node reward accruals
  (availability / preparation / execution), totals, and payout status.
  Until that key is provided, console totals in the audit state are only as
  fresh as the last console screenshot the user shares.

## Today (2026-11-11, from the console screenshot 13:58 EST)

- Total 0.2044 RENDER: Execution 0.2041, Preparation 0.0003, Availability 0.00.
- 4 execution accruals, all node ghelmen-sakpel (x570): 0.0595 + 0.0256 +
  0.0595 + 0.0595 = 0.2041. Matches the 4 /api/chat requests in the rig
  logs (2 at 09:38 EST on Gemma 4 31B, 2 at 12:15 EST on Qwen3 30B).
  Accrual timestamps lag the jobs and do not line up 1:1 -- the audit
  reconciles counts and totals, not timestamps.
- Availability 0.00 is correct mid-epoch (epoch closes Sun Nov 15 00:00 UTC).
- x870 and adam5070: connected all day, zero jobs, zero RENDER -- demand-side,
  not a breakage. Their availability still counts toward the epoch credit.
- **Formula check (open)**: the 4 jobs totalled ~314s (~0.087h) on the 5090
  and earned 0.2041 RENDER, but the RNP-021 formula predicts only ~0.013
  RENDER at multiplier 1.0 (~0.026 at a ~2x multiplier) for that little run
  time. Each 0.0595
  accrual equals exactly 10/168 (one hour of the old 10-baseline at
  multiplier 1.0). So execution pay for these tiny inference jobs does not
  follow the published time x multiplier formula as written -- per-job flat
  rate, container-allocation time, or a different inference price are all
  possible. The audit tracks this; resolving it needs the operator API
  (per-job accrual detail).
- **Verified 2026-11-11 14:22 EST against the primary RNP texts**
  (github.com/rendernetwork/RNPs, RNP-019.md + RNP-021.md, read via
  Lightpanda): the 19:11 EST correction to the user stands -- RNP-021 keeps
  availability at 2 RENDER/epoch/node, Foundation may raise it up to 3x,
  job baseline 10 -> 25 with formula 25 x Multiplier x (Hours/168).
  **No minimum-billing-hour and no inference-specific pricing clause exists
  in either RNP**, so a documented 1-hour floor is ruled out. Implied
  counted time per accrual under the live formula: at the 1.7 GPU score,
  0.0595 = ~14.1 min and 0.0256 = ~6.1 min; at the 1.81 compound example,
  ~13.3 / ~5.7 min. All readings imply counted time far above the ~70s
  inference per job and far below 60 min -- consistent with allocation
  windows (model load + inference + unload), not inference seconds alone.
  The three identical 0.0595 accruals also fit a per-job / per-allocation
  block credit. Net: the user is NOT being short-changed on these jobs --
  the console pays well above what the raw inference-time formula predicts,
  so if anything the formula under-predicts. Still unproven which counted
  time the console used; per-job console detail (operator API key) or rig
  allocation timestamps settle it. Tailscale SSH reads were blocked in the
  background-preparation environment (proxy 403) at 14:22 EST, so rig-side
  allocation times could not be pulled this run -- leave that to the hourly
  audit cron.

## Addendum 2026-11-13 — per-rig Compound Multipliers + Epoch 65 expectation

Re-verified from the primary RNP-019/RNP-021 texts (fetched 2026-11-13).
The "Multiplier" in 25 × Multiplier × (Hours/168) is the COMPOUND Multiplier
(CM = Σ W_i × M_i), NOT the raw GPU Model Score. The 5090's 1.7 is only the
GPU factor, weighted 36% inside CM. Weights: GPU 36%, VRAM 26%, network 15%
(removed as a factor by RNP-021 — its exact reweighting is unpublished),
clock 8%, memory bandwidth 8%, CPU 4%.

Per-rig CM (RNP-019 weights, network at medium = 1; range = without network):
- x870 (5090, 9900X): CM ≈ 1.35 (1.20–1.41) → 0.201 RENDER/counted hour; 33.8/wk at 100%
- x570 (5090, 3600):  CM ≈ 1.33 (1.18–1.39) → 0.198/h; 33.3/wk at 100%
- adam5070 (5070 Ti 16GB, 7600X): CM ≈ 0.89 (0.74–0.87) → 0.132/h; 22.3/wk at 100%
  (5070 Ti GPU score 1.1; VRAM factor 16/24 = 0.67 — note 16GB is below
  RNP-019's 24GB minimum spec, yet the node is accepted and earning.)

Console reconciliation 2026-11-13 17:50 UTC (69 rewards): execution
2.3018 = ghelmen 1.1711 + drewwegh 0.8794 + hedmehg 0.2513 (exact).
A typical 0.0418 accrual implies ~12.6 counted min on a 5090 rig and
~18.9 min on adam5070 at the CMs above — plausible container-allocation
windows. Per-accrual amounts are ~equal across GPUs because the 5090's
higher rate is offset by shorter counted time per job (it finishes faster);
the CM advantage shows up as throughput, not bigger per-job credits.

Epoch 65 availability expectation (closes Sun Nov 15 00:00 UTC): all three
nodes first verified online Nov 11 (13:01–13:31 UTC; adam5070 registered
late Nov 10 EST). From Nov 11 13:31 UTC = 82.5h = 49.1% of the epoch →
~2.95 RENDER/node at the live 6 rate; adam5070 up to ~3.41 if its
registration counts from ~00:30 UTC. Fleet expectation ≈ 9–10 RENDER
(~CA$20 at CA$2.16). Upside ~12–13 if x570's registration predates Nov 11.
Discriminator at close: ~1.0/node = baseline-2 only; ~3/node = 6-rate with
mid-epoch join (expected); ~6/node = 6-rate full week.
