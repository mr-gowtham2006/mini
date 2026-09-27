# Hypothesis registry — follow-up 02, Phase 2 (falsifiable, ≤5)

Date: 2026-09-13 · Source: `../01-background/contradictions-map.md` (rows 1–6 = seeds; rows 7–9 = settled inputs, NOT hypothesized) + `../01-background/literature-review.md` (T-C1..T-C7, 10 gaps).
Decision apparatus: M0b battery (quantile vs GDN-light vs MP guardrail + KNN/PCA tripwires; F1≥0.85, AC@1≥70% + ≥10pp graph ablation, flip<40%, <600s, 0-diverge; shadow cross-partition). Twin facts: T=300, 1 Hz, free sim labels, 32 machines, RUN 68.8% / STARVED 29.5% / DOWN 1.7%, fault mags 4–7σ rectangular.

## Locks honored
- Parent F1 (Locked): raw point-wise F1 only, PA-off, fixed-percentile calibration for the primary bar. No PA number anywhere.
- Parent F2/F3 (Locked): battery-scoped verdicts only; no ROI prose; no wedge claims.
- Sibling upgrade-spec §5 compatible: 1 sample/step base tick; multi-scale export; episode-seeded + wear-stratified splits; ALL stats inside folds; SMOTE-family inside-folds-only permission retained (default OFF per T-C5); calibration-normals ~300/episode is directional only.
- Retrieval rule: NO new retrieval before registry entries exist (honored — this registry is the pre-retrieval record).

## Registry (EXACTLY 5)

| ID | File | Seed row | Claim (one line) | Priority | Status (2026-09-13, post-devil's sweep) |
|---|---|---|---|---|---|
| H1 | `H1-supervised-with-free-labels.md` | Row 2 (supervision vs caricature; Leaning-A + hedge) | Per-family supervised-with-free-labels beats normal-only on twin data incl. unknown-family slice | P0 | Contested — tripwire-2 leg provisionally falsified (DRA/bias/RedLamp), known-family margin leg survives |
| H2 | `H2-classical-tripwire-gate.md` | Row 1 (GDN vs KNN; Unresolved) | No deep claim admitted unless GDN-light beats max(KNN,PCA) raw on twin data | P0 (gate) | Inconclusive — NOT falsified after 5 (PA rows voided, gate holds, awaits M0b read-off) |
| H3 | `H3-state-as-covariate.md` | Row 6 (state ladder; Leaning-B ladder) | Single-model-with-state-covariate + DOWN-masking beats filter-only AND per-state models on STARVED-heavy episodes | P1 | Contested — tripwire-2 leg provisionally falsified (per-mode 100%-vs-22.2% precedents), covariate-vs-filter leg survives; per-state co-default until M0b |
| H4 | `H4-normalization-scope.md` | Row 4 (per-machine vs global; open gap) | Per-machine robust norm beats global min-max by ablation margin | P1 | Inconclusive — NOT falsified after 5, closest call (zero direct-comparison papers; TranAD global-min-max + scale-erasure + thin-pool threats live) |
| H5 | `H5-thresholding-arm-selection.md` | Row 3 (thresholding; Unresolved) | Three calibration arms are distinguishable on twin data; joint criterion picks a budget-stable winner | P1 | Inconclusive — NOT falsified after 5 (POT≈Pct collapse risk + GG-payoff doubt live; M²AD disclosed pro-GG) |

## Seed disposition (why 6 rows → 5 hypotheses)
- Row 5 (resampling; Leaning-B default OFF): NOT hypothesized. Adopted as pipeline rule — default no-resampling + class-weight/cost-first; SMOTE only via ablation, always inside `imblearn.pipeline` (T-C5). Spec permission (§5 inside-folds) unchanged. A default-vs-ablation question with a settled default needs no hypothesis slot.
- Rows 7–9 (grouped splits; evidence-triggered retrain; no feature store): settled inputs, implemented as pipeline rules, never hypotheses.
- Gap coverage: H1→Gap 3, H2→Gap 9 (twin-scoped values await M0b), H3→T-C3/Q-D, H4→Gap 1, H5→Gap 4 + T7. Gaps 2/5/6/7/8/10 are spec/evidence work, not hypothesis slots.

## Cross-hypothesis rules
- H2 gates every deep reading of H1/H3/H4/H5: if a deep model is involved, its delta is reported vs the classical tripwire too.
- All numeric tripwires run on episode-seeded grouped CV (no window leakage), norms/thresholds fit inside folds, PA-off throughout.
- Any hypothesis failing its tripwire is marked Falsified with the date + battery ID; the registry is updated, not rewritten.

## Phase 3.5 claim-lock + devil's-sweep record (2026-09-13)
- Annex: `../03-evidence/claim-lock-annex.md` — 6 Locked (scope-limited, no twin-transfer numbers) · 6 Unresolved theaters (named missing legs U-1..U-6) · 0 Refuted (no wire fired on twin data; provisional leg threats are Contested, not refuted).
- Falsification coverage 5/5: every H-file carries a runnable numeric tripwire AND a completed 5-query devil's-advocate falsification attempt (`../03-evidence/contradicting/H1-contra.md … H5-contra.md` + `../03-evidence/evidence-log.md` Run D1). No numeric tripwire can fire before the M0b battery — literature verdicts are provisional legs only.
- Field-level citation check: per-source R/P/H table in annex §Field-level citation check (9 R / 11 P / 5 H; H removed with notes; 0 new fetches consumed).

*End — 0 Active / 0 Falsified / 0 Corroborated / 2 Contested (H1, H3) / 3 Inconclusive (H2, H4, H5); 5 falsification attempts recorded, 0 wires fired.*
