# Context: sim-depth-physics-ml (follow-up 01)

Triggered by user 2026-09-13: "simulate the actual real world factory as accurately as possible with all the complex relationships and physics and also various different metrics of the machines not just minor ones... see how we can make [the simulation] better... extensively research what is better... use linear MCP to see the current issues... make the simulation better rather than just at the anomaly parts, but properly simulation in depth... how can we actually find the complex anomalies and their trace back in real factory simulation."

## What the parent research established (relevant excerpts)
- F1 (Locked, 85-90%): point-adjusted F1 inadmissible; raw point-wise F1 only.
- F4 (Conditional 70-78%): topology-masked PCMCI+ tau=2 + flip-gate is defensible causal layer, conditional on mask correctness; 14.4% number untested.
- F5 (Contested): universal detector/RCA/grounding bars refuted; twin-scoped replacements unresolved — M0b battery owed (quantile vs GDN-light vs MP guardrail, AC@1 ≥70% + ≥10pp graph ablation, template ≥90% vs open+RAG control).
- F6: wedge = ranked-trace ≤3 steps + provenance narration + seeded replay jointly, CPU-local <10min.

## Current implementation (read 2026-09-13)
- `src/twin.py` (~1400+ lines): SimPy DES, 32 machines (A0-9/B0-9/C0-7/ASM0-2/RWK0), 31 buffers + SBUF(30) + AGV pool cap 2, rework max 2 passes, part-carried DEGRADE/REJECT flags.
- Signal: `clean = base + 0.5σ·sin(2π·t/cycle) + AR1(0.6)`; state offsets RUN 0 / STARVED −2σ / BLOCKED −1σ / DOWN −3σ; clamp ±6σ. Temp = uniform per class band (NO thermal inertia). Throughput 0/1 per step. No motor current/power, pressure, flow, tool wear, energy, lubrication, ambient coupling.
- Faults (7): spike/drift/bias (origin-only signal dev), delay (+d cycle), loss (stale-hold thinning), breakdown (forced DOWN), quality (ASM2 reject 15-40%). Seeded streams: 32 noise + place/drop/agv/fail (SeedSequence spawn 36). T=300, CAL_WIN=120, GT-exclusion windows.
- `src/config.py`: Table 3.1 operating points, MTTF/MTTR, buffer caps, FAULT_RANGES (mag 4-7σ, dur 8-25, d 3-6, drop 10-30%, mult 1-3, reject 15-40%).
- Normative spec: `docs/SIM_SPEC.md` (456 lines). Battery: F1 ≥0.85 raw, AC@1 ≥70%, flip <40%, <600s wall, 0-diverge ×5.

## Linear state (read 2026-09-13)
- MINIPRO-16 (M0.1 twin) DONE. MINIPRO-17 (M0.2 acquisition, 7 channels) Backlog. MINIPRO-10 (M0b detector sensitivity, ensemble + shadow cross-partition) Backlog. M1/M2/M3/M4/M5 Backlog.

## User scope decisions (2026-09-13, via question tool)
- Physics: as deep as possible WITHOUT breaking detection/traceback quality and ML-trainability. Think ML-engineer POV: sampling rate (steps/sec), feature richness for later predictive models.
- Deliverable: survey-then-spec (gap-ranked survey + concrete twin upgrade spec for src/twin.py + config).
- Anomalies: all complex types (cascades + slow subtle + sensor-vs-process).
- Constraints: keep T=300 / <600s / 0-diverge / SimPy core; SciPy (ODE etc.) ALLOWED if CPU budget passes.

## What this follow-up must produce
1. `01-background/`: systematic lit review (academic + industry) on high-fidelity factory sim physics, sensor channels for ML, sampling-rate design, complex/cascading anomaly detection + root-cause traceback in real plants.
2. `02-hypotheses/`: ≤5 falsifiable hypotheses from contradictions.
3. `03-evidence/`: supporting + contradicting with claim-lock annex.
4. `04-synthesis/`: findings + conclusions + open-questions + recommendationsintel → twin upgrade spec (equations, channel table, fault extensions, sampling-rate recommendation, ML-training dataset design, acceptance tests) that respects SIM_SPEC bars.
