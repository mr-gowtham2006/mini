# Context: ml-ad-training-preprocessing (follow-up 02)

Triggered by user 2026-09-13: "research all things needed for the AD training later and the data preprocessing needed and think about that properly before thinking of changes for the data generation. Research about everything ML properly."

## Ordering principle (user-mandated)
Training + preprocessing requirements FIRST; data-generation changes (follow-up 01 upgrade-spec C1–C6, T9 duty-cycle proposal) are downstream of what training needs. This follow-up constrains generation; generation does not constrain it.

## Measured twin facts (2026-09-13, seed 777, T=300, 32 machines = 9600 machine-steps)
- Clean episode: RUN 68.8%, STARVED 29.5%, DOWN 1.7%, BLOCKED ~0%. Fault episodes nearly identical mix.
- Funnel: ~180 line parts → ~24 kits → ~22 sunk (≈13% kit completion; assembly/rework starved of examples).
- Fault mags 4–7σ rectangular (caricature); incipient 1–3σ absent. No wear state. Single obs per machine (no parity).
- 7 channels at 1 sample/step: vibration-RMS-grade obs, temp (uniform, no inertia), throughput 0/1, quality flag, state, buffer level, events. Follow-up 01 proposes CH8 current + CH9 energy + CH10 air + WSTATE + THERM + ENV + RC + SHF.

## Parent constraints (inherited, non-negotiable)
- F1 (Locked): raw point-wise F1 only, PA-off, fixed-percentile calibration.
- M0b battery (MINIPRO-10): quantile vs GDN-light vs MP-discord guardrail; F1≥0.85, AC@1≥70% + ≥10pp graph ablation, flip<40%, <600s, 0-diverge; shadow cross-partition.
- Follow-up 01 locks: L1 physics-residual gain, L2 envelope-SNR validity, L3 PCMCI+ drift collapse, L4 rate-budget practice. Contested: per-term transfer numbers, 299-normals rule, M2AD magnitudes.
- Linear: MINIPRO-17 (acquisition), MINIPRO-10 (M0b), M1 (hardening), M2 (narration). No training pipeline issue exists yet — this research should define it.

## What this follow-up must produce
1. `01-background/`: academic (AD methods for manufacturing time series + preprocessing/feature theory) + industry (production training pipelines, labeling, feature stores, deployment, Sim2Real fine-tuning).
2. `02-hypotheses/`: ≤5 falsifiable hypotheses (e.g. normal-only vs supervised-with-sim-labels; per-machine vs global normalization; state-conditional modeling vs state-as-feature; window-length/feature-set choices; thresholding rules).
3. `03-evidence/`: supporting + contradicting + claim-lock annex.
4. `04-synthesis/`: findings + conclusions + open questions + recommendations + **training-preprocessing spec**: problem formulation per fault family, architecture shortlist with selection criteria, full preprocessing pipeline (cleaning → state handling → normalization → windowing → features → splits → imbalance → calibration), leakage-discipline rules, evaluation protocol (raw-F1 + precision-side + delay-aware), data-generation requirements BACK-PROPAGATED to the twin (duty cycle, magnitude ladder, rate budget, stratification keys the twin must export), and a proposed Linear training-pipeline issue definition.
