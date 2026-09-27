# Verdandi gap falsification — devil's advocate record (2026-09-16)

Method: 18 targeted searches (2+ per gap), biased toward finding covering prior art.
Verdict scale: FILLED = covering citation found (adopt-and-cite) · NARROWED = partially covered, remainder stated · SURVIVES = nothing covering found.

## Gap 1 — simulate-before-act as re-runnable proof (replay hash audit)
- Queries: (a) academic "verifiable discrete event simulation replay audit hash manufacturing digital twin"; (b) tech "deterministic replay hash audit digital twin simulation proof SimPy".
- Closest art: ForensicTwin (MDPI J. Imaging-style, 2025 — SHA-256 hash-chained forensic logs, replay-attack prevention in DT simulation); "Towards Trustworthy Digital Twins: Verifiable Simulation via Recursive Zero-Knowledge Proofs" (Res.Gate, Jun 2026 — recursive SNARKs for verifiable simulation); Simio replay verification blog (Nov 2025 — replay ≠ hash audit); SimVerity (arXiv Aug 2026 — replay matched scenarios, verdict-transfer, but smart-home domain).
- Verdict: **NARROWED** — hash-chained audit (ForensicTwin) and verifiable-simulation proofs (ZK-SNARK paper) exist in general, but no SimPy discrete-manufacturing system gates correction actions on a re-runnable replay-hash proof. Remainder: action-gated replay-hash artifact in discrete-flow twin. Adopt-and-cite: ForensicTwin hash chaining, ZK verifiable-simulation framing.

## Gap 2 — empirical per-action success rates across seeded rollouts
- Queries: (a) academic "empirical action success rate seeded rollouts digital twin maintenance decision"; (b) tech "seeded rollout evaluation repair action success rate simulation sweep"; (c) academic "Bayesian updating maintenance repair probability learning from outcomes literature review 2025 2026".
- Closest art: TwinRL-VLA (2026 — seeds twin replay buffer with 20 successful rollouts, reports success %; but robot RL, not per-repair-action tables); Bayesian post-repair prognostics SBSM (PHM Europe 2026 — repair actions as priors, imperfect-repair RUL update; learns repair effect, but RUL-focused, not discrete-action success rates); Edge-AI DT predictive maintenance (2026 — 86 seeded degradation events, 62 to intervention; seeded eval, but policy-level not per-action).
- Verdict: **NARROWED** — seeded-rollout success measurement is standard in robot RL eval (LeRobot/Isaac-GR00T harnesses) and Bayesian imperfect-repair learning exists, but no discrete-manufacturing twin publishes per-correction-action success tables from seeded rollouts replacing hand-set probabilities. Remainder: the per-action table itself.

## Gap 3 — joint flow-topology + provenance-narration + seeded-replay system/bench
- Queries: (a) academic "flow topology provenance explanation replay digital twin manufacturing benchmark causRCA"; (b) web "causRCA HAI-CPPS Cloud-OpsBench DetTrace manufacturing root cause benchmark 2026".
- Closest art: causRCA (CIRPe/Procedia CIRP 2025 — joint RCA + causal-discovery benchmarking on CNC lathe HIL twin, expert causal graph; no narration/provenance, no replay); Cloud-OpsBench (LLM4Ops 2026 — agentic RCA + deterministic snapshot replay; K8s cloud, not manufacturing flow-topology); CausalTrace (AAAI 2026 — neurosymbolic causal + counterfactual + interactive operator interface in SmartPilot; no seeded replay or provenance narration).
- Verdict: **NARROWED** — every PAIR is covered (causRCA: joint RCA+CD; Cloud-OpsBench: agentic RCA+replay; CausalTrace: causal+interactive counterfactual), but the TRIPLE conjunction in manufacturing is absent. Remainder: one system/bench combining all three legs.

## Gap 4 — three-valued (supported/contradicted/unknown) verifier for manufacturing RCA narration
- Queries: (a) academic "three-valued verifier grounded explanation manufacturing root cause supported contradicted unknown EviGuard FIDES"; (b) web "Grounded Decoding TAMO-FoA verification manufacturing root cause narration hallucination".
- Closest art: EviGuard (MDPI Appl. Sci. 2026 — machine-verifiable evidence grounding for LLM industrial-incident reasoning; EXPLICIT three-valued design: "absence of evidence is not evidence of falsehood… verdict is unknown"; UCR 12.6%→1.7%, 96.1% claim precision); TAMO-FoA (2026 — tool-augmented RCA, AIOps/cloud, no three-valued verdicts); Grounded Decoding (NeurIPS 2023 — grounded generation for embodied agents, not a verifier); FIDES (conversational neuro-symbolic, sound routing, not three-valued).
- Verdict: **FILLED** — EviGuard implements exactly the claimed verifier (claim-level support/contradict/unknown with abstention) for industrial-incident RCA narration. Directive: adopt-and-cite EviGuard; Verdandi's remainder is only porting it to SimPy ranked-trace claims (mechanical, not novel).

## Gap 5 — in-dialog counterfactual executed on a deterministic twin mid-conversation
- Queries: (a) academic "conversational counterfactual deterministic digital twin manufacturing dialogue in-dialog simulation"; (b) tech "CALD CausalTrace AgenticTwin counterfactual accuracy IndustryAssetEQA manufacturing QA"; (c) web "FIDES conversational manufacturing what-if simulation deterministic twin evaluation 2026".
- Closest art: FIDES / conv_automata (Casciani — conversational layer + reasoning layer that "exploits either a digital twin simulating the production process or a formal verifier"; conversational what-if via twin); CausalTrace (AAAI 2026 — interactive counterfactual-effect module with real-time operator interaction); CNC DTE offline async "what-if" deterministic simulation (WSC 2024 — deterministic what-if, but not conversational).
- Verdict: **NARROWED** — FIDES kills the "conversational what-if via twin" half and CausalTrace kills "interactive counterfactual," but neither demonstrates a SEEDED/DETERMINISTIC twin with replay-verified counterfactual executed mid-dialog in discrete-flow RCA. Remainder: determinism + replay guarantee around the in-dialog counterfactual.

## Gap 6 — deterministic-gate agent benchmark for discrete-flow RCA+action
- Queries: (a) academic "deterministic gate benchmark discrete manufacturing root cause action agent SafetyBench FactoryBench"; (b) tech "OpenRCA TraceBench Supcon benchmark action coverage discrete flow agent evaluation".
- Closest art: OpenRCA 2.0 (2026 — causal process supervision with Path Recall metric; software telemetry, diagnosis-only, no action, no gate); TraceBench (Aug 2026 — controlled time-series root-cause attribution eval; no action coverage); Supcon Industry-AI-Agent-Benchmark (virtual-factory KPI scoring of agents; action+KPI but no RCA determinism gate); Agent-SafetyBench/FactoryBench (safety / machine-understanding QA, not RCA+action gating).
- Verdict: **SURVIVES** — no benchmark combines deterministic gating + discrete-flow + RCA + correction-action scoring for agents. Nearest parts (OpenRCA 2.0 process metrics, Supcon virtual-factory KPI bench) each miss ≥2 legs. This is a clean novelty claim.

## Gap 7 — benchmarked sensor-vs-process ambiguity pairs / multi-root overlap tracing challenges
- Queries: (a) academic "sensor versus process fault ambiguity multi-root overlap manufacturing benchmark TimeGraph"; (b) web "overlap automatic root cause analysis manufacturing fault taxonomy HAI-CPPS sensor fault benchmark".
- Closest art: Oliveira et al. "Overlap in ARCA in Manufacturing" (IJPR 2022; MDPI Appl. Sci. 2023 — formal overlap phenomenon + information-theoretic measure + overlap-resilient diagnosis; CHARACTERIZES overlap but ships no ambiguity-pair benchmark); MDPI servomotor multi-sensor study (2025 — residual ambiguity confined to similar-signature faults, noted not benchmarked); causRCA probe/hydraulics/coolant splits (fault-type partitions, not ambiguity pairs).
- Verdict: **NARROWED** — overlap is formalized and measured (Oliveira) and sensor/process confusability is observed, but no benchmarked ambiguity-pair suite or multi-root overlap tracing challenge exists. Remainder: the paired-case benchmark construction (deliberately confusable sensor-vs-process and overlapping-root cases with tracing scores).

## Gap 8 — measured operator trust/triage-time study for grounded-twin explanations
- Queries: (a) academic "operator trust triage time grounded explanation manufacturing digital twin user study NASA TLX"; (b) web "RAG manufacturer evaluation triage time diagnostic assistant grounded twin explanation measured 2026".
- Closest art: HAW-Hamburg REPOSIT study (Jun 2026 — N=22, LLM-supported decision-making in DT for predictive maintenance of industrial robots; measured diagnosis accuracy +25pp, task-completion time, NASA-TLX, trust TAS, UEQ-S — covers trust+time+load for LLM-twin diagnostics); job-shop scheduling study (2026 — N=253 explanation-vs-reliance, not twin-grounded); ED digital-twin triage study (different domain); RAG-at-manufacturer evals found only generic groundedness/faithfulness metrics, no triage-time.
- Verdict: **NARROWED** — HAW-Hamburg N=22 covers LLM-twin diagnostic trust + task time + TLX, but NOT for provenance-grounded narration with replay-verified actions in discrete-flow triage. Remainder: trust/triage-time study specific to grounded (claim-linked) explanations + replay-verified correction proposals.

## Tally
- FILLED (adopt-and-cite): Gap 4 (EviGuard).
- NARROWED (partial novelty — state remainder as claim boundary): Gaps 1, 2, 3, 5, 7, 8.
- SURVIVES (full novelty claim): Gap 6.
