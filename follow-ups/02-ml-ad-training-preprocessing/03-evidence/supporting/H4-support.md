# H4 supporting — per-machine robust norm beats global min-max by ablation margin

Date: 2026-09-13 · Twin ML training · Parent F1 Locked · Phase-1 base: A02 (per-sensor median/IQR), A03 (min-max train-stats) — zero retrieved papers compare scopes (open gap)

## Registered claim (H4)
Per-machine per-sensor robust (median/IQR, RUN-normal-only, inside folds) beats global-per-channel min-max by ≥+2pp raw-F1 (heterogeneity-concentrated on C/ASM/RWK).

## Supporting findings

### S-H4-1 — GDN per-sensor median/IQR robust error norm (Phase-1 A02 re-verified, L1)
- Source: https://ar5iv.labs.arxiv.org/html/2106.06947 (§3.6 Eq.12, fetched): a_i(t) = (Err_i(t) − μ̃_i)/σ̃_i with μ̃ = median, σ̃ = IQR across time; "more robust against anomalies" than mean/std.
- Claim: robust per-sensor normalization prevents high-variance sensors drowning stable ones.
- Level/confidence: L1 / High (method fact in SOTA-beating system on SWaT/WADI raw).
- Supports H4 vs alternatives: Arm-M's exact estimator (median/IQR) is the GDN-proven choice; counters A1 (global wins) at precedent level — but scope comparison (per-machine vs global) still awaits twin ablation.

### S-H4-2 — RobustScaler official semantics: median + IQR, fit-on-train, outlier-robust (NEW, L3 official docs, fetched)
- Source: https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html (fetched: "outliers can often influence the sample mean/variance in a negative way... median and IQR often give better results"; fit computes median/quantiles on training set, transform applies later)
- Claim: median/IQR scaling robust to outliers; fit/transform separation is the leakage-discipline primitive.
- Level/confidence: L3 (official docs) / High for semantics, Medium for AD transfer (general guidance + outlier-detection example gallery).
- Supports H4 vs alternatives: licenses H4 verification method (RUN-normal-only inside folds; test transformed never refit) + interim default M; counters leakage-oblivious global-min-max practice.

### S-H4-3 — Scope axis formalized: dataset-level vs instance-level statistics + mean/std-vs-minmax ranking (NEW, L3 preprint full-text)
- Source: https://ar5iv.labs.arxiv.org/html/2512.02833 (Ahmed et al., TSFM normalization study, full HTML fetched)
- Claim: normalization defined by (i) statistics (mean-std/min-max/max-abs) × (ii) scope (dataset-level vs instance/window-level); across 4 TSFMs × 6 datasets: mean/std normalizers (RevIN/Hybrid/Standardization) beat alternatives by 25–45% avg; RevIN ZS MASE 1.02 vs MinMax 1.22 / MaxAbs 2.58 / raw 9.38; statistics choice outweighs scope (≤0.04 MASE among mean/std trio); Lag-Llama uses median/IQR for outlier robustness; absence of normalization degrades performance ~30% (cited).
- Level/confidence: L3 / Medium-High for statistics ranking (forecasting MASE, not AD F1), Medium for scope framing transfer.
- Supports H4 vs alternatives: (a) min-max is empirically the weak statistic — supports M-over-G direction against A1; (b) scope×statistic factorization is the exact H4 ablation design (vary scope, hold estimator); (c) per-series/instance framing supports per-machine granularity. Caveat: forecasting/zero-shot, not detection — magnitude NOT transferable.

### S-H4-4 — Time-series normalization comparison: MaxAbs-suggested alternative to z-norm; high-variance-channel dominance (NEW, L2 abstract + L4 practitioner, snippet-level)
- Sources: https://www.sciencedirect.com/science/article/abs/pii/S2214579623000400 (Lima & Souza, Big Data Res. 2023: 10 methods × 3 classifiers × 38 datasets; abstract via search) + Lacuna practitioner note (median/quantile scaling; "prevents high-variance sensors from drowning out stable ones" citing GDN)
- Claim: normalization choice materially moves time-series mining results; high-variance dominance is the failure mode per-machine scope treats.
- Level/confidence: L2 abstract SNIPPET-ONLY + L4 / Low-Medium (classification task, not AD; extrapolation noted in-source).
- Supports H4 vs alternatives: heterogeneity-concentration prediction (minority-class/worst-machine effects) — counters A2-oblivious (backbone-erased) reading at mechanism level; cross-backbone interaction test still required.

## How support maps to H4 tripwire
- Direction (M−G ≥+2pp): S-H4-1 (robust estimator in winning system) + S-H4-3 (min-max weak; robust/mean-std strong) — convergent mechanism support, no twin magnitude.
- Heterogeneity concentration (prediction 2): S-H4-4 (variance-dominance failure mode) + S-H3-2 spillover (global manifolds loosen on heterogeneous modes).
- Backbone-invariance (prediction 3) / personalization-substitution (A3): NO new evidence — retained as battery interactions.

## Gap-pass per sub-hypothesis
- Robust-vs-minmax estimator leg: SUPPORTED — follow-up query: "RobustScaler median IQR vs MinMaxScaler anomaly detection F1 comparison".
- Per-machine vs global SCOPE leg (Gap 1 core): PARTIAL (scope formalism + variance-dominance mechanism, zero direct scope-comparison papers — gap narrows, not closed) — follow-up query: "per-sensor per-machine vs global normalization multivariate anomaly detection ablation".
- Anomaly-robust fitters (contamination-proof quantiles): PARTIAL via RobustScaler docs — follow-up query: "anomaly-robust scalerWinsorized trimmed normalization detection benchmark".
