# Contradicting evidence — H2 (raw-F1 detector bar: GNN ≤0.50, quantile ≈0.73)

Date: 2026-09-11 · Agent: falsification-agent · Target: H2-raw-f1-detector-bar.md
Tripwire: falsified by any GNN raw-F1 ≥0.60 OR quantile raw-F1 <0.60 on same battery.

## Queries (5, cascade academic→tech→web)
1. (academic) "graph neural network time series anomaly detection F1 score benchmark state of the art"
2. (academic) "GDN MTAD-GAT multivariate anomaly detection F1 score SMD SMAP MSL benchmark"
3. (tech) "GNN anomaly detection high F1 score outperforms statistical baseline GitHub benchmark"
4. (academic) "point adjustment overestimates time series anomaly detection F1 without adjustment deep learning fails"
5. (web) "GDN graph neural network anomaly detection F1 SMD SWaT raw point-wise score"

## Relevance gate
Kept sources with dataset-named F1 numbers (SMD/SMAP/MSL/SWaT/WADI) or evaluation-protocol analysis (point adjustment). Dropped generic surveys without numbers and graph-anomaly (node-classification) leaderboards (different task: Anomaly-F1 on Computers/N2NSC).

## Findings (strongest contradicting first)
- C-H2-1 (DIRECT counter, raw point-wise): Deng & Hooi, GDN (AAAI 2021), Table 2 — GDN F1 = 0.81 SWaT / 0.57 WADI, precision 99.35/97.50, computed point-wise over test ground truth. SWaT 0.81 ≥ 0.60 CONTRADICTS universal "GNN ≤0.50 raw" on a real water-treatment testbed. (WADI 0.57 <0.60 supports H2 — recorded.)
- C-H2-2 (DIRECT counter, widely cited): Zhao et al., MTAD-GAT (ICDM 2020) — F1 0.9013 SMAP / 0.9084 MSL / 0.7975 TSA; LUT replication 0.9100 SMD / 0.9134 MSL / 0.9239 SMAP. All ≥0.60 by large margins. Caveat: uses point adjustment (see C-H2-4).
- C-H2-3 (counter, independent): GTAD (PMC 2022) — F1 0.9539 MSL, 0.9363-class rivals; GDN 0.9591 MSL in same table. Again PA-protocol (caveat applies).
- C-H2-4 (supports H2's mechanism): Kim et al., "Towards a Rigorous Evaluation of Time-Series Anomaly Detection" (AAAI) + follow-ups — PA protocol "overestimates detection performance; even a random anomaly score can easily turn into a SOTA TAD method." This DEFUSES C-H2-2/C-H2-3 (their numbers are PA-inflated) but does NOT defuse C-H2-1 if GDN's Table 2 is raw point-wise (predates PA convention; computed over full test ground truth).
- C-H2-5 (mixed): MTAD benchmark (OpsPAI/MTAD, arXiv 2401.06175) — KNN beats deep models on raw F1-hat for SMD/SMAP/MSL (supports H2's skepticism), but deep models lead post-PA (shows metric-dependence, not GNN incapacity).

## Gap-pass
No retrieved study runs GNN-vs-quantile raw point-wise F1 on the twin's 20–32 fault battery (M0b owed). Dataset transfer (SWaT/WADI/SMD → factory twin) is the open gap.

## Field-level citation check (R/P/H)
| ID | Reported number | Precise (raw point-wise F1?) | Hits tripwire? |
|---|---|---|---|
| C-H2-1 | GDN 0.81 SWaT / 0.57 WADI | Likely yes (point-wise over GT) | YES on SWaT (≥0.60) |
| C-H2-2 | MTAD-GAT 0.80–0.92 | No (point-adjusted) | No (protocol mismatch) |
| C-H2-3 | GTAD 0.95 MSL | No (point-adjusted) | No (protocol mismatch) |
| C-H2-4 | random→SOTA under PA | Protocol critique, no F1 | No (supports H2) |
| C-H2-5 | KNN best raw, DL best PA-adjusted | Partial | No |

## Verdict: PROVISIONALLY FALSIFIED
C-H2-1 trips the "GNN raw-F1 ≥0.60" wire on SWaT (0.81) if GDN's Table 2 counts as raw point-wise — the universality of "GNN ≤0.50" does not survive contact with the literature. Downgraded from Strongly because: (a) WADI 0.57 conforms to H2, (b) PA-inflation (C-H2-4) clouds C-H2-2/3, (c) no twin-battery run exists. Recommendation: scope H2 to the twin battery explicitly ("on M0b battery", not universal) and pre-register PA-off scoring, or concede the bar to "GNN ≤0.60 on WADI-class data".
