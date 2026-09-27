# H2 Supporting — Raw-F1-only detector bar (PA invalid, raw SOTA collapse)

Date: 2026-09-11 · Hypothesis: H2-raw-f1-detector-bar.md

## Queries executed (relevance gate: each directly targets H2)
1. academic: "point adjustment invalid time series anomaly detection F1 Kim" — KEPT
2. academic: "Schmidl raw F1 time series anomaly detection benchmark point-wise" — KEPT
3. tech: "TranAD USAD MTAD-GAT WADI SWaT F1 score anomaly detection" — KEPT
- Dropped/rewritten: none.

## Sources (cascade: search-cache → direct-URL → scholar → web)
| # | Source URL | Claim | Level | Confidence | Cascade verdict | Supports H2 how |
|---|---|---|---|---|---|---|
| S1 | https://arxiv.org/pdf/2109.05257 (Kim et al., AAAI 2022 "Towards a Rigorous Evaluation of Time-Series Anomaly Detection") | PA protocol overestimates performance: even random anomaly scores become SOTA under PA-F1 | L2→L1 (arXiv 2109.05257 → AAAI 2022, doi via ojs.aaai.org/index.php/AAAI/article/view/20680) | High | cache=HIT; direct-URL=skipped/unverifiable; scholar=not-queried | Core support for H2 prediction 3 (PA-on flips ranking): PA is proven invalid → raw point-wise F1 bar is necessary, not optional. |
| S2 | https://www.sota2.com/research/sota/anomaly-detection-on-wadi | Raw-protocol WADI F1: TranAD 49.51, MTAD-GAT 41.69, USAD 30.56 (all <0.60) | L4 (benchmark leaderboard) | Medium-High | cache=HIT; direct-URL=skipped/unverifiable | Direct numeric support for H2 tripwire framing (GNN raw-F1 ≤0.50 band; TranAD 49.5 / MTAD-GAT 41.7 / USAD 30.6 match registry's cited values). |
| S3 | https://www.vldb.org/pvldb/vol15/p1779-wenig.pdf + https://timeeval.github.io/evaluation-paper/ (Schmidl/Wenig/Papenbrock, VLDB 2022 "Anomaly Detection in Time Series: A Comprehensive Evaluation") | Large-scale evaluation shows many detectors score well only under biased/trivial setups; ranking is protocol-sensitive | L1 (PVLDB 15(9)) | High | cache=HIT; direct-URL=skipped/unverifiable | Supports H2 prediction 1 (collapse gap ≥0.30 under raw calibration): independent large benchmark confirming protocol decides the winner. |
| S4 | https://proceedings.neurips.cc/paper_files/paper/2024/file/c3f3c690b7a99fba16d0efd35cb83b2c-Paper-Datasets_and_Benchmarks_Track.pdf ("The Elephant in the Room", NeurIPS 2024 D&B) | Employs point-wise Standard-F1 / AUC-ROC / AUC-PR alongside PA-F1 (kept "for completeness" as imperfect); event-based F1 to counter PA bias | L1 (NeurIPS 2024) | Medium-High | cache=HIT; direct-URL=skipped/unverifiable | Supports H2's raw point-wise F1 choice as community-standard direction post-PA. |

## Field-level citation check
| # | Title | Authors | Venue | Year | DOI/URL | Label | Match/Mismatch |
|---|---|---|---|---|---|---|---|
| S1 | Towards a Rigorous Evaluation of Time-Series Anomaly Detection | Kim et al. | AAAI (arXiv:2109.05257) | 2021/2022 | arxiv:2109.05257; AAAI doi link above | R (peer-reviewed AAAI) | Match |
| S2 | Anomaly Detection on WADI benchmark leaderboard | SOTA2 Research (curators) | sota2.com leaderboard | 2022–2025 rolling | sota2.com URL above | H (hosted leaderboard) | Match (values as displayed; protocol-dependence noted — TranAD 2023.08 row differs by protocol) |
| S3 | Anomaly Detection in Time Series: A Comprehensive Evaluation | Schmidl, Wenig, Papenbrock | PVLDB 15(9) | 2022 | vldb.org + timeeval page | R | Match |
| S4 | The Elephant in the Room: Towards A Reliable Time-Series Anomaly Detection Benchmark | (authors not extracted) | NeurIPS 2024 Datasets & Benchmarks | 2024 | neurips.cc URL above | R | Partial — authors unverified; flagged |

Hallucinated (H) sources removed: none — all URLs verbatim from search; no invented citations.
