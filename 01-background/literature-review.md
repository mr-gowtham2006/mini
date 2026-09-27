# Literature Review (Canonical Merge) — Verdandi anomaly-twin-trace

Date: 2026-09-11 · Merger: synthesis-merger · Sources: review-academic.md + review-industry.md (both 2026-09-11)
Topic: 32-machine factory twin; topology-constrained PCMCI + quantile detector + template+verifier narration + seeded replay.

## 0. PRISMA search-strategy table (per angle, verbatim queries)

### Academic angle (12 queries, academic/tech engines)

| Angle | Database/connector | Full query verbatim | Limits | N retrieved→screened→kept |
|---|---|---|---|---|
| Academic | academic | PCMCI causal discovery time series stability short samples tigramite | Top-8, relevance | 8→8→4 |
| Academic | academic | GDN graph neural network anomaly detection multivariate time series F1 | Top-8, relevance | 8→8→2 |
| Academic | academic | "point-adjusted" F1 overestimates anomaly detection evaluation critique (†) | Top-8, relevance | 8→7→4 |
| Academic | academic | PCMCI spurious edges false positive autocorrelation critique (†) | Top-8, relevance | 8→8→4 |
| Academic | academic | RCAEval benchmark root cause analysis microservices AC@K | Top-8, relevance | 8→8→3 |
| Academic | academic | LLM incident root cause analysis grounding microservices TAMO KRCA | Top-8, relevance | 8→8→4 |
| Academic | academic | causal digital twin industrial control system simulation fault injection | Top-8, relevance | 8→8→4 |
| Academic | academic | SWaT WADI anomaly detection benchmark TranAD USAD MTAD-GAT raw F1 | Top-8, relevance | 8→8→3 |
| Academic | tech | GNN anomaly detection fails generalize illusion progress simple baseline outperforms (†) | Top-8, relevance | 8→8→4 |
| Academic | tech | OpenRCA LLM root cause hallucination ungrounded incident analysis (†) | Top-8, relevance | 8→6→4 |
| Academic | academic | USAD unsupervised anomaly detection multivariate time series autoencoder OmniAnomaly stochastic (snowball of top-3) | Snowball, top-6 | 6→6→2 |
| Academic | academic | BARO CIRCA Bayesian metric-based root cause analysis microservices robustness (snowball of top-3) | Snowball, top-6 | 6→6→2 |

### Industry angle (11 queries, tech/web engines)

| Angle | Database/connector | Full query verbatim | Limits | N retrieved→screened→kept |
|---|---|---|---|---|
| Industry | tech | PyRCA root cause analysis library vs commercial RCA tools comparison | Top-8, relevance | 8→8→4 |
| Industry | tech | FactorySimPy manufacturing simulation digital twin Python discrete event | Top-8, relevance | 8→8→3 |
| Industry | tech | Salesforce Merlion time series anomaly detection toolkit features | Top-8, relevance | 8→8→3 |
| Industry | tech | GDN USAD PyOD deep anomaly detection open source benchmark comparison | Top-8, relevance | 8→8→3 |
| Industry | web | root cause analysis AIOps market 2026 Datadog Dynatrace Splunk PagerDuty | Top-8, relevance | 8→8→6 |
| Industry | web | factory predictive maintenance downtime cost per hour MTTR manufacturing 2025 2026 | Top-8, relevance | 8→8→5 |
| Industry | web | digital twin manufacturing market Siemens MindSphere platform 2026 | Top-8, relevance | 8→8→5 |
| Industry | web | GE Predix failure postmortem Uptake industrial IoT platform shutdown lessons | Top-8, relevance | 8→8→5 |
| Industry | web (contra) | AIOps does not reduce MTTR criticism alert fatigue evidence failure | Top-8, relevance | 8→8→5 |
| Industry | web (contra) | digital twin ROI failure criticism manufacturing simulation reality gap | Top-8, relevance | 8→8→6 |
| Industry | tech | RCAEval benchmark LLM root cause analysis incident RCAEval adopters | Top-8, relevance | 8→8→4 |

(†) = contradiction-seeking query. Academic: 4/12 contradiction-seeking. Industry: 2/11 contradiction-seeking.

### Inclusion / exclusion criteria (with reasons)

Include: (a) directly bears on Verdandi's four claims (topology-constrained causal discovery, raw-F1 detector evaluation, RCA ranking AC@K, grounded narration) or market/competitor positioning; (b) contradiction-duty sources retained even when low-level (e.g. L5 opinion kept as contradiction anchor, weight discounted); (c) OSS repos/docs only when load-bearing for a build-vs-buy decision (PyRCA, FactorySimPy, Merlion, RCAEval).
Exclude: vendor listicles without measurable claims; dead links; pure code mirrors without docs; duplicate URLs across queries (counted once); PA-only detector numbers cited as support (admissible only as collapse evidence).

### Flow-count line

Flow: 180 retrieved → 177 screened → 89 eligible → 47 included (3 excluded at screening: 1 off-topic abstract [Q3], 2 dead-link/code-mirror drops [Q10]; 88 excluded at eligibility: off-topic, duplicate URLs across queries, vendor listicles without claims, PA-only numbers unusable as support).

## 1. Thematic synthesis (both angles kept)

### Academic T1. Causal discovery: PCMCI works only inside its contract
PCMCI/PCMCI+ (Runge 2019/2020) beats naive CI under autocorrelation via the MCI test, but assumes stationarity, no hidden confounders, faithfulness. Short samples + strong autocorrelation degrade power/calibration (AIES 2025; Sandia OSTI 1991387; Krich 2020). Bagged-PCMCI+ (Debeire 2024) stabilizes via bootstrap majority vote — supports Verdandi's flip-gate/seed-sweep (tau=2, n≥800). Verdict: topology-constrained PCMCI + flip gate defensible; blind discovery on 32 nodes is not.

### Academic T2. Detector evaluation: reported SOTA collapses under raw protocol
Point-adjusted F1 overestimates — random scores become SOTA under PA (Kim arXiv:2109.05257); all-anomalous prediction reaches ~0.43 point-wise F1 (Schmidl 2023). TCN-GAT SWaT 0.886→0.281 under fixed-percentile calibration (IEEE ICCI 2026). Raw-protocol SOTA2/WADI: TranAD 49.5, MTAD-GAT 41.7, USAD 30.6. GNNs lose to simple baselines, OOM at scale, fail cross-dataset transfer. Verdict: per-machine quantile calibration + raw-F1-only admissibility REQUIRED; learned-graph detector correctly deprioritized.

### Academic T3. RCA benchmarks: AC@K ceiling is modest
RCAEval (735 failures, 15 baselines): best Avg@5 ≈ 0.46–0.54; DELAY/LOSS near-death for all. BARO (graph-free, FSE 2024 Best Artifact) is the strongest dishonest baseline vs Verdandi's topology-prior claim; CIRCA needs architecture-derived parents. Verdict: AC@1 ≥70% on 20–32 injected faults defensible ONLY as sim-ground-truth result, never as general claim.

### Academic T4. LLM-RCA: tools + constraints beat open narration
TAMO (TSC 2025) and KRCA (2026, 0.88/0.79, −77.3% time) show grounded tool-using agents cut MTTR. OpenRCA/OpenRCA 2.0: ungrounded accuracy ~38.5%; 12 pitfall types over 1,675 runs. Verdict: template + verifier + fallback (provenance IDs mandatory) is the correct posture; template-level 1.00 PROVEN, full-LLM open.

### Academic T5. Twin simulation: DES + fault injection is established
Co-simulation DT training diagnostic BNs (Sensors 2022); hardware-in-the-loop causRCA dataset (Zenodo 2025); VALOR/DT-based IDS with 23 attack scenarios; causal DTs 0.90–0.94 F1 (verify PA-vs-raw before citing). Verdict: hand-rolled seeded twin with free sim ground truth is standard practice at this scale; FactorySimPy rejection reasonable.

### Industry M1. Who faces the problem + cost framing
Hand-triage (30–60 min/alarm) is the norm; unplanned downtime ~$260k/hr average (~$1.3–2.3M/hr auto; ~$1.4T/yr top-500); PdM vendor claims (−50–75% downtime, ~3-mo payback) are upper-bound marketing. Student/viva inversion: marks replace MTTR dollars, replay hash replaces CMMS audit. Verdandi's honest analog: cut per-alarm hand-triage to ranked cause + replay in seconds — NOT fleet MTTR reduction.

### Industry M2. Market / competitor section
14-entry competitor table (review-industry.md §2): Datadog Watchdog, Dynatrace Davis, Splunk ITSI, PagerDuty AIOps, BigPanda/Moogsoft, Siemens Insights Hub + Twin Composer, Predix (dead lesson), Uptake (stalled), PyRCA, RCAEval ecosystem, FactorySimPy, Merlion, PyOD/ADBench/GDN/USAD, LLM-RCA startups. Net: no direct competitor does ranked upstream trace (≤3 steps) + per-sentence provenance + seeded replay for factory-twin alarms on CPU laptop. Commercial AIOps = IT estates at SaaS prices; Siemens = real plants at enterprise cost; OSS = detectors/benchmarks stopping at ranking. Predix/Uptake graveyard confirms narrow, upkeep-free, sim-owned scope.

### Industry M3. Market factors
SaaS-metered pricing vs Verdandi ~zero marginal cost (non-transferable — holds only absent prod pipeline); local-first topology fits constrained deployment, not cloud-twin mainstream; no safety-restart authority matches industry expectation; topology (P&ID) assumed fixed per semester with drift flip-gates — transfers only where versioned; sim-vs-real gap bounds claims without invalidating the viva artifact.

## 2. Consensus levels (merged)
- STRONG: PA protocol invalid (raw F1 only); PCMCI beats naive CI but needs its contract; RCAEval ceiling ~0.5 Avg@5; tool-grounded LLM-RCA > open narration; hand-triage pain + downtime economics real; no competitor in the semester-local niche.
- MODERATE: GNNs don't transfer; bagging stabilizes PCMCI+; sim-injected faults valid as RCA ground truth; local-first fits constrained segment.
- CONTESTED: exact raw-F1 detector ranking; causal-DT 0.9+ claims (protocol unverified); LLM-RCA % gains across incompatible benchmarks; PdM ROI magnitudes (vendor-hosted).

## 3. Gaps relevant to Verdandi
1. No published topology-constrained PCMCI + flip-gate study at 32-node factory scale with n≈800 seed sweeps — Verdandi's spike is novel support.
2. No per-machine-quantile ablation for TCN-GAT-style collapse — Verdandi's veto-mask+quantile result fills this.
3. Template+verifier provenance rate (1.00@50) has no direct comparator — report as template-level, not LLM-level.
4. Raw-protocol detector comparison on the twin's own 20–32 fault battery still owed (M0b).
5. No brownfield transfer evidence (topology drift, sensor noise, operator-trust curve) — out of semester scope, must stay disclosed.
