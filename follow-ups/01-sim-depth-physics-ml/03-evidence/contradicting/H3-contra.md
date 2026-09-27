# H3-contra — Falsification attempt: precision-side bar

Date: 2026-09-13 · Role: devil's advocate · Hypothesis: H3 (≥1 top-quartile raw-F1 setting breaches 10 alerts/1000 healthy windows and is demoted; rank-reversal persists at 5 and 20/1000)
Falsification criterion targeted: zero top-quartile settings breach 10/1000; registered alternative A1 (budget miscalibration — demotion is threshold artifact).
Searches run: 5 (all 2026-09-13). Fetches: 0 (snippet-level evidence; no PDF opened per TEXT ONLY rule).

## Contra evidence

### C-H3-1 (HIT — plants tolerate 10–15% FPR on ordinary assets; 10/1000 may be stricter than plant trust)
- oxmaint Predictive Maintenance False Alarm Management (2026-07-18): "Best-in-class plants operate below 10 percent false positives. Anything above 20 percent begins eroding technician trust, and above 35 percent you enter alarm fatigue territory… for run-of-the-mill pumps and motors, 10 to 15 percent is defensible" (safety-critical: <5%).
- iFactory AI-vision inspection (2026-08-04): "practical target is below 2%… the specific threshold at which operators begin routinely overriding the system — typically 3 to 5% — is the hard ceiling."
- Verdict: HIT as contra — a 10-alerts/1000-windows budget (= 1%) sits 3–15× below demonstrated plant tolerance bands (2–15% depending on asset/criticality). A demotion at 10/1000 risks being exactly the A1 artifact: the budget, not plant trust, does the demoting. H3's 5/20-neighbour check does not span the empirically relevant 20–150/1000 range.

### C-H3-2 (HIT — EEMUA rates are about alarm floods, and steady-state compliance is already common)
- ProcessVue/EEMUA-191 summary: "less than one alarm per ten minutes in steady state was very likely to be acceptable, one per five minutes manageable."
- ASM Consortium benchmarking (academia.edu/21918995): "about one-third of the consoles surveyed were able to achieve [1/10 min]… about one-quarter more achieving the manageable level"; only post-upset peak rates (≤10 in first 10 min) remain a broad challenge.
- ABB SCADA white paper: same 1-alarm/10-min steady benchmark; priority split 5/15/80.
- Verdict: HIT — steady-state EEMUA compliance is already achieved by a third+ of consoles; the live problem is upset-peak floods, which H3's fault-free healthy-window replay does not measure. The bar may demote settings on a regime (steady-state) plants already handle while missing the regime (upsets/changeovers) where trust actually breaks — converging with H3-A2 (healthy segments too clean).

### C-H3-3 (MIXED — F1/precision rankings often AGREE, no demotion to observe)
- BDCC 9:128 benchmarking (MVTec-style Industry 4.0 table): Dinomaly/PatchCore/GLASS rank 1/2/3 identically across AUC, Recall, F1 AND AP — "Value Rank" columns agree across all four metrics over 15 classes.
- "Why Ranking Anomaly Detection Algorithms Isn't as…" (arxiv 2608.04613): ranking uncertainty ±0.34 ranks vs only 1.51 average spread across 7 algorithms — rank differences are mostly noise.
- Verdict: HIT as contra — when detectors separate cleanly, sensitivity ranking and trust ranking agree and the tripwire (zero top-quartile breaches) fires trivially; when they don't, rank-reversal is noise, not signal. Either way the predicted ≥1 stable demotion is fragile. (Counter-note: Lacuna SAAM-ALARM paper reports models ranking #1 recall but #6 precision — genuine rank-reversals exist in visual AD; kept as supporting-side, not suppressed.)

### C-H3-4 (HIT — FPR~9% is not a ceiling; production operates far below it)
- arxiv 2601.00005: "target or desired FPR is set to 1%… a 1% FPR is considered acceptable."
- AUPIMO (arxiv 2401.01984): validation FPR restricted to 1e-5–1e-4; "only the range below ~1% FPR is of practical" operation (aicodeinvest guide).
- ICMLA network-AD prototype: "98.5% true and 1.3% false positive rates" deployed at 10–50 Gbps.
- Verdict: HIT — the realistic-ceiling premise (FPR ~9%) is contradicted by systems holding 1% or far lower at high recall; a 9%-tolerant narrative overstates what plants accept in automated/scored deployments, which cuts against H3's motivating contrast but ALSO implies the 10/1000 budget may demote nothing because well-built detectors already clear it — tripwire fires, H3 false.

### C-H3-5 (HIT — threshold-artifact mechanics are real and documented)
- PMLR 151 (Rath et al., minimum-precision constraints): post-hoc threshold search for precision "delivers sub-par results"; optimal BCE classifier capped at 0.68 precision vs 0.9 target — precision must be constrained in training, not bolted on as a bar.
- aicodeinvest guide: "If you can quantify costs, optimize C_FP·FP + C_FN·FN. If neither, maximize F1" — a fixed 10/1000 budget without cost grounding is the arbitrary-threshold construction A1 warns about; proper practice is top-K/fixed-FAR or cost optimisation.
- aiopsschool PR-AUC: "Threshold misconfiguration: deploying a threshold tuned in training without production calibration leading to increased false positives" — demotions move with calibration, not intrinsic setting quality.
- Verdict: HIT — the literature expects demotions from an uncalibrated fixed budget to be unstable across recalibration, which is precisely the failure mode H3's criterion 4 (5/20 check) only partially guards against.

## Falsification verdict: PROVISIONALLY FALSIFIED after 5 searches
No battery run exists to fire the tripwire empirically, but four of five query lines returned convergent contra: the 10/1000 budget is stricter than demonstrated plant tolerance (C-H3-1), measures the already-solved steady-state regime (C-H3-2), faces ranking-agreement/noise base rates (C-H3-3), and sits inside FPR bands good detectors already clear (C-H3-4), with documented threshold-artifact mechanics (C-H3-5). Survives only if the battery shows a demotion that is BOTH present at 10/1000 AND stable across a plant-grounded budget range (recommend synthesis extend the neighbour check to 20–150/1000). Falsification strength: **Provisionally falsified after 5 searches**.
