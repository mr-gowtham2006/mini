# Verdandi failure-mode map — external evidence only
**Scope:** how systems LIKE Verdandi fail in literature + deployment. Each item: mechanism (2–4 lines) + strongest published evidence (URL + level) + exact Verdandi component/claim endangered. No generic AI-risk essays; numbers only.
**Evidence levels:** L1 = peer-reviewed paper / released dataset+code; L2 = measured benchmark / industry postmortem with named operator + numbers; L3 = vendor/analyst/press synthesis (directional only).
**Verdandi shorthand:** TWIN = 32-machine SimPy twin (topology-A ~22), CPU <10min demo; TRACE = ranked causal trace (PCMCI + walk); NARR = grounded LLM narration; REPLAY = seeded replay; ACT = correction agent (closed A1–A7 + simulate-before-act + masked-PPO RL); BENCH = single-synthetic-domain benchmark ambitions.

---

## 1) Digital-twin deployment failures

**F1 — Platform overreach: horizontal twin-platform built before any single vertical is excellent.**
Mechanism: one PaaS tries to cover aviation + power + healthcare + transport data models, safety cases, and cadences at once; engineering spreads thin, integration with legacy OT stalls, internal divisions refuse to adopt, revenue misses by ~10x. This is the Predix shape: $4B (up to $7B incl. consulting/hiring per Wikipedia) spent 2011–2019, $15B 2020 software-revenue target → ~$1–1.2B actual, stock ~$30 (2016) → <$7 (2018), Predix name retired 2022, GE Digital absorbed into Vernova 2023.
Evidence (L2): https://meltingspot.io/en/blog/general-electric-predix-digital-transformation-failure-4-billion ; https://en.wikipedia.org/wiki/Predix_(software) ; https://www.klover.ai/ge-ai-strategy-industrial-ai-dominance-from-ashes-of-predix/ ; https://datafield.dev/ai-ml-for-business/part-06/chapter-31/case-study-02.html
Endangers: **TWIN + BENCH claim "one twin covers the line."** Verdandi is single synthetic domain today; any slide toward "general manufacturing twin" without a vertical ROI (downtime avoided on topology-A) repeats Predix resource-dilution + customer-confusion rows.

**F2 — Pilot purgatory / OT-IT integration wall (Uptake arc).**
Mechanism: PoC on clean historian export looks good; scaling to hundreds of heterogeneous assets hits siloed/proprietary OT protocols, long sales cycles, and buyers who cannot tie predictions to P&L. Uptake hit unicorn $2.3B (Series D $117M, 2017, Caterpillar/Berkshire/US-Army logos, ~$100M ARR ~2020) then stalled and was acquired by Bosch Mar 2026 on confidential (below-peak) terms, refocused on fleet/telematics where the parent owns the hardware.
Evidence (L2/L3): postmortem synthesis via web search 2026 (Uptake→Bosch acquisition reporting; valuation-tracker pages).
Endangers: **TWIN adoption claim.** A SimPy twin with no OT-connector story (OPC-UA/SCADA/PLC tag mapping, historian-gap handling) is a dashboard, not a deployment path. Measure: time from plant historian export → running twin, not demo runtime.

**F3 — Sim2real gap quantified: near-perfect in sim, ~0.25 in real; small real calibration sets recover most of it.**
Mechanism: synthetic-trained detectors/controllers exploit simulator texture, lighting, friction, and timing regularities absent in the plant; performance collapses on real images/traces until a small paired-real adaptation set + embedding alignment is added. Numbers: RF-DETR on 550 synthetic inspection images ≈ perfect in sim → **0.2516 mAP real** → **0.8853 mAP with only 50 paired real images** + 500 unpaired synthetic (k-DPP + KL alignment) (PHM-Europe 2026). Production-cell object detection: +11–15% from domain-informed synthetic mixes (C4 combo +15% over best single procedure). Deep-draw: geometry explains 77–92% of sim-reality deviation variance; blank-holder force up to 33% of geometry-adjusted variance (9,000 matched sim-real instances, 2026). CFRP RTM: sim-only 93.4%/0.33 IoU → transfer with 10 real samples 95.3%/0.38, with 240 real 95.9%/0.50; better-physics sim +4pp accuracy / 3x IoU vs naive sim.
Evidence (L1): https://www.papers.phmsociety.org/index.php/phme/article/view/5037 ; https://arxiv.org/html/2311.11039v2 ; https://doi.org/10.1007/s12666-026-03870-5 ; https://doi.org/10.21203/rs.3.rs-2186337/v1 ; framework: https://osf.io/86cgu
Endangers: **TWIN fidelity claim + ACT simulate-before-act.** If the SimPy twin is the safety oracle for A1–A7, its uncalibrated gap IS the safety gap. Must report: sim-chosen action → real-log replay delta, and the "50-real-sample recovery curve" for Verdandi's own twin; otherwise every RL/correction number is sim-fiction.

## 2) Anomaly-detection failures on real plant data

**F4 — Threshold miscalibration under drift: high precision, collapsed recall (the 53x-scale-shift case).**
Mechanism: fixed-percentile thresholds fit on validation residuals do not transfer when test anomaly-score scale shifts (train-test drift); the detector still ranks (precision ~0.96) but the cut misses most episodes. Numbers: TCN-GAT reconstruction AE on SWaT: **F1 0.281 vs reported SOTA 0.886; precision 0.962, only 6/18 attack segments detected; ~53x validation→test score-scale shift** (ICCI 2026 negative-result study). General form: 5 detectors on SWaT share ROC-AUC 0.75–0.877 yet F1@p99 spans **0.025–0.764** (TranAD 0.75 AUC → 0.008 pAUC@1% → 0.025 F1@p99) — the cut, not the representation, decides the leaderboard (Shi et al. 2026).
Evidence (L1): https://doi.org/10.1109/icci68752.2026.11506458 ; https://export.arxiv.org/pdf/2608.02821
Endangers: **TRACE entry point (anomaly → ranked walk).** Verdandi's trace is only as good as its trigger threshold; a drifted threshold silently starves PCMCI/walk of episodes. Must fix threshold on validation, freeze it, report event-level miss rate — not re-tuned point-F1.

**F5 — Alarm-budget collapse: unconstrained metrics reverse under operator workload.**
Mechanism: papers report threshold-free / point-adjusted scores; operators can handle B events/hour. Under matched budgets no detector dominates: **SWaT miss 0.62–0.97 at B=0.5 events/h; on WADI some methods hit near-zero miss only at >10x intended alert workload.** Rankings flip once workload is enforced.
Evidence (L1): https://doi.org/10.1109/access.2026.3659034
Endangers: **TRACE + NARR operating-point claim.** If Verdandi reports F1 without B (events/h) + duty-cycle guardrail + detection delay, it is reporting lab numbers. Downstream register must add: Pareto miss-vs-budget curve on topology-A.

**F6 — PA-metric inflation (Kim et al.): point-adjust makes weak detectors look SOTA.**
Mechanism: point-adjust (PA) protocol marks a whole segment correct if ≥1 point fires; long attacks then inflate F1 by 2–5x vs event-level scoring. Classical result reused across SWaT/WADI literature; deployment-first re-evals (2026) show the same protocol flips model choice (graph vs flow vs spectral) once unified splits + event aggregation + frozen thresholds replace PA.
Evidence (L1): Kim et al. PA critique (canonical); 2026 replication: https://arxiv.org/html/2602.15457 ; https://ismart.ece.mcgill.ca/ASTAD_Papers/14_Benchmarking_IoT_Time_Serie.pdf
Endangers: **BENCH credibility.** Any Verdandi benchmark using PA or test-tuned thresholds is leaderboard gaming by definition. Use event-level F1 + miss@B + delay.

**F7 — Deep-vs-classical upset + regime-blindness: no universal winner; flows collapse on drift, graphs erode on noise.**
Mechanism: inductive bias matches a regime, then fails outside it. Numbers (14 models × 7 datasets, zero test-time calibration, 2026): SWaT+noise GBAD **0.804→0.677 (−16%)**, STGAT 0.759→0.680 (−10%), MTAD-GAT 0.762→0.756 (−0.8%); flow/density (GANF) mild on SWaT (−1.5%, 0.795→0.783) but **collapses toward 0.0 on SKAB/NPP under small log-drift**; THOC ~0.90 on TEP but −45–70% on SWaT/WADI noise; fixing learned DAG gives +0.5–1.0pt clean but **~8x drift sensitivity**; single-sensor zeroing flips an industrial run **0.38→0.58 (+54%)**.
Evidence (L1): https://arxiv.org/html/2602.15457 ; https://doi.org/10.1109/access.2026.3659034 ; https://export.arxiv.org/pdf/2608.02821
Endangers: **TRACE detector choice + TWIN regime coverage.** A GDN-style raw-0.81-SWaT number does not transfer to WADI-localized attacks (pooled energy dilutes local valves/pumps; NSIBF subspace wins: ROC 0.796/PFULL 0.852 vs 0.53–0.61 pooled). Verdandi must report per-regime (noise/drift/dropout/long-episode) slices, not one F1.

## 3) Causal-discovery failures

**F8 — Prior-fragility: wrong mask hurts vs blind; fixed graph buys clean points, pays 8x in drift.**
Mechanism: topology/mask priors constrain search; correct priors help, wrong priors (wrong edge forbidden/required, stale P&ID) force errors worse than unconstrained discovery. Measured proxy: fixing the learned DAG +0.5–1.0pt clean but ~8x drift sensitivity (2026 stress suite, §F7). PCMCI+/LPCMCI/FGES rankings themselves shift with confounding, nonstationarity, and noise family on TimeGraph (KDD 2025).
Evidence (L1): https://arxiv.org/html/2506.01361v1 ; https://github.com/hferdous/TimeGraph ; stress result https://arxiv.org/html/2602.15457
Endangers: **TRACE (PCMCI + walk) core claim.** Verdandi hard-codes topology-A (~22 nodes) as prior; if the plant drifts (new bypass, re-routed conveyor) the prior becomes the error source. Register needs: prior-ablation (blind vs masked vs wrong-mask) on seeded replay.

**F9 — Drift/nonstationarity collapse: TPR → 0.00-class on trend/shift regimes.**
Mechanism: stationarity-assuming discovery (PC/FGES/PCMCI variants) loses true edges under deterministic trends, regime shifts, and heavy confounding; TimeGraph variants show the TPR/FDR/SHD spread across A1 (linear) → B/C (nonlinear, t-noise, confounded) + D (trend/compound-violation) groups, with trend/compound groups saturating to abstention in downstream decision wrappers.
Evidence (L1): https://arxiv.org/html/2506.01361v1 ; https://doi.org/10.13016/m2bdv1-2aeo
Endangers: **TRACE under plant drift.** A 32-machine line with tool wear, seasonal temperature, and batch changes lives in group-D conditions. Must report TPR@FDR on drifted replay, not clean-stationary replay.

**F10 — Topology-free parity: simple methods match/beat causal graphs (SimpleRCA/PRISM line).**
Mechanism: when the dependency graph is absent or stale, deviation-ranked walks / component models with guarantees beat full DAG discovery at a fraction of cost. Numbers: PRISM **68% Top-1 over 735 failures × 9 datasets, +258% over best baseline, 8 ms/query** without a dependency graph; RCA-survey line (CausalRCA/CloudRanger/Microscope/MS-Rank family) shows graph-construction cost rarely pays unless telemetry is pre-sliced.
Evidence (L1): https://arxiv.org/html/2408.13729v2 (survey of graph-based RCA limits); PRISM result p. via search (arXiv 2601.21359); https://arxiv.org/html/2209.02500v2
Endangers: **TRACE complexity claim.** If a topology-free baseline ties PCMCI+walk on Verdandi's own 735-style failure set, the causal layer is unjustified overhead. Register must include a SimpleRCA/PRISM-style baseline as the null to beat.

**F11 — Scale collapse: hundreds of nodes break constraint/GES search.**
Mechanism: conditional-independence test count and GES neighbor search grow superlinearly; full-plant graphs (hundreds of tags: WADI 123 tags vs SWaT 51) force subsampling that drops the true root. WADI-localized-attack dilution (§F7) is the small-scale preview.
Evidence (L1): SWaT/WADI scale contrast in https://export.arxiv.org/pdf/2608.02821 ; TimeGraph density ablations https://arxiv.org/html/2506.01361v1
Endangers: **TRACE scaling story (22 → 32 → plant-wide).** Report runtime + Top-k vs node count; show where walk degrades and what gets pruned.

## 4) LLM-explanation failures

**F12 — Ungrounded overclaim: 28% severe overclaims LLM-only → 2% grounded.**
Mechanism: LLM-only QA hallucinates readings, stops at downstream symptoms, invents metric values outside context. Numbers (IndustryAssetEQA, ACL-Industry 2026, 4 asset classes incl. turbofan/hydraulics/production systems): structural validity **+0.51**, counterfactual acc **+0.47**, explanation entailment **+0.64**, severe expert-rated overclaims **28%→2% (~93% reduction)**; per-model: Prov.OK 0.47→0.89, Claim-precision 0.12→0.67–0.74, answerability 46%→97%, grounding 3.0→4.5/5.
Evidence (L1): https://aclanthology.org/2026.acl-industry.49/ ; https://arxiv.org/html/2604.23446 ; code https://github.com/IBM/AssetOpsBench/tree/IndustryAssetEQA/IndustryAssetEQA
Endangers: **NARR (grounded narration) claim.** Ungrounded narration IS the 28% regime. Verdandi's grammar-constrained narration without a provenance/KG gate repeats it. Register must require claim-precision + entailment-pass + overclaim rate, not BLEU/fluency.

**F13 — Forced-hallucination-to-satisfy-grammar + verifier-passing-but-wrong IDs.**
Mechanism: a strict output grammar (JSON cause-chain / ranked IDs) pressures the LLM to fill slots even when evidence is missing; a weak verifier (string-match / soft entailment) then passes a well-formed but wrong root ID. Antidote measured: EviGuard's three-valued logic {supported, contradicted, **unknown**} + cause_candidate (never assert proven cause from logs alone) + response gate blocking high-impact acts on any unknown/contradicted precondition.
Evidence (L1/L2): EviGuard mechanism via search (MDPI CPS reasoning 2026); IndustryAssetEQA entailment gap (Entail.Pass 0.08→0.72) as the verifier-absent baseline.
Endangers: **NARR → ACT handoff.** If narration output feeds A1–A7 directly, a grammar-valid wrong ID becomes a wrong physical action. Require EviGuard-style unknown-gating before any ACT call.

**F14 — Multi-turn red-team pushes operator agents past safety limits 8–12%.**
Mechanism: adaptive multi-turn attackers (not single prompts) social-engineer role-teams (SRO/RO/TO/STA/AO) across turns until a critical safety function is lost. Numbers (NRT-Bench, KAERI + AIM Intel, Jun 2026, nuclear control-room sim, 4 frontier operator models, paired-replay): **8.7–12.1% of attack sessions end losing a critical safety function**; of 149 paired sessions none defeats all four models but **~1/3 defeats ≥1** (failures barely overlap — aggregate rate hides model-specific holes). Companion SCADA SafetyBench (IEC 60870-5-104) shows theCompleteness gap: models score 0-fail on refusal but differ on spelling out required checks (untrusted-note / two-person confirm).
Evidence (L1): https://arxiv.org/html/2606.20408v2 ; https://huggingface.co/datasets/Albertmade/nrt-bench ; https://github.com/heinrihs-s/Scada-Agent-SafetyBench
Endangers: **ACT autonomy + NARR operator-facing text.** Any Verdandi agent that takes free-text "operator suggestions" or multi-turn context without a safety-channel model inherits the 1-in-10 breach rate. Register must red-team ACT with multi-turn (not single-prompt) attacks before any live-loop claim.

## 5) Autonomous-correction failures

**F15 — Rerouting oscillation / nervousness: replanning faster than the plant amplifies noise.**
Mechanism: congestion triggers re-route; re-routes invalidate each other's reservations; controller replans again — routes change faster than AGVs move. Field signatures: task time **+20–35% in peak waves while utilization looks high**; **8–12% of shift in wait at the same 3–5 nodes**; 100–300 ms comms delay at 1.2–2.0 m/s forces conservative stops or unsafe aggressive re-permissioning.
Evidence (L2): https://www.hycmoop.com/news/robotics-automation/agv-amr/When-does-an-AGV-traffic-control-algorithm-fail.html ; https://www.hycmoop.com/news/robotics-automation/agv-amr/Deadlock-risks-hidden-in-AGV-traffic-control-algorithms.html ; https://doi.org/10.1109/tase.2023.3276233
Endangers: **ACT reroute-class actions (A-list) + simulate-before-act latency.** If Verdandi's simulator assumes zero-latency perfect execution, it will prescribe the oscillation it cannot see. Measure: replan rate vs plant-state rate + wait-state % on replay.

**F16 — AGV pileup / deadlock cascade (named pattern: e-com promo-night collective halt).**
Mechanism: path-conflict + lagging scheduler under a batch surge → hundreds of AGVs halt across aisles, conveyors pile up, orders lost (reported pattern: **hundreds of trolleys halted, >¥2M orders lost** in one promo-night sorting collapse; WMS batch spikes exceed zone buffers in 2–3 min).
Evidence (L2/L3): https://www.pusr.com/blog/5773.html ; https://www.hycmoop.com/news/robotics-automation/agv-amr/AGV-and-AMR-Fleet-Software--When-Dispatch-Logic-Breaks-Down.html
Endangers: **ACT closed-vocab safety case.** A1–A7 must include an explicit "hold / do-not-reroute" action and a deadlock detector; otherwise the correction agent is the pileup author. Register: pileup-injection test on topology-A.

**F17 — RL-loses-to-heuristics OOD: beats SPT/EDD in-distribution, collapses outside it.**
Mechanism: policy overfits arrival-rate / breakdown-frequency / layout distribution; OOD (rush mix, novel bottleneck, new machine count) pushes states off the value-function manifold → erratic actions, while SPT/EDD (relative-attribute rules) degrade gracefully. Mitigation literature converges on hyper-heuristics (RL picks the rule, not the dispatch), domain randomization, and GNN encoders — i.e., RL needs the heuristic as a guardrail.
Evidence (L1 synthesis of scheduling-RL OOD literature via 2026 search; Dow/wafer-fab in-distribution wins vs OOD collapse pattern).
Endangers: **ACT masked-PPO core bet.** Verdandi's masked-PPO on one synthetic topology is the overfit setup by construction. Must report in-dist vs OOD (new arrival mix, machine-count change, breakdown regime) vs SPT/EDD/CR baselines; ship hyper-heuristic fallback if OOD gap > margin.

**F18 — Delayed-effect misattribution + hand-specified repair probabilities.**
Mechanism: dispatch/scheduling rewards are sparse and delayed (early mis-route → tardiness hours later); value nets misassign credit across the horizon, especially when OOD changes horizon length. Hand-set repair success probabilities then launder the misattribution into "expected-value" action ranking that is pure assumption.
Evidence (L1): scheduling-RL credit-assignment literature (sparse/delayed reward + horizon-shift failure; see §F17 sources); general RL-extrapolation failure.
Endangers: **ACT reward + simulate-before-act scoring.** If Verdandi's repair-probs are hand-set and its horizon is fixed, the A1–A7 ranking is assumption-driven. Register: sensitivity sweep over repair-probs + horizon; require learned-or-measured probs before any "optimal correction" claim.

## 6) Benchmark / eval failures

**F19 — Leaderboard gaming via protocol sensitivity (SOTA2 rows).**
Mechanism: SOTA changes with threshold rule (test-tuned vs validation-frozen), point vs event scoring, PA on/off, and split choice. Demonstrated reversals: TranAD 1st-on-HAI/last-on-SWaT while NSIBF 1st-on-WADI/last-on-HAI; flow-vs-graph swaps under drift/noise (§F7); alarm-budget reversals (§F5).
Evidence (L1): https://export.arxiv.org/pdf/2608.02821 ; https://doi.org/10.1109/access.2026.3659034 ; https://arxiv.org/html/2602.15457
Endangers: **BENCH ambitions.** A single-domain, single-protocol Verdandi leaderboard is a SOTA2 artifact. Publish the protocol (splits, frozen thresholds, event aggregation, budget B) alongside every number or the number is void.

**F20 — Single-domain generalization gap + frontier-LLM operational-decision ceiling (~≤18% → ~1/3 with scaffolding).**
Mechanism: raw frontier models drown in noisy multi-modal telemetry, hallucinate values, stop at symptoms, miss multi-hop propagation; RCAEval (735 real failures), OpenRCA (long-context enterprise telemetry), TraceBench/FactoryBench show raw models solving <1-in-10 hard cases, up to ~1-in-3 moderate cases only with SOP guardrails + deterministic pre-triage + human-in-loop. EviGuard authors' own stated limit: single-domain validation (the paper's scope caveat).
Evidence (L1/L2): RCAEval/OpenRCA/TraceBench/FactoryBench synthesis via 2026 search; EviGuard scope limit; Gartner 2026 context (agents orchestrate ~10% ops by 2030, humans approve).
Endangers: **BENCH + NARR + ACT autonomy narrative.** Verdandi is single synthetic domain by design — exactly the regime the literature says does not generalize. Any "agent resolves X%" claim must be conditioned on domain + scaffolding + approval gate, or it joins the ≤18% graveyard.

---

## Contradiction-seeking results (where the failure did NOT materialize / safeguards proved unnecessary)

**C1 — High-fidelity bounded twins DO pay: virtual commissioning 50% faster, 10–15% CapEx saved.**
Where sim-transfer worked: structured robotic/packaging lines with rigidly bounded physics (payload, friction, PLC latency constrained) + bidirectional edge-synced SCADA/PLC models (Siemens/NVIDIA Omniverse stacks, e.g., PepsiCo lines): **up to 50% cut in integration/ramp time, 10–15% CapEx savings, ~90% of bottlenecks caught virtually.** The failure (high-mix machining with unmodeled hardness/coolant variance + 400 ms cloud lag + tribal-knowledge overrides → 180% overrun, 3 collisions, $120k tooling loss, shelved as dashboard) is the unbounded-physics counterpart.
Evidence (L3, directional): 2025–2026 manufacturing-twin ROI synthesis via web search.
Verdandi implication: the twin is defensible IFF Verdandi stays in the bounded-discrete regime (fixed topology-A, quantized states, edge-rate sync) and publishes the sim→real recovery curve (§F3). Unbounded-physics claims (tool wear, thermal) are out of scope until measured.

**C2 — Priors + invariant mechanisms DO survive shift when shift is sparse and structured.**
Where causal fragility did not materialize: under the Sparse-Mechanism-Shift hypothesis only a few conditionals change per regime; multi-environment data (shifts/batches/suppliers as passive experiments) + structural priors (machine order, P&ID, KG) recover the graph without hazardous active interventions. Invariant-mechanism methods then adapt where correlational models degrade.
Evidence (L1 theory line: invariant causal prediction / sparse-shift literature via 2026 search).
Verdandi implication: keep the topology-A prior BUT version it (prior-v1 vs plant-rev), log prior-vs-blind vs wrong-mask deltas (§F8), and treat each seeded replay regime as an environment for invariance tests. The safeguard (prior-gating) is necessary; the failure it prevents is silent wrong-graph confidence.

**C3 — RL DOES beat heuristics in-distribution; the guardrail (not RL) is what makes it deployable.**
Where RL-collapse did not materialize: Dow/wafer-fab-style controlled deployments where the arrival/breakdown distribution is stationary: RL minimizes tardiness/makespan vs SPT/EDD by anticipating bottlenecks. Deployment consensus is RL-as-hyper-heuristic (RL selects among SPT/EDD/CR) + action masking + PLC feasibility filter — the heuristic is the safety layer, RL the optimizer. "No guardrail needed" is attested as fallacy in every deployment source found; no credible no-guardrail success case surfaced.
Evidence (L2/L3 deployment consensus via 2026 search).
Verdandi implication: keep masked-PPO IFF the mask + heuristic fallback + PLC-feasibility filter are first-class (measured OOD gap vs SPT/EDD, §F17). The safeguard proved necessary everywhere it was tested; cutting it is the failure.

---

## Query log (17 distinct; 3 contradiction-seeking marked *)
1. GE Predix Uptake failure postmortem digital twin platform overreach 2026
2. sim2real gap manufacturing quantified simulation improves real degrades percent
3. anomaly detection real plant data precision collapse threshold miscalibration SWaT WADI
4. Kim et al point-adjust PA metric inflation time series anomaly detection F1 overestimate
5. GDN graph neural network anomaly detection SWaT 0.81 collapse regime deep vs classical
6. TimeGraph causal discovery temporal drift TPR collapse benchmark
7. SimpleRCA topology-free root cause analysis microservice parity causal graph
8. IndustryAssetEQA grounding reduces hallucination overclaim rate LLM industrial QA
9. NRT-Bench SCADA safety bench LLM agents pushed past safety limits red team
10. AGV pileup warehouse incident rerouting oscillation nervousness autonomous dispatch failure
11. EviGuard three-valued verifier causal explanation 2026
12. RCAEval OpenRCA TraceBench FactoryBench frontier LLM operational decisions low accuracy
13. reinforcement learning loses to heuristics out-of-distribution manufacturing scheduling dispatch
14. *digital twin success ROI counterexample where simulation transfer worked manufacturing 2025 2026
15. *causal discovery robust to distribution shift prior knowledge helps manufacturing case where safeguards unnecessary
16. *reinforcement learning beats heuristics manufacturing scheduling success counterexample no guardrail needed (= Uptake postmortem slot covered in same batch)
17. Uptake industrial AI failure postmortem valuation downfall IIoT graveyard
