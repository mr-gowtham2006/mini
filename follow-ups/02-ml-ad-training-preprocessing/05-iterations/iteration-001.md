# Iteration 001 — ml-ad-training-preprocessing

## Research Question
Everything ML needed before changing data generation: AD training formulation + full preprocessing pipeline for the Verdandi twin, with generation requirements back-propagated to the twin (training-first ordering, user-mandated).

## Date
2026-09-13

## Actions Taken
- Phase 0: CONTEXT with measured twin facts (29.5% STARVED clean, 13% kit funnel, 4–7σ caricatures); inherited F1 lock + M0b design + sibling 01 locks; no training-pipeline Linear issue exists.
- Phase 1: academic (11 queries, 25 sources, 12 L1–L2: GDN-raw/Kim-repro/Lau-STAD/M2AD/SPOT/Schmidl/Affiliation/Elephant full-texts) + industry (12 queries, 26 sources, 10 official L3 docs) → merger: 51 rows, 7 themes T-C1..T-C7, 9 contradictions (rows 7–9 settled inputs).
- Phase 2: exactly 5 hypotheses with M0b tripwires (H1 +3pp & recall≥0.30; H2 +3pp gate; H3 +2pp & STARVED-heavy; H4 ±2pp wire; H5 stability rule).
- Phase 3: confirmation (new sources: STAND/STAD, DevNet/OSAD recipe, TEP +14.6%, TSFM norm study) vs falsification (25 queries: H1/H3 tripwire-2 legs provisionally falsified; H2/H4/H5 not falsified) + claim-lock (6 scope-limited LOCK-1..6; U-1..U-6; 5 H citations purged; H1/H3 Contested, H2/H4/H5 Inconclusive; 0 refuted without M0b).
- Phase 4: TF1–TF10 + conclusions (7/3/6) + 12 open questions (5 P0 M0b-blockers) + 5 recommendations + training-preprocessing-spec (§1–§7 incl. 7 back-propagated generation requirements + Linear issue definition).

## Key Decisions
| Decision | Rationale |
|---|---|
| Training-first ordering | User mandate; generation (01 spec) waits on training requirements |
| Rows 7–9 settled, not hypothesized | Grouped splits / triggered retraining / no-feature-store already decided — hypotheses reserved for live rows |
| Per-state co-default (H3) | Tripwire-2 provisionally falsified both single-model default and filter-only; both ship to M0b |
| H4 M-first default + signed wire | Zero direct papers; pipeline must ship something with a reversal tripwire |
| H citations purged, never cited | Han-1%/OSAD/DCD-VAE/A19-half/FluxEV failed field verification |

## Results
- **Established:** 7 findings (TF1 grouped splits; TF2 weights-first/SMOTE-OFF; TF3 classical-tripwire gate; TF4 regime-conditioning direction; TF7 median/IQR norm; TF9 thresholding incumbency+constraints; TF10 PA-inadmissibility) + Moderate TF2
- **Contested:** TF5 (state ordering), TF6 (supervision: known-margin survives, unknown-leg falsified)
- **Inconclusive:** TF8 (norm scope — genuine literature gap), TF9c (3-arm winner — unrun)
- **Overall confidence:** Established High (mechanism + multi-domain) / Moderate (L3-capped TF2); contested legs carry no numbers

## What Was Learned
Training on twin data is decided by five pipeline laws (grouped splits, weights-first, classical gate, regime conditioning, robust norms) plus three open bets the M0b battery must settle (state ordering, supervision's unknown-family leg, norm scope). The twin's three measured pathologies (29.5% idle, 13% funnel, caricature mags) each got a training-dictated generation requirement with teeth (§5), and the T9-style duty-cycle/magnitude/rate proposals from the 01 discussion now have training-side justification instead of intuition.

## Next Steps / Open Paths
M0b battery settles H1–H5 + TF5/TF6/TF8/TF9c (P0 questions); MINIPRO-17 implements §5 back-propagated requirements before the freeze; new Linear training-pipeline issue (§7) filed after M0b; P1 questions (dose, CPU-cost, masking, GG secondaries) follow.
