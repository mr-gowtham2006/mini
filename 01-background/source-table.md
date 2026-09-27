# Source Table (Merged Canonical) — Verdandi anomaly-twin-trace

Date: 2026-09-11 · Merger: lead fallback (angle tables verified, merger stalled) · Flow: 180 retrieved → 177 screened → 89 eligible → 47 included.
Levels: L1 top-tier peer-reviewed/meta · L2 peer-reviewed/preprint/official-docs/OSS-benchmark · L3 reputable press/analyst/vendor-guide · L4 practitioner grey lit · L5 opinion. Dedup: RCAEval appears as A-S15 + I-S22 (counted once → 47 unique).

## Academic sources (A-S1–A-S26, from source-table-academic.md)

| # | Source | Type | Level | Key claim | Limitation |
|---|---|---|---|---|---|
| A-S1 | Runge et al. 2019 PCMCI, Sci Adv — science.org/doi/10.1126/sciadv.aau4996 | journal | L1 | PCMCI+MCI controls autocorrelation FPs; tigramite ref impl | Stationarity, no-confounders, faithfulness assumed |
| A-S2 | Runge 2020 PCMCI+, UAI/PMLR | conf | L1 | Contemporaneous+lagged discovery; higher adjacency power | Oracle-case consistency only |
| A-S3 | Debeire et al. 2024 Bagged-PCMCI+, PMLR | conf | L2 | Bootstrap vote improves precision+recall + link confidence | Bootstrap count is new knob; extra compute |
| A-S4 | PCMCI+ robustness hydrological AIES 2025 | journal | L2 | Graphs vary across realizations/sample sizes | Catchment domain; no factory data |
| A-S5 | Sandia PCMCI benchmark OSTI 1991387 | report | L2 | Temporally-induced spurious causality documented | Lab benchmark; params may not transfer |
| A-S6 | Krich et al. 2020 Biogeosciences | journal | L2 | Contemporaneous drivers cause spurious links PCMCI can't fix | Different domain; timescale issue |
| A-S7 | Deng & Hooi 2021 GDN, AAAI | conf | L1 | Learned sensor graph + attention; F1 ~0.81/0.57 own protocol | Own (likely PA-era) protocol; drift risk |
| A-S8 | Kim et al. PA critique arXiv:2109.05257 | preprint | L1 | Random scores become SOTA under PA | Preprint; fixed dataset set |
| A-S9 | Schmidl et al. 2023 flawed-eval | preprint | L2 | All-anomalous ≈0.43 point-wise F1; use composite/event-wise | Re-analysis, no new method |
| A-S10 | Quo Vadis TSAD position 2024 | preprint | L2 | PA + flawed protocols = main pitfall | Position paper; selective |
| A-S11 | Tuli et al. 2022 TranAD, VLDB | journal | L1 | Transformer detector; +17% own-protocol | PA-era; WADI weak for all |
| A-S12 | Audibert et al. 2020 USAD, KDD | conf | L1 | Adversarial AE, fast+stable | PA-era; raw WADI ≈0.31 |
| A-S13 | Su et al. 2019 OmniAnomaly, KDD | conf | L1 | Stochastic RNN + recon probability; F1 0.86 (3 ds) | PA-era; interp ≤0.89 |
| A-S14 | TCN-GAT SWaT negative, IEEE ICCI 2026 | conf | L2 | 0.886→0.281 fixed-percentile; 53× shift | Single model/dataset (strength as negative) |
| A-S15 | Pham et al. RCAEval arXiv:2412.17015/ICSE (=I-S22) | preprint+conf | L1 | 735 failures, 15 baselines, AC@k; best Avg@5 0.46–0.54; DELAY/LOSS death | Microservice telemetry, not factory |
| A-S16 | BARO FSE 2024 Best Artifact | conf | L1 | Graph-free nonparametric end-to-end RCA | Needs pre/post windows; no causal graph |
| A-S17 | CIRCA CBN+intervention 2022 | preprint | L2 | Regression hypothesis testing + descendant-adjusted scores | Needs architecture parents |
| A-S18 | TAMO TSC 2025 | journal | L1 | Tool-assisted LLM agent for fine-grained RCA | Cloud-native; agent-loop cost |
| A-S19 | KRCA 2026 arXiv:2607.01788 | preprint | L2 | Progressive narrowing; 0.88/0.79, −77.3% time | Hyper-scale; preprint, single-org |
| A-S20 | OpenRCA ICLR 2025 (microsoft/OpenRCA) | conf+repo | L1 | 68GB telemetry; Claude 3.5 solves simplest only | Synthetic; outcome labels only (v1) |
| A-S21 | OpenRCA 2.0 2026 arXiv:2606.27154 | preprint | L2 | Process labels via intervention asymmetry; 38.5% ungrounded | Annotator/agent information asymmetry |
| A-S22 | Kim 2026 RCA-agent failure analysis arXiv:2602.09937 | preprint | L2 | 12 pitfall types / 1,675 runs; outcome-only eval hides failure | Tied to OpenRCA scope |
| A-S23 | Causal DT CPS 2025 arXiv:2510.09616 | preprint | L2 | 0.90–0.94 F1 SWaT/WADI/HAI; 78.4% RCA; −74% FP | Verify PA-vs-raw before citing as support |
| A-S24 | DT co-simulation BN Sensors 2022 | journal | L2 | DT fault injection trains equipment+factory BNs | DES effort; expertise-gated |
| A-S25 | causRCA machinery dataset Zenodo 2025 | dataset | L2 | HIL twin, valve-leak/filter-clog truth | Machinery-specific schema |
| A-S26 | GALA graph-augmented LLM RCA 2026 | preprint | L3 | Graph+trace scoring + SURE-Score; +25% over LLM baselines | New metric; 2-benchmark eval |

## Industry sources (I-S01–I-S22, from source-table-industry.md)

| # | Source | Type | Level | Key claim | Limitation |
|---|---|---|---|---|---|
| I-S01 | salesforce/PyRCA (GitHub) | OSS repo+docs | L2 | Metric-RCA lib; walk patterns Verdandi reuses capped | IT scope; no narration/provenance/replay |
| I-S02 | Coralogix 10-tool RCA taxonomy | vendor guide | L3 | Observability vs AIOps-overlay vs dedicated RCA map | Vendor-authored; coarse, no MTTR deltas |
| I-S03 | Merlion official docs | OSS docs | L2 | Unified TS anomaly/forecast/change-point + calibration/AutoML | Archived trajectory; glue-idea only |
| I-S04 | PyOD benchmark docs (ADBench/TSB-AD) | OSS benchmark | L2 | 30–40 algos, 57–1070 datasets backing | Detection only; no ranking/replay |
| I-S05 | FactorySimPy docs | OSS docs | L2 | SimPy-4 manufacturing DES graph components | v0.1.0b3 API-unfit (ADR-0012); sim-only |
| I-S06 | Guideflow AIOps tools 2026 | market review | L3 | Datadog+Dynatrace top platform; BigPanda correlator; PagerDuty=response | Affiliate-style; no primary measures |
| I-S07 | Axiometrica AIOps compared 2026 | analyst blog | L4 | Davis precise chains; G2 40% productivity/90% MTTR (large deploys) | G2-sourced; silent on failures; lock-in unpriced |
| I-S08 | Techplained AIOps 2026 | explainer | L4 | PagerDuty orchestrates, does NOT do RCA; 50→3 compression | Secondary; positioning may shift |
| I-S09 | Teeptrak downtime cost 2026 (Siemens/ABB-sourced) | industry analysis | L3 | ~$125–260k/hr; ~$2.3M/hr auto; ~$1.4T/yr top-500; PdM −50%, ~3-mo payback | Vendor-study recycling; upper-bound |
| I-S10 | Oxmaint maintenance benchmark 2026 | benchmark report | L3 | 60% vs 80%+ OEE; PdM market 22–34%/yr; −70–90% downtime | Vendor-hosted; uncontrolled ceiling |
| I-S11 | Siemens Digital Twin Composer CES 2026 | vendor press | L2 | Xcelerator photorealistic twin + MES/QMS/PLC/IIoT; marketplace mid-2026 | Announcement; no independent ROI |
| I-S12 | ResearchIntelo industrial twin market | market report | L3 | Siemens DI SW ~€6.8B FY24; Altair acquisition deepens twin | Paywalled; revenue ≠ ROI proof |
| I-S13 | ICIS 2023 IIoT graveyard | peer-reviewed | L1 | 1st wave failed ~2018 (Predix); 2nd wave exits underway | Practitioner track; explanatory |
| I-S14 | PlatformEngineering Predix postmortem | practitioner | L4 | $7B burn: horizontal overreach, upkeep denial | Single-author; $ figure varies $4–7B |
| I-S15 | Datafield Uptake/C3 case study | case study | L4 | Focused startups beat GE on deployability/price | Educational; Uptake itself stalled |
| I-S16 | Traversal AI incident-response 2026 | industry data | L4 | Confident-wrong AI in P1s costs more than silence; fatigue persists | Vendor blog; primary data not reproduced |
| I-S17 | Johal 2026 AI-agents-DevOps opinion | opinion+bench | L5 | 112 incidents/37 orgs: AI MTTR +41% vs runbooks; misdiag 22% vs 3% | Self-reported, unaudited — contradiction anchor, discounted |
| I-S18 | NewStack AIOps critique (2020) | press critique | L3 | Triple promise "largely intractable" | 2020 vintage; polemical anchor |
| I-S19 | Forbes Tech Council twins-failed 2026 | press | L3 | Twins failed on decision/human web, not sensors/compute | Paid channel; anecdotal |
| I-S20 | Snatika reality-gap 2026 | analysis | L4 | Sim +15% → real −2% illustration | Marketing-adjacent; single example |
| I-S21 | IoTDT fact-check production 2026 | fact-check | L4 | ROI back-loaded/conditional; ignored twin returns nothing | Niche outlet; convergent signal w/ S19–S20 |
| I-S22 | phamquiluan/RCAEval (=A-S15) | OSS benchmark | L1/L2 | 735 cases adopted by NOFire/MetaRCA/rca-llm | IT domain; manufacturing transfer unproven |

No invented sources — all rows trace to angle tables written from engine-returned URLs 2026-09-11.
