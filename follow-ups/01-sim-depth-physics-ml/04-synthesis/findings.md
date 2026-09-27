# Findings — sim-depth-physics-ml (follow-up 01 synthesis)

Date: 2026-09-13 · Scope: 01-background + 02-hypotheses (H1–H5, all Contested) + 03-evidence (supporting + contradicting + evidence-log + claim-lock annex L1–L4 / U1–U15 / R1–R4).
Rule honored (SELF-ENFORCING): ONLY Locked L1–L4 claims appear below as established; Unresolved U1–U15 stay open (named as gaps, never as facts); refuted-provisional universals (R1–R4) appear ONLY as refuted with their narrowed surviving readings. No unlocked twin numeric (raw-F1 ~0.73-style, AC@1, flip-gate, envelope ≥15%) is presented as established — all such numbers stay conditional on the still-unrun frozen M0b battery.
Confidence rubric (strict): High ≥85% requires L1–L2 support (band stated strictly inside ≥85); Moderate 70–85% requires L3 + survived falsification; below = Contested with NO number. Parent findings F1–F6: extended, never contradicted (tension check per finding).

---

## F7 [CONTESTED — H1 verdict] No per-term physics addition is established; the ablation gate itself stands as method, not result

**Claim:** Whether any specific new term (thermal lag, drive-current coupling, wear-knee scalar, wear-coupled impulse) moves frozen-battery raw point-wise detection-F1 by ≥3pp or ranked-trace AC@1 by ≥10pp is unresolved. The per-term ablation-gate rule (add term → same-seed ablated-vs-full run → below both deltas = cut) is the prescribed decision method, not an established outcome. The unconditional-depth reading ("add all physics") is provisionally refuted (annex R4); the gate rule itself is NOT refuted.

**Confidence:** Contested, no number (supporting L1 residual-gain existence vs contra low-fi parity + sign-reversal + uniform-depth deterioration; no battery ever ran, tripwire empirically unfired).

**Evidence strength:** GRADE Moderate — 9 items: supporting 5 (S-H1-01 Khan Sci Rep 2026 L2 verified; S-H1-02 CMC ablation L2 snippet-only UNVERIFIED; S-H1-03 ITSCF thermal-current L2 snippet-only; S-H1-04 PMSM ablation L4 stub-only; S-H1-05 Halkwinds hybrid L4 verified) + Phase-1 precedent rows (A-S11 drive ODE L3; A-S9 thermal L2 snippet; A-S6 wear-knee L2 full text; A-S4 Siamese-AE FPR ~9%/FNR ~12% L2) vs contra 4 (C-H1-1 JIM-2024 low-fi parity L2; C-H1-2 Truong CoRL23 sign-reversal verified; C-H1-3 six-source data-gap convergence L5; C-H1-4 selective-multi-fidelity Machines 14(5):480). No same-seed ablation battery exists in the retrieved set.

**Key sources:** https://www.nature.com/articles/s41598-026-48227-6 · https://ideas.repec.org/a/spr/joinma/v35y2024i5d10.1007_s10845-023-02144-x.html · CoRL23 PMLR 205:859–870 (Truong, verified full page) · https://www.halkwinds.com/research/digital-twin-enterprise-adoption-report · https://doi.org/10.48550/arxiv.2208.01408 (A-S14) · https://simpy.readthedocs.io/en/stable/topical_guides/time_and_scheduling.html (A-S15)

**Remaining uncertainty:** Which terms pass (predicted: wear-knee or current coupling) vs fail (predicted: thermal lag or raw impulse) — H1-P1 is a prediction, not a result. Whether stability/credit belongs to term vs seeds/mask/calibration (A1) is unseparated until the ablation fixes seeds/mask/Q_DET across arms. U1 (CMC Table 6 magnitudes), U9 (PMSM magnitudes) are the open verification legs.

**Alternative explanations:** (1) Fidelity ≠ detection (A-S5/C-H1-1): low-fi already matches high-fi; any observed gain comes from seed/mask/calibration choice, not the term — distinguished by holding seeds/mask/calibration fixed with only the term toggling; if Δ vanishes under seed-sweep, this wins. (2) Gain is fault-mix-specific (e.g. only T3 wear faults) — distinguished by per-class Δ reporting; a single-class pass scopes the term to that class, not general depth. (3) Uniform-depth deterioration (C-H1-4): adding all terms jointly degrades real-time/detection performance — distinguished by per-term (not bundled) verdicts.

**Would-be-overturned-by:** Frozen M0b battery (20–32 seeded faults, quantile detector fixed, PA-off, T=300/CAL_WIN=120, all seeds counted): any retained term with ΔF1_raw < 0.03 AND ΔAC@1 < 0.10 vs its ablated arm falsifies H1 for that term. Numeric tripwires: full-twin wall ≥600s or any diverge across 5 seeded replays fails the term regardless of metric gain; sign-stable replication across ≥5 seeds required (P2).

**Parent consistency:** Extends F5 (M0b battery owed) with the per-term gate design; no tension with Locked F1 (raw-F1-only scoring), F2 (no platform/ROI framing — verdicts battery-scoped), F3 (detection/traceback legs only). Reinforces F4's conditional posture (mask-conditionality preserved in ablation arms).

---

## F8 [CONTESTED — H2 verdict] Tiered impulse term provisionally refuted on the separation leg; windows-only arm survives as the live alternative

**Claim:** The ≥15% envelope-band energy-separation gain via the 1/step wear-coupled synthetic impulse term is provisionally refuted (annex R1): low-rate sufficiency pressure from below (RMS already carries the signal) + real-envelope bandwidth requirement (2–10 kHz resonance) from above leave no precedent for a 1/step synthetic impulse delivering ≥15%. The conditioned-F1 leg (no ≥3pp drop) is NOT refuted — no harm study found. Surviving reading: windows-only arm (multi-scale export without the synthetic impulse term) or a resonance-honest tier; the no-bearing-frequencies claim boundary holds either way.

**Confidence:** Contested, no number (separation leg provisionally falsified across 5 contra queries; F1 leg intact; metric-validity leg separately established in F12).

**Evidence strength:** GRADE Moderate — 8 supporting-side items (S-H2-01 GES2N envelope-SNR L4 verified full text; S-H2-02/03 multiband/sub-band L2 snippet-only U2; S-H2-04 kurtogram direction L2 snippets U3; cache A-S12 ≥8pp full-bandwidth, A-S2 L1 pipeline, A-S10 multi-scale windows, I03 burst/RMS tier) vs contra 4 (C-H2-1 APC 0.0167 Hz research dataset + ISO-10816 1-min-RMS practice + RMS-roughness sufficiency; C-H2-2 non-monotonic rate arms + JMSE <4pp per 100× downsample + U13 Sensors-21:6678 snippet-only; C-H2-3 real-envelope 2–10 kHz / 25.6 kS/s requirement + band-misplacement danger; C-H2-4 single-band under-capture + ball-defect envelope exception). C-H2-5 (harm literature) MISS — recorded, not suppressed.

**Key sources:** https://arxiv.org/html/2405.00727v2 (GES2N) · https://www.sciencedirect.com/science/article/abs/pii/S0888327015002897 (A-S2) · https://arxiv.org/html/2507.16696v2 (A-S12) · https://www.mdpi.com/2075-1702/14/2/164 (A-S10) · https://osicdn.blob.core.windows.net/learningcontent/pdfs/UC-2017-Hands-On-Lab---Incorporating-Condition-Monitoring-Data-Vibration,-Infrared-and-Acoutic-for-Condition-Based-Maintenance.pdf (I03)

**Remaining uncertainty:** Whether multi-scale windows alone explain the gain (H2-A2: three-way arms RMS-only / windows-only / windows+impulse decide). U2 (Duan multiband full text), U3 (ARKurtogram/STAKgram confirm), U13 (Sensors-21:6678 confirm) are open legs. The exact envelope-separation statistic (GES2N-style targeted SNR vs sub-band energy) for the battery acceptance test is undecided.

**Alternative explanations:** (1) Half-modelled harm (A-S4 §VII): partial impulses add noise without the fast physics they mimic; apparent separation gain is noise-fitting — distinguished by the conditioned-F1 leg on held-out seeds; passing separation + failing F1 = harm confirmed, H2 false. (2) Windows-only sufficiency (H2-A2): multi-scale export alone carries the gain — distinguished by the three-way arms; if windows-only matches tiered, the impulse term is cut and the win is scoped to windowing.

**Would-be-overturned-by (battery):** RMS-only vs tiered arms, identical seeds: envelope-separation gain <15% on seeded impulse faults OR ΔF1_tiered−base ≤ −0.03 on skew/kurtosis-conditioned faults (quantile, fixed-percentile, PA-off) → H2 false as written. Surviving-reading tripwire: if windows-only arm matches windows+impulse within ±3pp F1 and ±5pp separation, the impulse term is cut even if the tiered-vs-base comparison passes.

**Parent consistency:** No tension with Locked F1 (conditioned raw-F1, PA-off); extends F5's M0b-owed posture with the envelope-domain acceptance design. Claim boundary (no bearing frequencies) aligns with the industry RMS-grade historian rows (I03/I04/I08).

---

## F9 [CONTESTED — H3 verdict] The 10/1000 budget as plant-grounded is provisionally refuted; the rate-budget practice itself is established (see F13); surviving reading is a 20–150/1000 range with stability check

**Claim:** A precision-side bar demoting ≥1 top-quartile raw-F1 setting at a pre-registered budget of >10 alerts/1000 healthy windows is provisionally refuted AS A PLANT-GROUNDED CONSTANT (annex R2): 10/1000 (=1%) sits 3–15× below demonstrated plant tolerance bands, measures the already-solved steady-state regime, and faces ranking-agreement base rates. Surviving reading: test demotion across a plant-grounded 20–150/1000 range with a 5/20/150 stability check; per-machine/worst-zone recording (S-H3-02 direction, U14) as battery-design recommendation. The practice of rate-budgeting + veto-gating is separately established (F13) — only the constant fell.

**Confidence:** Contested, no number for any demotion prediction (4 of 5 contra query lines convergent; battery unrun; budget-range stability untested).

**Evidence strength:** GRADE Moderate — supporting 4 (S-H3-01 Augury 50+-suppressed veto-gate L4 verified; S-H3-02 follow-through/worst-zone L5 mechanism-grade; S-H3-03 87k-alert flood L5 magnitudes unaudited; S-H3-04 Khan threshold-sensitivity + alarms-per-hour L2 verified; cache I23/I24 EEMUA, C05 postmortem, I12 sensor-health flags, A-S4 9%/12% ceiling) vs contra 5 (C-H3-1 oxmaint 10–15% tolerance / 3–5% override ceiling; C-H3-2 EEMUA steady-state compliance common, upset-peak unmeasured; C-H3-3 BDCC identical ranks + ±0.34 ranking noise; C-H3-4 deployed 1%/1e-5–1e-4 FPR systems; C-H3-5 PMLR-151 post-hoc-precision suboptimality).

**Key sources:** https://www.augury.com/blog/machine-health/why-your-team-has-stopped-trusting-their-predictive-maintenance-alerts/ · https://21tech.com/your-predictive-maintenance-platform-generates-100000-alerts-a-day-your-team-reads-12/ · https://www.nature.com/articles/s41598-026-48227-6 · https://www.eemua.org/products/publications/print/eemua-publication-191 (I23, landing) · https://www.nebulaworks.com/insights/posts/predictive-maintenance-18-months-production/ (I12)

**Remaining uncertainty:** Whether ANY top-quartile-F1 setting breaches even the relaxed 20–150/1000 range (tripwire may fire trivially if detectors already clear it — C-H3-4 direction). Whether healthy segments must include start-up/changeover transients for the rate to be meaningful (H3-A2). U14 (per-machine worst-zone prescription, single L5 source) is the open design leg. U10 (299-normals rule, single vision-AD domain) does not transfer its constants.

**Alternative explanations:** (1) Budget miscalibration (H3-A1): demotion is an artifact of the threshold, not plant trust — distinguished by the pre-registered range + 5/20/150 stability check; demotion surviving ≥2 of 3 budgets = real rank-reversal. (2) Healthy segments too clean (H3-A2): rate underestimated without transients — distinguished by transient-inclusive healthy pool; demotions appearing only with transients scope the bar to that replay. (3) Ranking agreement (C-H3-3): sensitivity and trust rankings agree, so the tripwire fires and H3 is false — a legitimate falsification, not a design flaw.

**Would-be-overturned-by (battery):** M0b + fault-free healthy-window replay: zero top-quartile-F1 settings exceeding the budget at 20/1000 AND 150/1000 (range edges) → H3 false. Numeric tripwires: demotion present at only one of {5, 20, 150}/1000 → threshold-artifact, demotion rejected; VETO_ASM2 + flip-gate failing to cut the healthy-window rate vs veto-off control → precision-mechanism leg revoked (H3-P2 fails).

**Parent consistency:** Extends F5 (twin-scoped replacement bars owed) with the corrected range; no tension with Locked F1 — the bar sits ALONGSIDE raw-F1, never replacing it; no PA-based ranking endorsed. Reinforces F2 (no platform/ROI framing — verdicts are battery-scoped trust mechanics, not value claims).

---

## F10 [CONTESTED — H4 verdict] The regime split is real (established, F13); the Jaccard-complement win is unresolved single-source; graph-free control is the highest-risk leg

**Claim:** Two separable sub-claims: (a) ESTABLISHED (carried in F13): masked PCMCI+ collapses on nonlinear/trend-seasonal regimes while holding linear-stationary — the regime split motivating a drift complement is real. (b) CONTESTED: the reference-graph Jaccard-distance complement recovers ≥50% AC@1 on wear-drift with ≥10pp gain and <10pp abrupt-subset regression — rests on single-source A-S7 + unrun battery (U6 open). Highest-risk leg: H4-P3 (graph-free BARO-style walk matching the complement on wear-drift, credit to ordering not graph distance). Hierarchical-PCMCI manufacturing win (U4) keeps the keep-PCMCI-for-abrupt half directional only.

**Confidence:** Contested, no number for any AC@1 prediction (complement win unresolved; Tripwires A/B/C unrun). The split-existence half carries F13's Moderate band, not repeated here.

**Evidence strength:** GRADE Moderate — supporting 4 (S-H4-01 causRCA repo verified, paper numbers snippet-only U5; S-H4-02 TimeGraph B1/C1 collapse L3 verified full text; S-H4-03 tigramite stationarity admission L3 verified; S-H4-04 hierarchical-PCMCI L3 snippet-only U4; cache A-S7 adapted-FCI+Jaccard L2 full text) vs contra 5 (C-H4-1 hierarchical-PCMCI manufacturing wins + RADICE PCMCI+ production RCA vs Tripwire A; C-H4-2 Jaccard field precedent recorded as supporting-side MISS + StaR unified-dynamic-graph vs the split; C-H4-3 IFAC-CPG/LSTE/COKE/CERN ordering-timing set for A1; C-H4-4 BARO + CD-RCA low-amplitude weakness vs Tripwire B; C-H4-5 regime-restriction/window-repair for A2). Falsification coverage 5/5 queries; genuinely contested verdict.

**Key sources:** https://arxiv.org/html/2506.01361v2 (TimeGraph KDD'25, Table 2) · https://github.com/causalgraph/causRCA (verified repo) · https://jakobrunge.github.io/tigramite/ (maintainer docs) · https://pmc.ncbi.nlm.nih.gov/articles/PMC11207435/ (A-S7) · https://proceedings.mlr.press/v124/runge20a.html (A-S1) · https://opensource.salesforce.com/PyRCA/latest/index.html (I28)

**Remaining uncertainty:** Whether PCMCI+-alone already reaches ≥50% on the twin's T3 wear-drift subset (Tripwire A — C-H4-1 says not a given either way). Whether multi-scale windowing alone repairs drift (A2 → fix belongs to H2, not a second method). U4 (hierarchical-PCMCI full evidence), U5 (causRCA Table 3/7 confirm), U6 (second independent graph-distance-for-drift source) are all open. Complement threshold mechanics (expert-set Jaccard threshold brittleness, C-H4-2) unexamined.

**Alternative explanations:** (1) Ordering/timing sufficiency (H4-A1): threshold-crossing order + delay timing traces wear chains without graph machinery — distinguished by the graph-free BARO-style control arm; if it matches the complement, A1 wins and the complement is cut. (2) Window/scale artifact (H4-A2): drift failure is short-timescale blindness, not discovery-regime failure — distinguished by a multi-scale-window variant of PCMCI+-alone; if it alone recovers drift, the fix is windowing (H2). (3) Unified dynamic graph (StaR direction, C-H4-2): one dynamic-graph method covers both regimes — distinguished by comparing split vs single-dynamic-graph cost/accuracy on the battery; split must justify two methods.

**Would-be-overturned-by (battery, three arms + graph-free control, fault-subset AC@1 depth≤3, all seeds):** Tripwire A: PCMCI+-alone wear-drift AC@1 ≥50% → split unnecessary → H4 false. Tripwire B: (split − PCMCI+-alone) wear-drift AC@1 <10pp → complement adds nothing → H4 false. Tripwire C: split abrupt-subset AC@1 drops ≥10pp vs PCMCI+-alone → split remedy rejected even if drift leg passes. P3 tripwire: graph-free control within ±5pp of complement on wear-drift → credit to ordering, complement cut.

**Parent consistency:** Pressures ONLY conditional F4 (wear-drift regime F4 already flags conditional); abrupt/propagating posture untouched. Converges with parent F3's BARO-ablation-control prescription (graph-free control = F5b's BARO-style arm). No tension with Locked F1–F3.

---

## F11 [CONTESTED — H5 verdict] Manual per-channel spec as ship-blocker provisionally refuted; auto-calibration rule survives as the narrowed reading

**Claim:** The manual-spec reading ("every new channel ships a hand-written Q_DET operating point + noise spec or it doesn't ship") is provisionally refuted (annex R3): zero-shot single-threshold transfer + normalised-score single-cutoff tooling + M2AD automatic GMM+Gamma cross-sensor calibration + randomisation+small-sample calibration converge on channels shipping WITHOUT hand-written specs. Surviving reading: auto-calibration rule — every channel ships an AUTOMATICALLY DERIVED operating point + noise characterisation (third global/auto arm, H5-A2, decisive). M2AD magnitudes (+21%, 130 assets) and the 299-normals constants do not lock (U10/U11).

**Confidence:** Contested, no number for any firing-failure prediction (manual reading provisionally falsified; auto reading untested on the battery).

**Evidence strength:** GRADE Moderate — supporting 6 (S-H5-01 Halkwinds calibration-TCO L4 verified; S-H5-02 299-normals L4 abstract-verified U10; S-H5-03 47/50 FP-cause L5; S-H5-04 minimum-viable-data-model L5; S-H5-05 uncalibrated-sensor NN harm L2 snippet-only U7; S-H5-06 M2D2 threshold-adjust loop L2 snippet-only U8; cache I12 sensor-health flags, C02/C04 back-loaded ROI) vs contra 4 (C-H5-1 zero-shot set incl. OpenCSI single-threshold F1 0.99; C-H5-2 tsanomaly/GDI/signalmap/safeband single-cutoff tooling; C-H5-3 M2AD auto-GMM+Gamma verified abstract; C-H5-4 DR/DROPO/RAPT optional-calibration). C-H5-5 MISS (no ported-threshold industrial-channel study) — recorded.

**Key sources:** https://www.halkwinds.com/research/digital-twin-enterprise-adoption-report · https://arxiv.org/abs/2608.15090 (Deng 299-normals, abs verified) · OpenCSI pubdb 2607.26665 (single-threshold transfer) · M2AD PMLR 258:4384–4392 (AISTATS 2025, abstract verified) · https://21tech.com/your-predictive-maintenance-platform-generates-100000-alerts-a-day-your-team-reads-12/ · https://www.nebulaworks.com/insights/posts/predictive-maintenance-18-months-production/ (I12)

**Remaining uncertainty:** Whether the twin's uncalibrated-new-channel arm actually trips either leg (alert flood or ≥5pp F1 drop) with FAULT_RANGES fixed — no controlled calibrated-vs-uncalibrated experiment exists in the set. Whether a global/auto arm matches per-channel hand calibration (A2 — decisive for scoping the rule down). U7 (uncalibrated-sensor open copy), U8 (M2D2 full evidence), U10/U11 (sample-planning + auto-calibration full-text + second source), U12 (≥1 verified channel-porting study) are open.

**Alternative explanations:** (1) Fault-mag confounding (H5-A1): mis-fire comes from 4–7σ sensitivity tuning, not missing calibration — distinguished by holding FAULT_RANGES fixed across arms; if varying mags erases the gap, A1 wins. (2) Global-threshold sufficiency (H5-A2): one normalised cutoff matches per-channel specs — distinguished by the third global/auto arm; a match scopes the rule down to global Q_DET + auto-calibration. (3) Randomisation-substitution (C-H5-4): FAULT_RANGES domain randomisation + tiny nominal-sample calibration holds both legs with zero marginal manual cost — distinguished by adding the randomisation+auto-cal arm to the battery.

**Would-be-overturned-by (battery, calibrated vs uncalibrated-new-channel arms + third global/auto arm, identical seeds/faults/mags):** Uncalibrated arm ≤10 alerts/1000 healthy windows AND ΔF1_uncal−cal ≥ −0.05 → H5 false (calibration dispensable). Scoping tripwire: global/auto arm within ±2pp F1 AND within ±3 alerts/1000 of per-channel-calibrated → rule scopes down to global/auto (manual-spec leg revoked even if the firing gap exists).

**Parent consistency:** No tension with Locked F2 (calibration labor is cost-ledger context, not a platform/ROI value claim). Ties to H3's precision bar (firing failure measured in alerts/1000 + raw-F1, both Locked-F1-compliant instruments). Extends F5's M0b-owed posture with the three-arm battery design.

---

## F12 [ESTABLISHED — Locked L1+L2] Physics-residual channels move detection numbers; envelope-band separation is the licensed acceptance statistic (existence + instrument, NOT twin transfer)

**Claim (narrow, two halves):** (a) Adding a physics-model residual channel (measured − nominal) + calibrated operating points lifts detection over raw-signal SOTA families on the same benchmark (existence demo, TEP scope). (b) Targeted envelope-spectrum SNR / sub-band energy separation is the community's working acceptance statistic for impulsive faults (metric validity). NEITHER half locks that any specific twin term clears ΔF1 ≥3pp, or that the twin's 1/step impulse delivers ≥15% — those legs are F7/F8-contested.

**Confidence:** High, 85–88% for the narrowed conjunction (two independent domains per half + counter-search; L1–L2 base: Khan Sci Rep L2 + A-S4 IEEE TII L2 for (a); A-S2 MSSP review L1 + A-S12 19-dataset result for (b)).

**Evidence strength:** GRADE High — 6 sources: Khan et al. Sci Rep 16:17488 (2026, full text: macro F1 0.93±0.02 vs data-only plateau ≈0.907–0.917, NAB +17% over Isolation Forest, ECE ≈0.03, alarms-per-hour operating points); A-S4 (IEEE TII: Siamese AE FPR ~9%/FNR ~12% beating SOTA unsupervised); GES2N arXiv:2405.00727v2 (envelope-SNR objectives beat CYCBD/ACYCBD/MOMEDA/envelope-norms/negentropy on 3 gearbox datasets); A-S2 (SK + kurtogram + envelope standard pipeline); A-S12 (full bandwidth beats downsampled ≥8pp, 19 datasets); counter-search H1-contra (low-fi parity, sign-reversal) pressured predicted pass terms only, not the residual-gain existence demo; H2-contra attacked synthetic-impulse transfer (C-H2-3), not the metric.

**Key sources:** https://www.nature.com/articles/s41598-026-48227-6 · https://ar5iv.labs.arxiv.org/html/2011.06296 (A-S4) · https://arxiv.org/html/2405.00727v2 (GES2N) · https://www.sciencedirect.com/science/article/abs/pii/S0888327015002897 (A-S2) · https://arxiv.org/html/2507.16696v2 (A-S12)

**Remaining uncertainty:** Transfer gap: TEP/gearbox/kHz regimes → twin's 1/step RMS + synthetic impulse is a coarse analogue; the twin-transfer legs live in F7/F8. GES2N as-fetched is preprint trajectory (L4) — metric-validity weight rests on the A-S2 L1 review + multi-source convergence, not GES2N alone.

**Alternative explanations:** (1) Residual gain is benchmark-specific (TEP/CHP idiosyncrasy) — countered by two independent facilities + mechanisms (TEP residuals, CHP Siamese-AE, gearbox envelope objectives) converging on the same direction. (2) Envelope-metric wins come from filter optimisation machinery (SciPy CG in GES2N), not the envelope statistic per se — scoped honestly: F12 licenses the instrument, not any particular optimiser; the battery's acceptance test must fix its statistic pre-registration.

**Would-be-overturned-by:** A same-benchmark replication showing residual-enriched vs raw-signal SOTA families with Δmacro-F1 ≤ 0 (n≥3 seeds, fixed protocol) → half (a) false; OR a peer-reviewed benchmark where targeted envelope-SNR underperforms full-waveform/raw baselines by ≥5pp on ≥2 impulsive-fault datasets → half (b) false. Numeric tripwires: residual macro-F1 ≤ 0.917 (at/below data-only plateau) with ECE > 0.10; envelope-SNR M1–M4 all negative vs best baseline.

**Parent consistency:** Reinforces F4's mechanism base (physics-informed features) without touching its conditional numerics; consistent with F1 (all cited numbers are raw/point-wise or NAB/ECE operating-point metrics — no PA-based ranking); no platform framing (F2).

---

## F13 [ESTABLISHED — Locked L3+L4] PCMCI+ drift-collapse is a benchmark fact; rate-budget + veto-gating is practiced precision mechanics (NOT the 10/1000 constant, NOT the complement win)

**Claim (narrow, two halves):** (a) PCMCI+ collapses on nonlinear/trend-seasonal regimes while holding linear-stationary: TimeGraph KDD'25 Table 2 (n=500/lag-2) B1 nonlinear-polynomial Gaussian TPR 0.00/FDR 1.00/SHD 10; C1 trend+seasonality Gaussian TPR 0.00/FDR 1.00; linear A1 Gaussian TPR 1.00/FDR 0.00 (all four tested methods fail B1/C1 together). (b) Detectors ship with calibrated threshold-sensitivity analyses reporting achievable TPR/FPR/alarm rates, and field precision mechanisms (spike-vs-trend veto, analyst gate) demote high-sensitivity detections. NEITHER half locks the Jaccard-complement win (U6, F10-contested) nor any numeric budget (R2-refuted constant, F9-contested range).

**Confidence:** Moderate, 72–80% for the conjunction (L3 base: TimeGraph KDD'25 peer-reviewed conference + tigramite maintainer docs + EEMUA normative anchor; survived 5-query falsification per half with tables/practice intact).

**Evidence strength:** GRADE Moderate-High — 7 sources: TimeGraph KDD'25 (full text v1, open generation scripts); tigramite docs + repo ("assuming stationarity, links are repeated in time"; masked-series + LPCMCI/RPCMCI support); A-S7 theory section (declining PCMCI for deterioration, Phase-1 full text); Khan Sci Rep 2026 (Platt ECE ≈0.03, alarms-per-hour operating points); Augury Jun-2026 (50+ detections suppressed on one compressor, trend-confirmation gate); EEMUA 191 ≈1 alarm/10 min anchor (I23/I24, landing + secondary); H4-contra confirmed no method dominates drifted regimes (leaving B1/C1 intact); H3-contra falsified the constant, not the practice.

**Key sources:** https://arxiv.org/html/2506.01361v2 (TimeGraph) · https://jakobrunge.github.io/tigramite/ · https://pmc.ncbi.nlm.nih.gov/articles/PMC11207435/ (A-S7) · https://www.nature.com/articles/s41598-026-48227-6 · https://www.augury.com/blog/machine-health/why-your-team-has-stopped-trusting-their-predictive-maintenance-alerts/ · https://www.eemua.org/products/publications/print/eemua-publication-191

**Remaining uncertainty:** Whether B1/C1 table values replicate at the twin's exact regime (wear-knee nonstationarity, n≥800, tau=2, masked) — benchmark fact, not twin measurement. Whether veto-gate efficacy transfers from analyst-adjudicated (Augury) to twin-automated (VETO_ASM2 + flip-gate) gates — mechanism precedent, not transfer proof. U14 (per-machine worst-zone recording) open.

**Alternative explanations:** (1) Collapse is a sample-size artifact (n=500 too small for B1/C1) — countered by A1 holding TPR 1.00 at identical n, and all four methods failing together (regime, not budget). (2) Veto-gating demotions reflect analyst conservatism (process-fluctuation adjudication), not detector quality — scoped honestly: F13 locks the practice existence, and the battery's veto-off control arm (H3-P2) measures the automated transfer.

**Would-be-overturned-by:** Independent replication of TimeGraph B1/C1 with PCMCI+ TPR ≥ 0.50 on either variant (same n=500/lag-2 protocol, open scripts) → half (a) false. Numeric tripwires: B1 SHD ≤ 4 with FDR ≤ 0.50 → collapse revoked. For (b): a field study across ≥10 plants showing zero veto/gating mechanisms in production detectors (all raw-threshold ship) → practice leg false; tripwire: veto/gate prevalence <10% of surveyed deployments.

**Parent consistency:** Half (a) compounds parent F4's prior-fragility flag (wear-drift is exactly the conditional regime) without overturning F4's abrupt-leg posture. Half (b) extends parent F6's wedge (trace-explainability as decision interface) with practiced precision mechanics. No tension with Locked F1–F3.

---

*End — 7 findings: F7–F11 contested (H1–H5 verdicts; refuted universals shown ONLY as refuted with surviving readings R1–R4 of the annex), F12–F13 established (Locked L1–L4 narrow). Every finding carries Confidence + Evidence GRADE/count + Key sources + Remaining uncertainty + ≥1 Alternative + numeric overturn tripwire.*
