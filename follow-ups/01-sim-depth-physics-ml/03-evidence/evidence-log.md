# Evidence log — supporting lane (H1–H5)

Date: 2026-09-13 · Run: supporting-evidence pass · Lanes: supporting only (contradicting owned by teammate — no contradicting search performed).
TEXT-ONLY throughout. Fetch cap: 17/25 used. No hypothesis marked corroborated (synthesis decides).

## 1. Relevance gate (per-query: "does this query directly target H?")
| # | Query | Target | Verdict |
|---|---|---|---|
| Q1 | digital twin ablation physics-based features improve fault detection | H1 | PASS — ablation × physics × detection numbers |
| Q2 | multi-scale window envelope vibration impulsive fault kurtosis | H2 | PASS — sub-band/envelope wins |
| Q3 | causal discovery gradual drift PCMCI FCI comparison | H4 | PASS — drift-regime method split |
| Q4 | EEMUA 191 alarm rate flood operator trust | H3 | PASS — precision-bar normative anchor |
| Q5 | "85% accurate" technicians trust precision recall fatigue | H3 | PASS — trust/fatigue mechanism |
| Q6 | sensor calibration threshold per-channel false alarm drift | H5 | PASS — calibration firing failure |
| Q7 | motor current thermal model ablation fault diagnosis | H1 | PASS — current/thermal term precedent |
| Q8 | PCMCI root cause manufacturing benchmark Tigramite PyRCA | H4 | PASS — abrupt-leg + benchmark |
| Q9 | digital twin ROI back-loaded calibration cost year two | H5 | PASS — calibration-cost ledger |
| Q10 | arxiv envelope sub-band bearing full bandwidth | H2 | PASS (rewrite of Q2 toward fetchable arXiv full text after MDPI 403s) |
| Dropped/rewritten | — | — | No query dropped. Q10 rewritten, not dropped: Q2's MDPI hits returned 403 on fetch, so Q10 re-targeted the same H2 need toward arXiv HTML. Logged here, not silently. |

## 2. Cascade order per source (cache → direct URL/DOI → scholar connectors → web search)
- Cache hits reused without re-fetch (recorded as cache): A-S2/A-S4/A-S6/A-S9/A-S10/A-S11/A-S12/A-S14/A-S15, I03/I04/I08/I12/I23/I24/I28, C02/C04/C05/C06.
- Direct URL/DOI fetches (17): techscience landing ✓, arxiv TimeGraph html ✓, arxiv 2608.15090 abs ✓, augury ✓, dev.to operator-trust ✓, causRCA repo ✓, nature SciRep ✓, halkwinds ✓, 21tech-100k ✓, arxiv GES2N html ✓, tigramite docs ✓, smartindunews ✓, mdpi SEAD ✗403, mdpi Energies ✗403, mdpi Sensors-1466 stub-only, doi PMSM stub-only, springer JS-blocked.
- Scholar-connector layer: exa-backed searxng academic/tech search (10 searches, Q1–Q10 minus web ones).
- Web-search layer: Q4/Q5/Q9 + fetches of trade/practitioner pages.

## 3. Per-source verdicts
| Source | H | Verdict | Note |
|---|---|---|---|
| nature SciRep s41598-026-48227-6 | H1,H3 | HIT (verified full text) | L2; physics residuals + calibrated rate-budget |
| techscience CMC 68140 landing | H1 | HIT-partial | abstract verified; Table-6 ablation nos. snippet-only → UNVERIFIED |
| IOP ITSCF thermal DT | H1 | UNVERIFIABLE (snippet-only) | landing excerpt; full text not fetched |
| doi 10.65455/30tg2482 (PMSM ablation) | H1 | UNVERIFIABLE (snippet-only) | DOI resolves stub only |
| halkwinds adoption report | H1,H5 | HIT (verified full text) | L4; hybrid win + calibration TCO (self-declared non-survey) |
| arxiv GES2N 2405.00727 | H2 | HIT (verified full text) | L4; envelope-SNR metric validation |
| Sensors 18:1466 multiband | H2 | UNVERIFIABLE (snippet-only) | MDPI 403 |
| Sensors 18:1389 sub-band AE | H2 | UNVERIFIABLE (snippet-only) | MDPI 403 |
| ARKurtogram / STAKgram | H2 | UNVERIFIABLE (snippet-only) | excerpts only |
| augury alarm fatigue | H3 | HIT (verified full text) | L4 vendor blog; veto-gate case |
| dev.to operator trust | H3 | HIT (verified full text) | L5 anon; mechanism-grade only |
| 21tech 100k alerts | H3,H5 | HIT (verified full text) | L5; flood numbers unaudited |
| causRCA repo | H4 | HIT (verified repo) | dataset+harness verified; paper score nos. snippet-only → UNVERIFIED |
| TimeGraph KDD'25 | H4 | HIT (verified full text) | L3; PCMCI+ collapse table verified |
| tigramite docs | H4 | HIT (verified) | L3; stationarity assumption + masked support |
| IEEE hierarchical PCMCI | H4 | UNVERIFIABLE (snippet-only) | excerpt only |
| arxiv 2608.15090 abs | H5 | HIT (verified abs page) | L4 preprint; 299-normal rule |
| smartindunews twin ROI | H5 | HIT (verified full text) | L5 trade |
| springer uncalibrated sensors | H5 | UNVERIFIABLE (snippet-only) | JS-blocked |
| MDPI M2D2 | H5 | UNVERIFIABLE (snippet-only) | MDPI 403 |
| mdpi SEAD / Energies (fetch attempts) | H1 | MISS (403) | recorded, not cited for magnitudes |

## 4. Gap-pass — still missing per sub-hypothesis + targeted follow-up queries
- G-H1-a (P1/P2): controlled toggle-one-term ablation with ΔF1/ΔAC@1 numbers. Q: "digital twin ablation remove physics model component F1 drop fault detection controlled comparison".
- G-H1-b: CMC Table 6 verification. Q: direct fetch https://www.techscience.com/cmc/v88n3/68140/html (1 fetch, no search needed).
- G-H1-c: thermal/current twin诊断 full-text numbers. Q: "digital twin current thermal fault diagnosis detection rate comparison site:iopscience.iop.org OR site:sciencedirect.com".
- G-H1-d: PMSM ablation magnitudes. Q: "PMSM LightGBM feature group ablation thermal electrical predictive maintenance Gao".
- G-H2-a: multiband/sub-band full-text (Duan 2018; 1389). Q: scholar title search for open-access copies (ResearchGate/semanticscholar abstract pages, TEXT-ONLY).
- G-H2-b: kurtogram full-text. Q: "ARKurtogram adaptive reweighted kurtogram bearing diagnosis abstract".
- G-H2-c: RMS-sufficiency for historian tier has only cache (I03/I04/I08) — no NEW source. Q: "historian RMS envelope sufficient bearing fault detection derived values case study".
- G-H3-a: peer-reviewed (≥L2) alert-fatigue/trust quantification — current support is L4/L5 + one L2 adjacent. Q: "alarm fatigue quantitative false alarm rate operator response manufacturing peer reviewed".
- G-H3-b: EEMUA-191-style rate compliance measurement study. Q: "EEMUA 191 benchmark audit alarm rate per 10 minutes compliance study plant".
- G-H4-a: causRCA Table 3/7 number confirmation. Q: direct fetch of Procedia CIRP abstract page or Zenodo record (no PDF).
- G-H4-b: hierarchical-PCMCI full evidence. Q: "hierarchical PCMCI root cause manufacturing IEEE PHM 2025".
- G-H4-c: second independent graph-change-distance-for-drift source beyond A-S7. Q: "graph edit distance degradation tracking root cause time series manufacturing".
- G-H4-d: masked-PCMCI abrupt-fault win with numbers (parent-F4-adjacent, NEW source). Q: "masked PCMCI prior knowledge root cause accuracy improvement ablation".
- G-H5-a: uncalibrated-sensor paper full evidence. Q: title search "Real-time detection of uncalibrated sensors using neural networks" open copy.
- G-H5-b: M2D2 threshold-adjust loop numbers. Q: "M2D2 multi-machine multi-modal drift detection semiconductor false alarm threshold adjust".
- G-H5-c: per-channel calibration cost/time quantification (hours/tags). Q: "predictive maintenance sensor calibration cost per tag commissioning hours study".

## 5. Guard compliance
Parent Locked F1–F3 not contradicted (all claims battery-scoped, raw-F1-only, no platform/ROI framing beyond H5's ledger context which F2 permits as cost, not value claim). No new hypotheses. Nothing written outside supporting/ + this log. No corroboration verdicts entered.

---

# Evidence log — falsification (devil's advocate) run

Date: 2026-09-13 · Run: contra-H1..H5 · Lanes: contradicting only (no supporting-search; supporting lane above untouched).
Method: TEXT ONLY; 25 search queries (5/hypothesis) + 4 fetches (2 verified: PMLR truong23a, PMLR alnegheimish25a; 1 HTTP-403 MDPI sensors-21-19-6678 → snippet-only UNVERIFIED; 1 browser-wall zenodo-18682497 → snippet-only UNVERIFIED). No images/PDFs opened. No status changes. Full per-hypothesis write-ups: contradicting/H1-contra.md … H5-contra.md.

## 1. Per-query relevance gate notes
| # | Hypothesis | Query | Gate verdict |
|---|---|---|---|
| Q1 | H1 | low fidelity simulation matches high fidelity anomaly detection ablation manufacturing digital twin | PASS — JIM 2024 parity + aviation 0.6%-gap/4.3x speedup hits |
| Q2 | H1 | added physics complexity no improvement fault detection ablation study simulation | PASS — partial MISS on exact ablation; adjacent hits kept |
| Q3 | H1 | digital twin stalled deployment data decision gaps not physics fidelity postmortem manufacturing | PASS — 6-source convergent HIT |
| Q4 | H1 | thermal model lumped parameter no gain detection performance anomaly industrial | WEAK PASS — fit-quality only, no detection deltas → MISS as contra |
| Q5 | H1 | simulation fidelity vs machine learning detection performance parity simple model sufficient | PASS — Truong CoRL23 (verified) + selective multi-fidelity + OSTI no-gain hits |
| Q6 | H2 | 1 Hz RMS sampling sufficient fault diagnosis motor bearing industrial historian | PASS — APC 0.0167 Hz dataset + ISO-10816 practice + RMS-roughness hits |
| Q7 | H2 | high frequency sampling no benefit anomaly detection downsampled vibration condition monitoring | PASS — multi-rate non-monotonicity + 100x-downsample <4pp hits |
| Q8 | H2 | plant historian 1 second RMS envelope sufficient never need bearing frequencies vibration monitoring practice | PASS — 1-min-RMS/1-s-buffer practice + 25.6 kS/s envelope-requirement (contra-both-ways) |
| Q9 | H2 | partial impulse model adds noise degrades detection half-modelled timescale simulation harm | FAIL — only adjacent comms/PD impulsive-noise literature → MISS |
| Q10 | H2 | sub-band envelope vs full bandwidth vibration no gain fault classification comparison | PASS — MESE single-band + ball-defect-exception + band-misplacement-danger hits |
| Q11 | H3 | manufacturing plant tolerates high false alarm rate operator trust sensitivity preferred precision | PASS — 10–15% defensible / 3–5% override-ceiling hits |
| Q12 | H3 | EEMUA 191 alarm rate unrealistic modern alarm management nuisance alarm tolerable process plant | PASS — steady-state compliance common; upset-peak is live problem → HIT |
| Q13 | H3 | anomaly detection F1 ranking agrees precision ranking no tradeoff sensitivity recall industrial case study | PASS — BDCC identical ranks + ranking-noise ±0.34 hits |
| Q14 | H3 | false positive rate 9 percent unnecessary anomaly detection low FPR high recall achievable benchmark | PASS — 1% acceptable / 1e-5–1e-4 / 1.3%-FPR-deployed hits |
| Q15 | H3 | precision bar destroyed recall value alert budget miscalibrated threshold artifact anomaly detection | PASS — PMLR-151 post-hoc-precision-suboptimal + cost-optimisation-practice hits |
| Q16 | H4 | PCMCI causal discovery gradual drift degradation root cause success time series | PASS — hierarchical-PCMCI-mfg + RADICE + deterioration-theory (contra-both-ways) |
| Q17 | H4 | graph distance Jaccard reference graph anomaly root cause failure no improvement | FAIL as contra — returned supporting-side field precedent (Jaccard-drift, RoFaD, StaR); recorded honestly in H4-contra |
| Q18 | H4 | simple threshold crossing order delay timing root cause manufacturing beats causal discovery | PASS — IFAC-CPG + LSTE-delay + COKE + CERN-binary-flag A1 hits |
| Q19 | H4 | causal discovery always helps anomaly diagnosis mask prior ablation no gain BARO random walk baseline | PASS — BARO-SOTA + CD-RCA-low-amplitude-weakness + Mask2Cause-design-matters hits |
| Q20 | H4 | PCMCI limitations nonstationary slow drift timescale failure causal discovery benchmark | PASS — stationarity/autocorrelation + regime-restriction-repair (A2) hits |
| Q21 | H5 | anomaly detection without calibration zero-shot threshold transfer new sensor channel success | PASS — training-free-zero-shot + OpenCSI-single-threshold + ACR + zero-shot-bearing hits |
| Q22 | H5 | single global threshold multiple sensor channels sufficient per-channel calibration unnecessary | WEAK PASS — adjacent only; not decisive |
| Q23 | H5 | domain randomization no calibration sim-to-real transfer anomaly detection sensor noise robustness | PASS — DR-no-finetune + DROPO + RAPT-optional-calibration hits |
| Q24 | H5 | unsupervised anomaly detection plug-and-play new sensor no retuning quantile threshold portability | PASS — tsanomaly-single-cutoff + GDI-zero-retune + safeband-self-adapt (product-grade caveat logged) |
| Q25 | H5 | adding new sensor channel no recalibration threshold porting works fine monitoring | FAIL — IT-monitoring trivia only → MISS |

## 2. Per-source hit/miss/unverifiable verdicts (condensed)
HIT (contra-direction, 19 lines): JIM-2024 low-fi parity; aviation 0.6%-gap ablation; Truong-CoRL23 (verified full page); 6 data-gap postmortems; Machines-14(5):480 selective-fidelity; OSTI-2424805 no-gain; APC 0.0167 Hz dataset; ISO-10816 RMS practice; RMS-roughness; multi-rate non-monotonicity; Sensors-21:6678 pipeline (UNVERIFIED full text); JMSE 100%-at-downsample; band-selection; envelope-band-requirement + fragility set; oxmaint 10–15% tolerance; EEMUA/ASM steady-state compliance; BDCC identical-ranks + ranking-noise; 1%-FPR-acceptable; AUPIMO-1e-5; PMLR-151 precision-constraint; HPCMCI-mfg + RADICE (vs Tripwire A); IFAC-CPG/LSTE/COKE/CERN A1 set; BARO + CD-RCA-weakness; regime-restriction-repair (A2); zero-shot set; single-cutoff tooling; DR/DROPO/RAPT; M2AD auto-calibration (verified abstract, MIXED direction). MISS (4): Q4-thermal (no deltas); Q9-harm (adjacent domain only); Q17 (supporting-side returned — logged, not suppressed); Q25 (IT trivia). UNVERIFIABLE (2): MDPI-6678 full text (403); zenodo-18682497 record (browser wall) — snippet-level only, flagged in-file.

## 3. Falsification strength per hypothesis
- H1: **Not falsified after 5 searches** (strong circumstantial pressure on P1 pass predictions; tripwire untriggered — no same-seed ablation battery exists in literature).
- H2: **Provisionally falsified after 5 searches** on the ≥15% envelope-separation leg; conditioned-F1 leg not falsified.
- H3: **Provisionally falsified after 5 searches** (budget stricter than plant tolerance, wrong regime, ranking-agreement base rates, artifact mechanics); survives only with plant-grounded budget-range stability.
- H4: **Not falsified after 5 searches** (genuinely contested; highest-risk leg P3 graph-free-control; subset battery with BARO-style control decisive).
- H5: **Provisionally falsified after 5 searches** as a manual per-channel spec ship-blocker; auto-calibration interpretation not falsified — recommend third global/auto arm per H5-A2.

## 4. Guard compliance (contra run)
No supporting-search performed. No hypothesis statuses changed (synthesis decides). Nothing written outside contradicting/ + this log section. No invented sources — every citation carries URL/DOI + fetch/UNVERIFIED flag. Q17 negative result (supporting-side hit during falsification search) disclosed, not suppressed.
