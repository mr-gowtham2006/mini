# Conclusions — follow-up 02: ML AD training + preprocessing

Date: 2026-09-13 · Read TF1–TF10 in `findings.md` first; nothing below adds evidence, it sorts verdicts.

---

## Established (ship as pipeline rules; cite TF-source)

1. **Splits + leakage discipline are settled** (TF1, High 85–88%): episode-seeded grouped CV, wear/maint/family stratification, 50%-overlap purge/embargo, all stats inside folds. Extends parent F1 and sibling upgrade-spec §5 without contradicting either.
2. **Imbalance default is weights-first, SMOTE default-OFF** (TF2, Moderate 72–80%): default no-resampling + class-weight/cost-first + per-class thresholds; SMOTE only via inside-`imblearn.pipeline` ablation. Upgrade-spec §5's inside-folds permission stands unchanged.
3. **Classical tripwires gate every deep claim** (TF3, High 86–90%): KNN/PCA (+iForest/LOF/OLS controls) in the M0b battery; deep needs ≥+3pp raw over max(KNN,PCA). Extends parent F1's protocol lock into a battery instrument.
4. **Condition on regime; never score mode-agnostic** (TF4, High 85–88%): state-as-covariate and/or per-regime structure + regime-conditioned thresholds + DOWN-masking discipline. Direction locked; rung unresolved (see Contested).
5. **Robust estimator is median/IQR** (TF7, High 85–90%): per-sensor median/IQR error norm; fit on RUN-normal-only inside folds. Scope explicitly excluded (see Don't-know).
6. **Fixed-percentile is the incumbent; SPOT has a spec the twin violates by arithmetic** (TF9a/b, High 85–89%): Pct ships as the standing calibration; POT arm runs under its n∼1000-vs-~300 handicap openly; production threshold-to-budget discipline (20/150/5 reporting) is practice, not theory.
7. **PA-on numbers never rank** (TF10, High 88–92%): restates parent F1 for the training pipeline; mechanics transfer, numbers void.

## Contested (both legs live; M0b tripwires decide; no number cited)

1. **H1 supervision (TF6):** known-family margin leg alive (STAND/DevNet/Lau direction) vs unknown-family leg provisionally falsified (DRA/bias/RedLamp/shape-bias). Ship both arms; fallback is normal-only + drift-chain.
2. **H3 ordering (TF5):** covariate-vs-filter leg alive vs C-vs-P leg provisionally falsified (product-aware + HSMM+PCA per-regime precedents). Ship covariate AND per-state bank as co-defaults + per-class escalation arm; no default winner.
3. **H5 outcome (TF9c):** three arms distinguishable vs collapse-to-Pct (POT≈Pct on F1; GG tax-without-gain). Ship the firing test with the joint criterion + stability rule; H5-ambiguous (budget-conditional) is a first-class resolution.

## Don't-know (named missing legs; need M0b or new retrieval — see `open-questions.md`)

1. **H4 scope U-4:** per-machine vs global normalization — zero direct papers; interim default (per-machine robust) is a safety choice with a signed ±2pp reversal wire, not a verdict.
2. **H2 twin outcome U-2:** whether GDN-light clears the classical wire on caricature-rectangular twin data — unclosable from literature by construction.
3. **H1 unknown recall U-1:** temporal open-set precedent for the 0.30 floor — Lau is non-temporal, OSAD removed; hedge-split read-off or targeted retrieval only.
4. **H5 distinguishability U-5:** shared-calibration 3-arm comparison — no study runs it; firing test only.
5. **Secondaries U-6:** synthetic-dose interior optimum on sensor manifolds; classical CPU-cost ≥10× leg; DOWN-masking isolated contribution; GG worst-machine edge numbers.
6. **Window/feature specifics:** base window length (5-vs-120 literature span is 24× and untransportable), feature-set composition, and multi-scale aggregation weights — method precedent (autocorrelation/RobustPeriod selection, envelope+FFT representation) exists but no locked finding; the spec (§5) fixes the procedure (derive from twin data), not the values.

*End — 7 Established · 3 Contested · 6 Don't-know. No unlocked numeric presented as established; no vague confidence; every verdict traces to a TF finding with its tripwire.*
