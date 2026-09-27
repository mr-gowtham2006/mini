# Claim-lock annex — sim-depth-physics-ml (Phase 3 Step 3.5)

Date: 2026-09-13 · Gate: HIGH-RISK RULE enforced — a high-risk non-code claim locks only with ≥2 independent domains + counter-search + primary source. Snippet-only/UNVERIFIED items (9 supporting flags + 2 walled falsification sources + A-S8b) cap at Unresolved, never Locked. Unlocked claims may not appear in synthesis. No battery ran anywhere, so no registry tripwire fired empirically; "provisionally falsified" = literature-level only.

## Locked

### L1 — Physics-residual channel moves detection numbers vs raw-signal baselines (existence demo, TEP scope)
- Claim (narrow): adding a physics-model residual channel (measured − nominal) + calibrated operating points lifts detection over raw-signal SOTA families on the same benchmark.
- Domain 1 (primary): Khan et al., Sci Rep 16:17488 (2026), full text fetched — macro F1 0.93±0.02 vs cited data-only plateau ≈0.907–0.917, NAB +17% over Isolation Forest, ECE ≈0.03, alarms-per-hour operating-point reporting.
- Domain 2: A-S4 (IEEE TII, CHP facility, full text Phase 1) — DT-simulated normal + few anomalies trains Siamese AE to FPR ~9%/FNR ~12%, beating SOTA unsupervised AD; independent facility, independent mechanism.
- Counter-search: H1-contra 5 queries (low-fi parity JIM-2024, Truong CoRL23 sign-reversal, 6 data-gap postmortems, selective-fidelity) — pressured the *predicted pass terms*, did not overturn the residual-gain existence demo.
- Scope limit: does NOT lock that any specific twin term (wear-knee, current coupling) clears ΔF1 ≥3pp — that needs the battery.

### L2 — Envelope-band energy/SNR separation is the community's working acceptance statistic for impulsive faults (metric validity, not twin transfer)
- Claim (narrow): targeted envelope-spectrum SNR / sub-band energy separation is an established, optimised detection metric — which licenses H2's envelope-domain acceptance test as the right instrument.
- Domain 1 (primary): GES2N, Schmidt/Wilke/Gryllias, arXiv:2405.00727v2, full text fetched — gradient-optimised envelope-SNR objectives beat CYCBD/ACYCBD/MOMEDA/envelope-norms/negentropy on 3 gearbox datasets (SES metrics M1–M4).
- Domain 2: A-S2 (MSSP 2016 review) — spectral kurtosis + kurtogram + envelope analysis is the standard impulsive bearing/gear pipeline; plus A-S12 (19 datasets, full bandwidth beats downsampled ≥8pp).
- Counter-search: H2-contra 5 queries — attacked the twin's *synthetic-impulse transfer* (C-H2-3: real envelope needs 2–10 kHz resonance), not the metric itself.
- Scope limit: does NOT lock that the twin's 1/step wear-coupled impulse term delivers ≥15% — that leg is provisionally falsified (see Refuted R2).

### L3 — PCMCI+ collapses on nonlinear / trend-seasonal regimes while holding linear-stationary (benchmark fact)
- Claim (narrow, numbers): TimeGraph KDD'25 Table 2 (n=500/lag-2): B1 nonlinear-polynomial Gaussian PCMCI+ TPR 0.00/FDR 1.00/SHD 10; C1 trend+seasonality Gaussian TPR 0.00/FDR 1.00; linear A1 Gaussian TPR 1.00/FDR 0.00. All four tested methods fail B1/C1 together.
- Domain 1 (primary): Ferdous et al., TimeGraph, KDD'25, full text fetched (v1), open generation scripts.
- Domain 2: tigramite maintainer docs (verified) — "assuming stationarity, links are repeated in time"; plus A-S7 theory section declining PCMCI for deterioration (full text Phase 1).
- Counter-search: H4-contra 5 queries (hierarchical-PCMCI mfg wins, RADICE, BARO, regime-restriction repair) — pressured Tripwire A/P3, confirmed no method dominates drifted regimes, left the B1/C1 table intact.
- Scope limit: locks the *regime split is real*, not that the Jaccard complement wins (single-source A-S7 + battery pending → Unresolved).

### L4 — Rate-budget operating points + veto-gating are practiced precision mechanisms (practice existence, not the 10/1000 constant)
- Claim (narrow): detectors ship with calibrated threshold-sensitivity analyses reporting achievable TPR/FPR/alarm rates, and field Precision mechanisms (spike-vs-trend veto, analyst gate) demote high-sensitivity detections.
- Domain 1 (primary): Khan et al., Sci Rep 2026 (L2, full text) — Platt scaling ECE ≈0.03, operating points via threshold-sensitivity analyses reporting alarms-per-hour.
- Domain 2: Augury Jun-2026 practitioner blog (L4, full text) — 50+ detections on one compressor suppressed in a year (all process fluctuation), alarm sent only when trend-confirmed; plus EEMUA 191 ≈1 alarm/10 min normative anchor (I23/I24).
- Counter-search: H3-contra 5 queries — falsified the *10/1000 constant* as plant-grounded (see Refuted R3), not the practice of rate-budgeting itself.
- Scope limit: does NOT lock any numeric budget; per-machine/worst-zone recording (S-H3-02 direction) is Unresolved (L5 single source).

## Unresolved (claim + missing leg)

- U1 — CMC neuro-symbolic ablation shares (multi-task 24.4%, positional 20.3%, physics 11.7%): Table 6 numbers snippet-only, UNVERIFIED → missing leg: full-text verification (gap G-H1-b). At most directional.
- U2 — Multiband envelope integration (Duan Sensors 18:1466) + optimal sub-band AE selection (Sensors 18:1389): both snippet-only (MDPI 403) → missing leg: open-access full-text fetch (gap G-H2-a).
- U3 — ARKurtogram/STAKgram robust sub-band direction (2024): excerpts only → missing leg: full-text confirm (gap G-H2-b).
- U4 — Hierarchical-PCMCI manufacturing win (PHM-Xi'an 2025): snippet-only → missing leg: full evidence (gap G-H4-b). Keep-PCMCI-for-abrupt half stays directional.
- U5 — causRCA Table 3/7 numbers (PC/FCI 0.29–0.40 vs PCMCI 0.16–0.29; MAP@3 0.37–0.87 vs 0.21–0.56): repo verified, paper numbers snippet-only (PDF unopened) → missing leg: HTML/abstract-page confirm (gap G-H4-a).
- U6 — Second independent graph-distance-for-drift source beyond A-S7: not found → missing leg: new source (gap G-H4-c). Complement win rests on A-S7 alone.
- U7 — Uncalibrated-sensor NN harm paper (Springer s00521-021-06865-z): snippet-only (JS-blocked) → missing leg: open-copy confirm (gap G-H5-a).
- U8 — M2D2 drift-flag → threshold-adjust loop: snippet-only (MDPI 403) → missing leg: full evidence (gap G-H5-b). M2AD auto-calibration (below) does not substitute: different system.
- U9 — PMSM LightGBM feature-group ablation magnitudes (doi 10.65455/30tg2482): DOI stub only, unfamiliar venue (L4) → missing leg: full-text + venue check (gap G-H1-d). Method precedent only.
- U10 — 299-normals-for-1%-FPR rule (Deng 2608.15090): abstract verified, preprint L4, single vision-AD domain → missing leg: second domain + full-text methods check. Sample-planning logic transfers; constants do not.
- U11 — M2AD GMM+Gamma auto-calibration (+21%, 130 Amazon assets): abstract verified only → missing leg: full-text methods + second independent auto-calibration source. Direction (calibration can be automated) stands; magnitudes don't lock.
- U12 — Zero-shot single-threshold transfer conjunction (C-H5-1: zenodo record browser-walled UNVERIFIED + OpenCSI/ACR/bearing snippets): → missing leg: verified full-text of ≥1 channel-porting study in the twin's regime. Falsification pressure on manual-spec reading stands as provisional only.
- U13 — Sensors-21:6678 downsample-cost-small claim (H2-contra C-H2-2): fetch 403, snippet-only UNVERIFIED → missing leg: full-text confirm. Capped at directional; excluded from any lock.
- U14 — Per-machine worst-zone alert-rate prescription (S-H3-02, L5 anonymous essay): single L5 source → missing leg: second independent (≥L4) source + measured follow-through data. Battery-design recommendation only.
- U15 — A-S8b minimum-sensor-redundancy framework (TCSME 2017): landing page only, PDF never opened → missing leg: full-text method verification. Excluded from locks.

## Refuted (provisional, literature-level — battery tripwire unrun in all cases)

- R1 — H2 envelope-separation leg (≥15% via 1/step synthetic impulse term): met by H2-contra C-H2-1 (APC 0.0167 Hz research dataset; ISO-10816 1-min-RMS practice; RMS-roughness sufficiency) + C-H2-2 (non-monotonic rate arms; JMSE <4pp per 100× downsample) + C-H2-3 (real envelope needs 2–10 kHz resonance the tier disclaims) + C-H2-4 (single-band under-capture; ball-defect envelope exception). F1 leg (no ≥3pp drop) NOT refuted — no harm study (C-H2-5 MISS). Surviving reading: windows-only arm (H2-A2) or resonance-honest tier.
- R2 — H3 10-alerts/1000-windows bar as plant-grounded: met by H3-contra C-H3-1 (plant tolerance 10–15% ordinary / 3–5% override ceiling; 10/1000 = 1% sits 3–15× below) + C-H3-2 (steady-state EEMUA compliance already common; live problem is upset-peak, unmeasured by healthy replay) + C-H3-3 (ranking agreement/noise base rates) + C-H3-4 (deployed 1%/1e-5–1e-4 FPR systems clear 10/1000 trivially) + C-H3-5 (post-hoc precision-bar suboptimality, PMLR 151). Surviving reading: 20–150/1000 plant-grounded range with 5/20/150 stability check.
- R3 — H5 manual per-channel spec as ship-blocker: met by H5-contra C-H5-1 (zero-shot single-threshold transfer) + C-H5-2 (normalised-score single-cutoff tooling, 4 implementations) + C-H5-3 (M2AD automatic GMM+Gamma cross-sensor calibration) + C-H5-4 (domain randomisation + optional small-sample calibration). Surviving reading: auto-calibration rule (every channel ships an automatically derived operating point + noise characterisation); decisive test is the third global/auto arm (H5-A2).
- R4 — H1 unconditional-depth reading ("add all physics"): met by H1-contra C-H1-1 (JIM-2024 low-fi parity) + C-H1-2 (Truong CoRL23 overfitting sign-reversal) + C-H1-4 (uniform-HFM deterioration, Machines 14(5):480). The ablation-gate rule itself is NOT refuted — C-H1-4 restates it. Per-term verdicts pending battery.

## Field-level citation check (title / authors / venue / year / DOI from in-file record; no new retrieval)

Method: checked against bibliographic fields recorded in source-table.md + support/contra files. R = all recorded fields self-consistent + full-text fetched in this programme; P = partial (snippet/abstract-only, or venue/identifier anomaly — flag for re-verification); H = no match (remove). No H found — no invented sources detected.

| Source | Fields present | Label | Disposition |
|---|---|---|---|
| nature SciRep s41598-026-48227-6 (Khan et al., Sci Rep 16:17488, 2026) | title/authors/venue/year/DOI consistent, full text fetched | R | keep |
| TimeGraph KDD'25 (Ferdous et al., arXiv:2506.01361v2, KDD'25) | title/authors/venue/year/ID consistent, full text fetched | R | keep |
| GES2N (Schmidt/Wilke/Gryllias, arXiv:2405.00727v2) | title/authors/venue/year/ID consistent, full text fetched | R | keep |
| tigramite docs + repo (Runge et al., maintainer-authored) | venue/authorship consistent, fetched | R | keep |
| A-S7 Sensors 2024 PMC11207435 | Phase-1 full text, consistent | R | keep |
| A-S4 IEEE TII (ar5iv 2011.06296) | Phase-1 full text, consistent | R | keep |
| A-S5 JIM 2024 (ideas.repec joinma) | abstract fetched, consistent | R | keep (contra use) |
| Truong CoRL23 PMLR 205:859–870 | full page fetched + verified | R | keep (contra use) |
| M2AD PMLR 258:4384–4392 AISTATS 2025 | abstract page verified only | P | flag — re-verify full text before magnitude use |
| Deng 2608.15090 (299 normals) | abs page verified only, preprint | P | flag — re-verify full text + second domain |
| causRCA Procedia CIRP 139:114–120 (2026) | repo verified; paper numbers snippet-only | P | flag — re-verify Table 3/7 |
| CMC Alzaben et al. 88(3) 2026 (techscience 68140) | landing verified; Table 6 snippet-only; low-tier venue | P | flag — re-verify full text |
| Hierarchical PCMCI PHM-Xi'an 2025 (doi 10.1109/phm-xian66756.2025.11427842) | snippet-only | P | flag — re-verify |
| Uncalibrated-sensor Springer s00521-021-06865-z | snippet-only (JS-blocked) | P | flag — re-verify open copy |
| M2D2 MDPI 15(12):6500 | snippet-only (403) | P | flag — re-verify |
| Duan Sensors 18:1466 / 18:1389 sub-band | snippet-only (403) | P | flag — re-verify open copies |
| PMSM Gao doi 10.65455/30tg2482 | stub only; nonstandard DOI prefix | P | flag — re-verify venue + full text |
| ARKurtogram/STAKgram 2024 strands | excerpts only | P | flag — re-verify |
| Augury Jun-2026 blog; 21Tech Apr-2026; dev.to AssetTech; halkwinds 2026; smartindunews | full text fetched; L4/L5 channels | P | keep for mechanism/practice only — never for magnitudes |
| Sensors-21:6678 pipeline; zenodo-18682497 | walled (403 / browser wall), snippet-only | P | flag — excluded from locks |
| A-S8b TCSME 2017 (PDF) | landing only, method UNVERIFIED | P | flag — excluded from locks |

No source labelled H. Nothing removed.

## Falsification coverage check

grep over contradicting/: H1-contra.md, H2-contra.md, H3-contra.md, H4-contra.md, H5-contra.md each record ≥1 falsification attempt (5 queries each, verdicts: H1 not falsified/pressured; H2 separation-leg provisional; H3 provisional; H4 not falsified/contested; H5 manual-spec provisional). Coverage: 5/5 ✓.

## Status delta for hypothesis-registry (applied)

H1 Active → Contested (supporting L1 exists; contra pressure C-H1-1..4 unresolved; battery decisive). H2 Active → Contested (separation leg provisionally refuted R1; F1 leg intact). H3 Active → Contested (10/1000 reading provisionally refuted R2; 20–150/1000 range survives). H4 Active → Contested (L3 locks regime split only; P3 graph-free-control riskiest; complement win unresolved U6). H5 Active → Contested (manual-spec reading provisionally refuted R3; auto-calibration reading survives U11).

*End — 4 Locked (narrow), 15 Unresolved, 4 Refuted-provisional; 0 hallucinated.*
