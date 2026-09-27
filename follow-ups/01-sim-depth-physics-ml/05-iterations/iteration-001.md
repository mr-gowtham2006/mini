# Iteration 001 — sim-depth-physics-ml

## Research Question
How to upgrade the Verdandi 32-machine SimPy twin toward real-world factory fidelity (complex relationships, physics, rich machine metrics) without breaking detection/traceback quality or ML-trainability — and how to detect + trace back complex anomalies in such a simulation. Deliverable: survey-then-spec.

## Date
2026-09-13

## Actions Taken
- Phase 0: read src/twin.py + config.py + SIM_SPEC.md + PLAN.md + ELENCHUS + M0B_PREREG + parent F1–F6; read Linear (Verdandi project, 9 issues, M0.1 Done / M0.2 + M0b + M1–M5 Backlog); scope locked via question tool (max-physics-for-ML / survey-then-spec / all-complex-types / SciPy-allowed).
- Phase 1: academic reviewer (10 queries, 16 sources, 10 L1–L2, text-only after 59-image provider failure + retry) + industry reviewer (12 queries, 25 sources, 7 standards/docs) → merger: PRISMA 176→176→36, 6 contradictions R1–R6, F1–F3 tension check clean.
- Phase 2: exactly 5 hypotheses H1–H5 from R1–R5, all with M0b-battery tripwires.
- Phase 3: confirmation (17 fetches, 5 support files) vs falsification (25 queries, 5/hypothesis, 5 contra files) + claim-lock annex (4 narrow locks L1–L4, 15 unresolved, 4 provisional refutations, 0 H citations, H1–H5 → Contested).
- Phase 4: findings F7–F13 + conclusions + 10 open questions + 5 recommendations + upgrade-spec.md (C1–C6 equation changes, 9-row channel table, fault taxonomy, ML-dataset design, T1–T8 tests, 5 claim boundaries).

## Key Decisions
| Decision | Rationale |
|---|---|
| Follow-up dir, not new topic | Parent F1–F6 locks constrain the spec; follow-up inherits them |
| Text-only + PDF→MD standing rule | Provider 59-image failure killed attempt 1; user instruction adopted permanently |
| No-team fallback (task delegates) | team_* tools unavailable in this environment |
| upgrade-spec.md as 5th synthesis file | User scope decision demanded survey-THEN-spec, not survey-only |
| All H → Contested, narrow locks only | Battery unrun; no twin-transfer number is evidence-backed yet |

## Results
- **Hypotheses falsified (provisional legs):** 4 legs (H2 separation, H3 10/1000, H5 manual-spec, H1 unconditional-depth) — each with surviving narrowed reading
- **Hypotheses corroborated:** 0 (battery-decisive by design)
- **Contested:** 5/5 (H1–H5)
- **Locked established:** 4 narrow (L1–L4, scope-limited, no transfer numbers)
- **Overall confidence:** Established High 85–88% / Moderate 72–80% (F12–F13 narrow); Contested, no number (F7–F11)

## What Was Learned
The twin's physics gap is real but the binding constraint is decision-grade evidence, not equations: every new term must survive an M0b-style ablation (A-S5), impulses must be envelope-calibrated not waveform-matched (A-S4), parity needs redundant channels (single-obs cannot disambiguate), and PCMCI+ needs a graph-distance complement for wear drift (TimeGraph 0.00/1.00). Industry says the architecture (DES + historian + ML, 1 Hz) is already right — the upgrade is channels + wear + taxonomy + calibration discipline, not a rewrite.

## Next Steps / Open Paths
M0b battery run decides H1–H5 (Q1–Q5 open questions, P0/P1); MINIPRO-17 acquisition implements P0 (CH8 current + CH9 energy + transfer boundary) then P1; M0b ensemble + shadow cross-partition (MINIPRO-10) consumes the tiered export + precision-side range.
