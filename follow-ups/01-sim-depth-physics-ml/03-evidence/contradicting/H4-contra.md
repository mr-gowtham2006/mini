# H4-contra — Falsification attempt: causal-layer split

Date: 2026-09-13 · Role: devil's advocate · Hypothesis: H4 (PCMCI+-alone wear-drift AC@1 <50%; graph-distance complement lifts ≥10pp to ≥50%; abrupt subset within ±10pp)
Falsification criteria targeted: Tripwire A (PCMCI-alone ≥50% on drift), B (split gain <10pp), C (abrupt regression ≥10pp); registered alternative A1 (simpler ordering/timing suffices) + A2 (windowing, not a second method, fixes drift).
Searches run: 5 (all 2026-09-13). Fetches: 0 (snippet-level; no PDF opened per TEXT ONLY rule).

## Contra evidence

### C-H4-1 (HIT vs Tripwire A — PCMCI-family success on manufacturing/drift-adjacent tasks)
- IEEE PHM-Xian 2025 (hierarchical PCMCI): "novel framework for temporal causality discovery and quality prediction tailored for multi-stage manufacturing processes… efficiently extracting root-cause process parameters" + multi-head-attention LSTM quality prediction. Manufacturing-native PCMCI win.
- RADICE (arxiv 2501.11545, Huawei): production RCA built directly on PCMCI+ (τmax-configured lagged + contemporaneous discovery) with false-positive filtering demonstrated on real performance-diagnostic cases.
- T-RCA (ACM 3627673.3680010, 2024): threshold-crossing → causal-graph → traversal framework "capable of identifying all true root causes under certain assumptions" — a causal-graph method succeeding via the very threshold-crossing order H4-A1 claims would bypass graphs.
- Verdict: HIT — PCMCI-family methods demonstrably clear manufacturing RCA bars; the <50%-on-drift premise (P1) is not a given. None is the exact T3 wear-knee regime, so Tripwire A is pressured, not fired.

### C-H4-2 (HIT vs the complement's novelty — graph-distance-for-drift is the SUPPORTING side's own evidence)
- PMC11207435 (deterioration tracking case study): reference causal graph (fresh components) vs subsequent graphs with **Jaccard distance** trend analysis; "when the Jaccard distance exceeds a predefined threshold set by domain experts, they can investigate" — the exact A-S7-direction mechanism H4 proposes, already field-demonstrated on a 6-month replacement cycle.
- ACM CIKM RoFaD (3357384.3357802): time-series-of-graphs method capturing "gradual and stable structured change" + failure propagation, explicitly fixing alert-flood-vs-noise-sensitivity tradeoff.
- StaR (ACM 3770855.3817863, 2026-09-05): memory-enhanced dynamic-graph RCA lifting AC@1 "from 0.028 to 0.920 on dynamic and stateful datasets… 0.381 to 0.752 on real-world datasets."
- Verdict: MISS as contra, recorded honestly — this line supports the complement rather than falsifying it. Its contra relevance is narrow: Jaccard-vs-reference needs a well-chosen reference graph + expert threshold (brittle reference-selection), and StaR shows a *unified* dynamic-graph model can do both regimes — pressuring the *split* (two methods) as opposed to one dynamic-graph method (Tripwire B adjacent).

### C-H4-3 (HIT — A1: simpler ordering/timing matches graph machinery)
- IFAC 2025 (OEE root-cause, CPG-based): root cause = anomalous node with no anomalous predecessor — pure topological-order + anomaly-timing query over a given graph, publicly simulated.
- MDPI Sensors 25(13):3980 (lag-specific transfer entropy): "scans candidate lags and selects the one that maximizes transfer entropy, delivering both the delay and the strength of every causal link… pinpoints the originating sensor" on TEP + three-phase-flow benchmarks — delay-timing alone traces disturbance paths.
- COKE (arxiv 2407.12254): chronological order + expert knowledge sequences variables by causal order in high-missingness manufacturing data without imputation.
- EPJC 026-15611-5 (CERN HCAL): "simple but novel compression algorithms for binary flag data… Bayesian network to query causality inference" on binary anomaly flags — binary-threshold-crossing order suffices at LHC scale.
- Verdict: HIT for A1 — four independent lines show ordering/timing-threshold machinery tracing faults without any graph-distance complement. H4-P3 (graph-free control does NOT match) is the leg most at risk; if BARO-style walk matches the complement on wear-drift, credit belongs to ordering.

### C-H4-4 (HIT — mask-always-helps contested; BARO baseline strength)
- BARO (arxiv 2405.09330, FSE'24): end-to-end RCA via multivariate Bayesian online change-point detection; SOTA baseline across subsequent papers (GALA 2508.12472 uses BARO as primary baseline and must beat it with LLM augmentation).
- CD-RCA (arxiv 2411.06990): causal-discovery RCA beats "heuristic attribution methods" BUT "Shapley value might encounter difficulties… when (1) amplitude of the root cause is lower than observational noise, or (2) there is no causal" edge — i.e. discovery adds nothing in exactly the low-amplitude-gradual regime wear-drift occupies.
- Mask2Cause (arxiv 2605.07280): unified structural mask essential (layer-wise/unshared masks degrade) — mask *design* matters more than mask *presence*.
- Verdict: HIT — the "masked discovery suffices, complement adds <10pp" (Tripwire B) outcome is plausible: strong change-point + walk baselines already cover abrupt/propagating, and discovery is weakest precisely on low-amplitude gradual onsets.

### C-H4-5 (HIT — A2 + Tripwire A mechanism: PCMCI failure is stationarity/timescale, fixable without a second method)
- Runge PCMCI+ (PMLR 124): "non-stationarity and especially autocorrelation can make causal discovery much less reliable"; PCMCI+ "even benefits from autocorrelation" by conditioning-set design.
- arXiv 2007.00267 (regime-dependent causality): "one of the general assumptions of PCMCI… is stationarity… known changes in the background signal can be accounted for by restricting the time series" — i.e. windowing/regime-splitting repairs drift-regime failure inside one method.
- PMC11207435 theory section: "PCMCI assumes stationarity, time-lagged dependencies, and causal sufficiency… not suitable for highly predictable systems with minimal new information at each time step… degradation typically manifests as a gradual change… differences between variables may not be substantial" (authors declined PCMCI for their deterioration case).
- Benchmark report OSTI 1991387 (gridded spatiotemporal PCMCI benchmark) + semi-stationary series work (arxiv 2407.07291): performance characterised as regime/window-dependent, not method-absent.
- Verdict: HIT for A2 — the documented failure mode (stationarity violation + low per-step novelty) is repaired by regime restriction / multi-scale windowing (H2's territory), not necessarily by adding Jaccard-distance machinery. If multi-scale PCMCI-alone recovers drift, Tripwire B fires.

## Falsification verdict: NOT FALSIFIED after 5 searches — genuinely contested
The contra lines are real (PCMCI-family manufacturing wins vs Tripwire A; ordering/timing sufficiency vs P3; window-repair vs Tripwire B) but the complement also has direct field precedent (C-H4-2, honestly recorded as supporting-side). No source ran the three-arms-plus-graph-free-control subset battery, so no tripwire fires. The highest-risk legs, in order: P3 (graph-free control matching) > drift-<50% premise (P1) > ≥10pp complement gain. Falsification strength: **Not falsified after 5 searches** (nearest to falsification of the five hypotheses on the P3 leg; the subset battery with the BARO-style control is decisive).
