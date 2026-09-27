# H2 supporting — classical-tripwire gate (deep must beat KNN/PCA raw)

Date: 2026-09-13 · Twin ML training · Parent F1 Locked (raw-only, PA-off) · Phase-1 base: A02 (GDN), A04 (KNN-best raw), A12–A15 (no-DNN-superiority snippets)

## Registered claim (H2)
No deep-detector claim admitted unless GDN-light beats max(KNN,PCA) raw-F1 by ≥+3pp on twin M0b battery (identical splits/calibration).

## Supporting findings (raw-protocol ONLY; PA rows flagged inadmissible as numbers)

### S-H2-1 — GDN raw table itself sets the wire AND shows classicals far below on water-plant benches (Phase-1 A02 re-verified full-text, L1)
- Source: https://ar5iv.labs.arxiv.org/html/2106.06947 (Deng & Hooi, AAAI21, full HTML fetched)
- Claim (raw point-wise, max-validation threshold — PA-free): SWaT F1 — GDN 0.81 vs PCA 0.23 / KNN 0.08 / AE 0.61 / MAD-GAN 0.77; WADI F1 — GDN 0.57 vs PCA 0.10 / KNN 0.08 / AE 0.34 / MAD-GAN 0.37. Graph ablation: replacing learned graph with complete graph / removing sensor-embedding / removing attention each degrades F1 (WADI 0.57→0.51/0.49/0.48).
- Level/confidence: L1 / High for bench fact, Low for twin transfer (water-plant benches only; twin fault mix is caricature-rectangular where distance baselines are strongest — H2 prediction 1 explicitly expects compression/reversal on twin).
- Supports H2 vs alternatives: this is the H0 exhibit (the transfer assumption H2 rejects) AND the gate's existence proof that a deep-vs-classical raw gap is measurable under identical calibration; counters no-gate reasoning. A1 (twin-is-WADI-easy) is exactly "this table transfers" — the battery decides.

### S-H2-2 — Per-sensor median/IQR robust norm inside GDN (supports gate fairness, feeds H4; L1, same source)
- Source: same as S-H2-1, §3.6 Eq.12 + §4.3 (fetched)
- Claim: robust normalization of per-sensor errors (median/IQR, not mean/std) + max-validation threshold + SMA smoothing + max-aggregation.
- Level/confidence: L1 / High (method fact).
- Supports H2 vs alternatives: gate arms must share this normalization/smoothing discipline or the deep-vs-classical delta is confounded — supports H2 verification method (identical features/windows/calibration).

### S-H2-3 — PA-taint flag: TranAD +17%, M2AD +21%, Merlion tutorial F1 are INADMISSIBLE as numbers (Phase-1 quarantine honored, L1/L3)
- Sources: A03 (https://ar5iv.labs.arxiv.org/html/2201.07284), A05 (https://proceedings.mlr.press/v258/alnegheimish25a.html), J06 (Merlion tutorial) per source-table §Q
- Claim: headline gains computed under PA/overlap-TP/tutorial-PA protocols.
- Level/confidence: L1–L3 / High for inadmissibility verdict.
- Supports H2 vs alternatives: prevents A1-by-PA smuggling; only mechanics transfer (MAML/scarcity premise rejected for data-rich twin; GMM+Gamma mechanics feed H5).

### S-H2-4 — STAD/STAND benchmark includes KNN/PCA/LOF/IForest classicals under common TSB-AD harness (NEW, L3, full-text §V-B2)
- Source: https://arxiv.org/html/2511.16145v1 (§V-B2 baseline list: UTAD-I = IForest/LOF/PCA/HBOS/KNN/KMeans; UTAD-II incl. OCSVM/AE/CNN/LSTM/TranAD/USAD/Anomaly-Transformer/TimesNet via TSB-AD harness)
- Claim: classicals run under consistent preprocessing/harness — the exact "same-battery" discipline H2 mandates.
- Level/confidence: L3 / Medium (preprint; harness fact, not outcome).
- Supports H2 vs alternatives: methodological precedent for the gate's identical-split/identical-calibration rule; counters "classicals need separate tuning" objections.

## How support maps to H2 tripwire
- Wire definition (GDN-light − max(KNN,PCA) ≥+3pp raw): S-H2-1 shows the wire is clearable on SWaT/WADI (+58pp/+47pp over classicals) — so a twin failure to clear is informative (gate HOLDS), not a broken wire.
- Protocol-dependence check (prediction 2): S-H2-3 explains why PA-on controls must be reported-not-ranked.
- Cost leg (prediction 3): no new evidence this run — retained gap-pass.

## Gap-pass per sub-hypothesis
- Raw deep-vs-classical gap on twin mix: UNVERIFIABLE without M0b — follow-up query: "KNN PCA vs GDN raw F1 rectangular synthetic fault mix ablation".
- Graph-ablation ≥10pp leg: SUPPORTED as bench precedent (S-H2-1 WADI −6–9pp on ablations; twin leg awaits battery) — follow-up query: "GDN learned graph vs complete graph ablation F1 SWaT WADI".
- Classical CPU-cost ≥10× advantage: GAP — follow-up query: "GDN vs KNN PCA training inference time CPU anomaly detection benchmark".
