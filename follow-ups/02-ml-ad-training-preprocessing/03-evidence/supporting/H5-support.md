# H5 supporting — thresholding-arm selection (Pct vs POT/SPOT vs GMM+Gamma)

Date: 2026-09-13 · Twin ML training · Parent F1 Locked (fixed-percentile primary bar) · Phase-1 base: A09 (POT/SPOT), A05 (GMM+Gamma), A01/A04/A06–A08 (threshold-agnostic/diagnostic companions)

## Registered claim (H5)
Three calibration arms distinguishable on twin data; joint criterion (raw-F1 + 20–150/1000 alert rate) picks a budget-stable winner (≥2-of-3 budgets 20/150/5).

## Supporting findings

### S-H5-1 — POT/SPOT/DSPOT EVT: distribution-free threshold from single risk q (Phase-1 A09 re-verified abstract-level, L1)
- Source: https://www.kdd.org/kdd2017/papers/view/anomaly-detection-in-streams-with-extreme-value-theory (KDD17 Siffer et al., landing+abstract fetched; full text per source-table A09 p1–4 already: POT/GPD z_q via Grimshaw MLE, SPOT stationary + DSPOT drift, streaming) + https://hal.science/hal-01640325/document (spoiler: HAL now serves Anubis PoW — fetch blocked this run; prior extraction stands)
- Claim: EVT-POT estimates z_q with P(X>z_q)<q without distribution assumption or hand-set threshold; single risk parameter controls FP; SPOT streaming update; DSPOT covers abrupt drift, not knee-drift; needs enough peaks N_t (thin at T=300).
- Level/confidence: L1 / High for mechanism, Medium for twin fit (stationarity-vs-wear tension = H5 prediction 3; thin-peaks caveat = Gap 4 core).
- Supports H5 vs alternatives: Arm-POT design (pre-registered q) + predicts POT variance across budgets and degradation on post-knee wear-drift subset; counters A1 (simplest-wins) only if POT separates — battery decides.

### S-H5-2 — M2AD GMM+Gamma dependence-correct calibration, χ² miscalibration Prop. 2 (Phase-1 A05 re-verified, L1)
- Source: https://proceedings.mlr.press/v258/alnegheimish25a.html (fetched abstract+bib; mechanics per source-table: per-sensor GMM → p-values → Fisher → Gamma; area error; status covariates)
- Claim: naive χ² aggregation miscalibrates under dependence; Gamma calibration corrects; global score over heterogeneous sensors/systems with theoretical heterogeneity/dependence treatment.
- Level/confidence: L1 / High for calibration mechanics (Prop. 2 cited), Low for magnitude (+21% overlap-TP INADMISSIBLE).
- Supports H5 vs alternatives: Arm-GG design + prediction 1 (lowest worst-machine alert rate via dependence correction on 32-machine joint scoring); A3 (GG tax without gain) companion cost check retained.

### S-H5-3 — Fixed-percentile / max-validation as production-grade baseline (NEW, L1 method fact + L3 preprint practice)
- Sources: GDN §3.6+§4.3 (fetched: "threshold as the max of A_s(t) over the validation data", L1) + https://arxiv.org/html/2606.00052 §III-D Eq.7 (fetched: per-mode Percentile95 thresholds τ_k, L3)
- Claim: max-validation and P95-per-regime thresholds are the published operating rules in both the GDN SOTA system and the mode-aware TEP study — fixed-percentile is not a strawman, it is the incumbent.
- Level/confidence: L1/L3 / High for incumbency.
- Supports H5 vs alternatives: Arm-Pct legitimacy (F1-locked home turf, prediction 2: competitive raw-F1, loses joint criterion on worst-machine rates); A1 branch (keep Pct) is a respectable resolution, not a failure.

### S-H5-4 — Alert-budget-as-hard-input production practice: threshold-to-budget-then-recall (NEW, L4 practitioner, snippet+prior-row)
- Sources: Phase-1 J13 (Technolynx: "handful/shift", threshold-to-budget-then-recall) + search-corroboration this run: TheCodeForge ("99.5th percentile over last 7 days, recomputed daily; t-digest/Greenwald-Khanna streaming; per-metric/per-entity thresholds"), Arun Baby ("conservative paging thresholds; historical replay for alert volume; shadow mode"), MAY2704 repo ("--alert-budget 20 ... --threshold-percentile 99.9")
- Level/confidence: L4 / Medium for practice pattern (no benchmarks; patterns only per quarantine rule).
- Supports H5 vs alternatives: H5's joint criterion (F1 + alerts/1000 at 20/150/5, global + worst-machine) IS the production practice formalized — supports budget-conditional (A2/H5-ambiguous) resolution legitimacy; Multigrid 5σ-budget math fetch TIMED OUT (logged miss — prior J26 snippet stands, snippet-only).

### S-H5-5 — EVT rarity framing + FluxEV SPOT-efficiency note (UNVERIFIED full text, abstract fallback per TEXT-ONLY rule)
- Source: https://sdiaa.github.io/papers/WSDM21.pdf (FluxEV: "SPOT is streaming POT... MOM competitive with MLE, 4–6× efficiency" — PDF binary fetch, TEXT-ONLY rule → UNVERIFIED full text, §4.4.1 characterization via search snippet)
- Claim (fallback): SPOT usable inside unsupervised TSAD pipelines; estimator choice (MOM vs MLE) moves calibration cost.
- Level/confidence: L2 UNVERIFIED / Low (never sole support).
- Supports H5 vs alternatives: feeds A3 cost companion (cost(GG)/cost(Pct) ≥10 leg needs GG AND POT timing) — direction only.

## How support maps to H5 tripwire
- F1-spread ≥2pp OR alert-spread ≥3/1000 leg: S-H5-1 (POT variance/stationarity tension) + S-H5-2 (GG worst-machine precision edge) predict separation on the precision side even if F1 is close (prediction 1).
- Stability ≥2-of-3 budgets: S-H5-4 (budget-conditional practice) makes H5-ambiguous a first-class resolution.
- Wear-drift POT degradation (prediction 3): S-H5-1 DSPOT-limits + thin-N_t-at-T=300 caveat.

## Gap-pass per sub-hypothesis
- POT/SPOT twin fit (Gap 4): PARTIAL — follow-up query: "SPOT DSPOT short time series few peaks threshold stability EVT".
- GMM+Gamma worst-machine edge: PARTIAL (mechanics strong, numbers inadmissible) — follow-up query: "Gamma calibration Fisher p-value aggregation dependent sensors false alarm".
- Fixed-percentile incumbency + budget-conditional practice: SUPPORTED — follow-up query: "alert budget threshold calibration anomaly detection production shadow re-measure".
