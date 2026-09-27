# H4 supporting evidence — Causal-layer split (PCMCI+ for abrupt, graph-distance for wear drift)

Date: 2026-09-13 · Scope: 32-machine SimPy twin · Battery guards: Locked F1–F3 respected; conditional F4 pressured only in wear-drift regime.
Claim tested: PCMCI+-alone wear-drift (T3) AC@1 <50%; split arm (masked PCMCI+ tau=2 + reference-graph Jaccard-distance complement) recovers ≥50% with ≥10pp gain and <10pp abrupt-subset regression.

## Findings (supporting only)

### S-H4-01 — On real manufacturing data, PC/FCI beat PCMCI at graph recovery (repo VERIFIED, numbers SNIPPET-ONLY)
- Source: repo https://github.com/causalgraph/causRCA (VERIFIED: Fraunhofer IWU causRCA dataset — real vertical-lathe time series, HIL fault scenarios with ground truth, expert causal graph, eval scripts for CD Table 3 / RCA Tables 5–7 — via full fetch) + paper Mehling et al., Procedia CIRP 139:114–120 (2026) → **L3**; the score numbers below are **snippet-only from the paper PDF excerpt, UNVERIFIED** (PDF not opened per TEXT-ONLY rule).
- Numbers (UNVERIFIED): CD F1 — PC/FCI 0.29–0.40, FGES 0.13–0.27, PCMCI 0.16–0.29; causal-RCA MAP@3 — PC/FCI graphs 0.37–0.87, PCMCI graphs 0.21–0.56.
- HOW it supports H4: on real manufacturing data with an expert graph, the PCMCI family underperforms constraint-based alternatives at recovering the graph that RCA consumes — the PCMCI-alone weakness leg (Tripwire A direction) on exactly the data regime the twin mimics. Also validates H4's battery design: expert/graph-F1 + downstream RCA scores as joint evaluation.
- Vs alternative A1 (simpler ordering suffices): the paper's Table 7 evaluates causal-RCA over graphs from different CD algorithms — i.e. the graph, not mere ordering, moves RCA scores; and the repo ships the graph-free-vs-graph comparison harness H4 needs.
- Confidence: moderate for existence/design (verified repo); LOW for magnitudes (snippet-only). Gap-pass item G-H4-a: confirm Table 3/7 numbers from the HTML/abstract page or author data sheet.

### S-H4-02 — PCMCI+ collapses on nonlinear / trend-seasonal regimes (VERIFIED full text)
- Source: https://arxiv.org/html/2506.01361v2 → fetched v1 full text — Ferdous et al., TimeGraph, KDD'25 → **L3** (peer-reviewed conference).
- Numbers (Table 2, n=500/lag-2 slice): B1 nonlinear-polynomial Gaussian — PCMCI+ TPR 0.00 / FDR 1.00 / SHD 10; C1 trend+seasonality Gaussian — PCMCI+ TPR 0.00 / FDR 1.00; linear A1 Gaussian — PCMCI+ TPR 1.00 / FDR 0.00. LPCMCI/PC/FGES also degrade on B1/C1 (no method dominates drifted regimes).
- HOW it supports H4: the regime split is real and measured — PCMCI+ is near-perfect on stationary-linear and near-vacuous on nonlinear/trending (the wear-drift signature: slow, predictable, nonstationary across the knee). Supports H4-P1 (PCMCI+-alone <50% on drift while holding abrupt) and the "window/scale won't save it" reading (failure is regime, not tuning — all four methods fail B1/C1 together).
- Caveat (supports H4-A2 honestly): since every method fails the drifted variants, TimeGraph alone does not prove the graph-distance complement succeeds — it proves the PCMCI leg needs a complement. The complement's win must come from A-S7 + battery.
- Confidence: moderate-high (peer-reviewed benchmark, open generation scripts + protocols).

### S-H4-03 — PCMCI assumes stationarity; regime/confounder variants exist upstream (VERIFIED official docs)
- Source: https://jakobrunge.github.io/tigramite/ + https://github.com/jakobrunge/tigramite (maintainer-authored) → **L3**.
- Claims: "Assuming stationarity, the links are repeated in time"; LPCMCI allows latent confounders; RPCMCI extracts regime-dependent graphs; dataframe supports masked time series.
- HOW it supports H4: the assumption boundary H4 exploits is admitted by the method's own documentation — wear drift across a knee (nonstationary, highly predictable per-step) sits outside PCMCI's design envelope, so a second layer for that regime is principled, not ad hoc. Masked-data support corroborates the twin's topology-mask + flip-gate posture for the abrupt leg.
- Confidence: high for the assumption statements (primary source).

### S-H4-04 — Hierarchical PCMCI extracts root-cause parameters in multi-stage manufacturing (SNIPPET-ONLY)
- Source: https://doi.org/10.1109/phm-xian66756.2025.11427842 — PHM-Xi'an 2025 → **L3 snippet-only**.
- Claim (excerpt): hierarchical PCMCI for causal analysis over multi-stage process time series, "efficiently extracting root-cause process parameters," cutting noise interference, feeding multi-head-attention LSTM quality prediction.
- HOW it supports H4: structured/masked PCMCI winning on abrupt/propagating multi-stage faults — the keep-PCMCI-for-abrupt half of the split. Scoped as directional only.
- Confidence: low (snippet). Gap-pass item G-H4-b.

### Phase-1 cache rows reused (no re-fetch)
- A-S7 (full text, Phase 1): adapted FCI + Jaccard-vs-reference-graph tracks gradual degradation "where PCMCI is unsuitable (minimal new info per step)" — H4's complement mechanism itself; the single most on-point row.
- A-S1: PCMCI+ contemporaneous orientation is the weak leg; stationarity + sufficiency assumptions — the ceiling statement.
- A-S3: priors improve discovery accuracy — mask-conditionality backing for the abrupt leg.
- I28 (PyRCA docs, fetched Phase 1): ε-diagnosis + topology/causal-graph scoring with domain constraints — walk+mask architecture validation (complements, does not test drift).

## Net assessment (supporting lane only)
PCMCI-alone weakness on drifted/real-manufacturing regimes is supported from three independent angles (benchmark collapse S-H4-02, real-data underperformance S-H4-01 directional, assumption boundary S-H4-03). The complement's positive win still rests on single-source A-S7 + the to-run battery — flagged as the load-bearing gap (G-H4-c: second independent graph-distance-for-drift source).
