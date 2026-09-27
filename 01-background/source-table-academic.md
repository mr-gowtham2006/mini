# Source Table — Academic angle (Verdandi anomaly-twin-trace)

Date: 2026-09-11. Levels: L1 systematic review/meta or top-tier peer-reviewed · L2 peer-reviewed/preprint with data · L3 workshop/thesis/repo-benchmark · L4 tech report · L5 opinion. Academic skew L1–L3.

| # | Source (URL) | Type | Level | Key claim | Limitation / threat |
|---|---|---|---|---|---|
| S1 | Runge et al. 2019, PCMCI, Science Advances — https://www.science.org/doi/10.1126/sciadv.aau4996 | journal | L1 | PCMCI+MCI controls autocorrelation FPs; tigramite reference implementation | Assumes stationarity, no confounders, faithfulness |
| S2 | Runge 2020, PCMCI+, UAI/PMLR — https://proceedings.mlr.press/v124/runge20a/runge20a.pdf | conf | L1 | Contemporaneous+lagged discovery; higher adjacency power | Oracle-case consistency only; finite-sample calibration varies |
| S3 | Debeire et al. 2024, Bagged-PCMCI+, PMLR — https://proceedings.mlr.press/v236/debeire24a/debeire24a.pdf | conf | L2 | Bootstrap majority vote improves precision AND recall + link confidence | Extra compute; bootstrap count is a new tuning knob |
| S4 | PCMCI+ robustness, hydrological, AIES 2025 — https://journals.ametsoc.org/view/journals/aies/4/4/AIES-D-24-0114.1.xml | journal | L2 | Output graphs vary across realizations/sample sizes; robustness must be measured | Domain-specific (catchments); no factory data |
| S5 | Sandia benchmarking PCMCI spatiotemporal, OSTI 1991387 — https://doi.org/10.2172/1991387 | report | L2 | Names temporally-induced spurious causality as the FP source MCI prunes | Lab benchmark; parameter grids may not transfer |
| S6 | Krich et al. 2020, PCMCI biosphere–atmosphere, Biogeosciences — https://bg.copernicus.org/articles/17/1033/2020/ | journal | L2 | Contemporaneous drivers cause spurious lagged+contemporaneous links PCMCI can't fix | Different domain; sub-sampling-timescale issue |
| S7 | Deng & Hooi 2021, GDN, AAAI — https://doi.org/10.1609/aaai.v35i5.16523 | conf | L1 | Learned sensor graph + attention forecasting; F1 ~0.81/0.57-range on own protocol | Own protocol (likely PA-era); learned graph = drift risk |
| S8 | Kim et al., point-adjustment critique, arXiv:2109.05257 — https://arxiv.org/pdf/2109.05257 | preprint | L1 | Random scores become SOTA under PA; PA overestimates | Preprint; theory + empirical but fixed dataset set |
| S9 | Schmidl et al., "Fancy Algorithms and Flawed Evaluation", 2023 — https://doi.org/10.48550/arxiv.2308.13068 | preprint | L2 | Point-wise F1 ~0.43 achievable by all-anomalous prediction; use composite/event-wise | Preprint; re-analysis, no new method |
| S10 | "Quo Vadis, Unsupervised TSAD?" position 2024 — https://arxiv.org/html/2405.02678 | preprint | L2 | PA + flawed protocols = main pitfall of the field | Position paper; selective evidence |
| S11 | Tuli et al. 2022, TranAD, VLDB — https://vldb.org/pvldb/vol15/p1201-tuli.pdf | journal | L1 | Transformer detector; +17% F1 over SOTA on own protocol | PA-era numbers; WADI still weak for all models |
| S12 | Audibert et al. 2020, USAD, KDD — https://doi.org/10.1145/3394486.3403392 | conf | L1 | Adversarially-trained AE, fast+stable; SOTA on own protocol | PA-era; raw-protocol WADI F1 only ~0.31 (SOTA2) |
| S13 | Su et al. 2019, OmniAnomaly, KDD — https://doi.org/10.1145/3292500.3330672 | conf | L1 | Stochastic RNN + reconstruction probability; F1 0.86 on 3 datasets | PA-era; interpretation accuracy ≤0.89 |
| S14 | TCN-GAT SWaT negative result, IEEE ICCI 2026 — https://doi.org/10.1109/icci68752.2026.11506458 | conf | L2 | 0.886→0.281 under fixed-percentile calibration; 53× score-distribution shift | Single model/dataset; negative result (strength here) |
| S15 | Pham et al., RCAEval, arXiv:2412.17015 / ICSE — https://doi.org/10.48550/arxiv.2412.17015 | preprint+conf | L1 | 735 failures, 15 baselines, AC@k/Avg@k; best Avg@5 0.46–0.54; DELAY/LOSS death | Microservices telemetry, not factory machines |
| S16 | Pham et al. 2024, BARO, FSE (Best Artifact) — https://doi.org/10.1145/3660805 | conf | L1 | Graph-free, nonparametric end-to-end RCA robust to noisy detection | Needs pre/post-failure windows; no causal graph output |
| S17 | Liu et al. 2022, CIRCA, CBN+intervention — https://ar5iv.labs.arxiv.org/html/2206.05871 | preprint | L2 | Regression hypothesis testing + descendant-adjusted scores | Needs architecture-derived parents; incomplete-data assumptions |
| S18 | TAMO, TSC 2025 — https://doi.org/10.1109/tsc.2025.3629066 | journal | L1 | Tool-assisted LLM agent (alignment+localization tools) for fine-grained RCA | Cloud-native; compute cost of agent loop |
| S19 | KRCA, 2026 — https://arxiv.org/html/2607.01788v3 | preprint | L2 | Progressive narrowing; 0.88/0.79, −77.3% time | Hyper-scale microservices; preprint, single-org eval |
| S20 | OpenRCA, ICLR 2025 — https://github.com/microsoft/OpenRCA | conf+repo | L1 | 68GB telemetry; Claude 3.5 RCA-agent solves only simplest cases | Synthetic benchmark; outcome labels only (v1) |
| S21 | OpenRCA 2.0, 2026 — https://arxiv.org/html/2606.27154v2 | preprint | L2 | Process labels via intervention asymmetry; 38.5% ungrounded baseline | Annotator sees injections the agent never sees (noted asymmetry) |
| S22 | Kim 2026, process-level RCA-agent failure analysis — https://arxiv.org/html/2602.09937v2 | preprint | L2 | 12 pitfall types over 1,675 runs; outcome-only eval hides reasoning failure | Tied to OpenRCA benchmark scope |
| S23 | Causal Digital Twins for CPS security, 2025 — https://arxiv.org/html/2510.09616v1 | preprint | L2 | 0.90–0.94 F1 SWaT/WADI/HAI; 78.4% RCA accuracy; −74% FPs | Verify PA-vs-raw protocol before citing as support |
| S24 | Digital-twin co-simulation training BNs, Sensors 2022 — https://doi.org/10.3390/s22041430 | journal | L2 | DT fault injection trains equipment+factory-level diagnostic BNs | DES-modeling effort; expertise-gated |
| S25 | causRCA machinery dataset, Zenodo 2025 — https://zenodo.org/records/15876410 | dataset | L2 | Hardware-in-the-loop twin with injected valve-leak/filter-clog ground truth | Machinery-specific; label schema may not map to Verdandi |
| S26 | GALA graph-augmented LLM RCA, 2026 — https://arxiv.org/abs/2608.08968 | preprint | L3 | Graph+trace scoring + SURE-Score human eval; +25% over LLM baselines | New metric (SURE-Score); two-benchmark eval only |

Snowball note: S12/S13/S16/S17 reached via snowball of top-3 (GDN baselines; PA-critique lineage; RCAEval baselines) and directly retrieved in Q11–Q12. No invented sources; every row above was returned by the search tools this session.
