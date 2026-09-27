# Recommendations — sim-depth-physics-ml (follow-up 01)

Date: 2026-09-13 · Each block: Decision + Tradeoffs (numbers/sources) + verb-led Next actions (artifact + owner) + numeric Drop condition. Battery-scoped only; no platform/ROI framing (parent F2 Locked); raw-F1-only scoring (parent F1 Locked).

---

## Recommendation 1: Gate every physics term on its own ablation — choose per-term gate over bundled depth

**Decision:** Choose the H1 per-term ablation gate (each term: same-seed ablated-vs-full run on the frozen M0b battery; retain iff ΔF1_raw ≥3pp OR ΔAC@1 ≥10pp, wall <600s, 0-diverge ×5) over adding the thermal/current/wear/impulse bundle jointly.

**Tradeoffs:** (1) Per-term batteries cost ~4× the runs of one bundled comparison — but the bundled arm cannot attribute (C-H1-4: uniform-HFM deterioration vs selective-hybrid win, Machines 14(5):480), and low-fi parity (JIM-2024, A-S5) plus Truong CoRL23 sign-reversal (overfitting + throughput loss) mean any unablated term is guilty until proven useful. (2) The 3pp/10pp bars may cut terms with small-but-real gains (e.g. thermal lag) — accepted deliberately: sub-delta terms cost CPU + calibration labor (Halkwinds 2026: calibration excluded from business cases, performance degrades within months) for unmeasurable detection value.

**Next actions:**
- Write `m0b-preregistration.md` (20–32 seeded faults, fixed seeds, quantile config hash, PA-off, per-term arms) — owner: twin lead.
- Run per-term ablation battery, log ΔF1/ΔAC@1/wall/diverge to `m0b-ablation-runtable.csv` — owner: twin lead.
- Cut every below-delta term from the spec before channel work begins — owner: synthesis lead.

**Drop condition:** Any term retained in `src/twin.py` with ΔF1 < 0.03 AND ΔAC@1 < 0.10 vs its ablated arm → drop the term and revoke its finding leg (F7 tripwire); full-twin wall ≥600s or any diverge across 5 seeds → drop the most recent term addition regardless of metric gain.

## Recommendation 2: Ship multi-scale export now, hold the synthetic impulse term on probation — choose windows-first over tier-bundled

**Decision:** Choose the windows-only arm (1/step RMS-grade base + 0.5/1.0/2.0-step-equivalent multi-scale export, no-bearing-frequencies boundary) as the default sampling design, with the wear-coupled impulse term admitted ONLY if it clears the H2 battery (≥15% envelope-separation gain AND conditioned-F1 within ±3pp).

**Tradeoffs:** (1) Windows-only forfeits the impulse-term upside — but the ≥15% leg is provisionally refuted from both sides (C-H2-1/2: RMS suffices at 0.0167 Hz–1 Hz historian practice, I03/I04/I08; C-H2-3: real envelope needs 2–10 kHz resonance at ≥25.6 kS/s the tier disclaims), while windows carry convergent support (A-S10 multi-scale windows; A-S12 ≥8pp full-bandwidth over 19 datasets; GES2N envelope-SNR metric, L4 verified). (2) Three-way arms (RMS-only / windows-only / windows+impulse) cost 3× scoring — but H2-A2 (windows explain the gain) is the live alternative and only the three-way split separates it.

**Next actions:**
- Implement multi-scale export windows in `src/twin.py` + log envelope-band energy statistic to `h2-envelope-runtable.csv` — owner: twin lead.
- Run three-way arms on seeded impulse faults; record separation gain + skew/kurtosis-conditioned raw-F1 — owner: detector lead.
- Pre-register the GES2N-style targeted-SNR vs sub-band-energy statistic choice before scoring — owner: synthesis lead.

**Drop condition:** Separation gain <15% OR ΔF1_tiered−base ≤ −0.03 → drop the impulse term (keep windows); windows-only matching windows+impulse within ±3pp F1 and ±5pp separation → drop the impulse term even if tiered-vs-base passes (H2-A2 wins).

## Recommendation 3: Replace the 10/1000 constant with a 20–150/1000 range + per-machine recording — choose plant-grounded range over single constant

**Decision:** Choose the surviving H3 reading (demotion tested at 20/1000 and 150/1000 edges with a 5/1000 stability check; per-machine/worst-zone alert rates recorded, transient-inclusive healthy pool) over the refuted 10/1000 constant.

**Tradeoffs:** (1) The range admits 20–150× more alerts than 10/1000 (10 vs 20–150 per 1000 windows) — but 10/1000 (=1%) sits 3–15× below demonstrated plant tolerance (oxmaint 10–15% ordinary / 3–5% override ceiling; deployed 1%/1e-5–1e-4 FPR systems clear 10/1000 trivially, C-H3-1/C-H3-4), so demotions at 10/1000 are threshold-artifacts, not trust findings. (2) Per-machine + transient-inclusive recording doubles instrumentation work — but trust is set by the worst zone (S-H3-02: follow-through 0.94→0.43) and steady-state compliance is already common while upset-peak is the live problem (C-H3-2: ASM/EEMUA benchmarking), so global clean-steady rates mislead.

**Next actions:**
- Build fault-free healthy-window replay incl. start-up/changeover windows; sweep detector settings, log alerts/1000 global + per-machine to `h3-precision-runtable.csv` — owner: detector lead.
- Run veto-off control (VETO_ASM2 + flip-gate disabled) on identical settings — owner: detector lead.
- Pre-register the 20/150/5 budgets before the sweep — owner: synthesis lead.

**Drop condition:** Zero top-quartile-F1 settings breaching BOTH 20/1000 and 150/1000 → drop H3 (rankings agree; C-H3-3 direction); demotion at only one of {5, 20, 150}/1000 → reject that demotion as threshold-artifact; veto-off vs veto-on rate delta ≈ 0 → revoke the precision-mechanism leg (H3-P2 fails).

## Recommendation 4: Split the causal layer with a graph-free control — choose three-arms-plus-BARO over PCMCI-only or split-on-faith

**Decision:** Choose the H4 split battery (PCMCI+-alone / Jaccard-complement-alone / split + BARO-style graph-free control; fault-subset AC@1 depth≤3) over keeping masked PCMCI+ for all regimes or adding the complement without the control.

**Tradeoffs:** (1) Four arms × two fault subsets costs the most battery of any H1–H5 test — but Tripwire A (PCMCI-family manufacturing wins: hierarchical-PCMCI PHM-Xi'an 2025, RADICE on PCMCI+), P3 (IFAC-CPG/LSTE/COKE/CERN ordering-timing set), and A2 (regime-restriction repair, C-H4-5) each independently kill an untested split; TimeGraph B1/C1 (TPR 0.00/FDR 1.00) + A-S7 (adapted-FCI+Jaccard field case) jointly motivate it. Only the full design separates ordering credit from graph-distance credit. (2) Jaccard-vs-reference needs an expert-set reference graph + threshold (C-H4-2 brittleness) — the battery must log reference/threshold choices or the drift win is non-replicable.

**Next actions:**
- Implement the Jaccard graph-distance complement + BARO-style control walk; freeze reference graph + threshold in `h4-causal-preregistration.md` — owner: ranking lead.
- Run four arms × {wear-drift, abrupt/propagating} subsets; log subset AC@1 to `h4-causal-runtable.csv` — owner: ranking lead.
- Add the multi-scale-windowed PCMCI+-alone factorial (H4-A2 control) — owner: ranking lead.

**Drop condition:** PCMCI+-alone wear-drift AC@1 ≥50% (Tripwire A) OR split gain <10pp (Tripwire B) → drop the complement; split abrupt-subset regression ≥10pp (Tripwire C) → reject the split even if drift passes; graph-free control within ±5pp of complement on drift (P3) → cut the complement, credit ordering.

## Recommendation 5: Ship auto-calibration, keep manual spec as fallback — choose M2AD-style auto arm over manual-spec blocker

**Decision:** Choose the narrowed H5 rule (every new channel ships an automatically derived operating point + noise characterisation — normalised rarity scores / GMM+Gamma layer / self-adapting limits — with the hand-written per-channel Q_DET+noise spec as audited fallback) over the refuted manual-spec ship-blocker.

**Tradeoffs:** (1) Auto-calibration adds estimator machinery (GMM+Gamma layer à la M2AD, PMLR 258, +21% over 130 Amazon assets — abstract-verified only, U11) — but zero-shot single-threshold transfer (OpenCSI: single threshold F1 0.99 zero-shot; ACR batch-norm; C-H5-1 set), single-cutoff tooling (tsanomaly/GDI/signalmap/safeband, C-H5-2), and DR+optional-small-sample calibration (RAPT, C-H5-4) converge: hand-written specs are not the binding constraint. (2) Three arms (calibrated / uncalibrated-ported / global-auto) cost 3× calibration runs — but H5-A2 (global suffices) is decisive for scoping the rule down, and fault-mag confounding (H5-A1) requires FAULT_RANGES fixed across arms regardless.

**Next actions:**
- Implement auto-calibration arm (M2AD-style GMM+Gamma over nominal residuals) + global-threshold arm; hold FAULT_RANGES fixed — owner: twin lead.
- Run three arms; log alerts/1000 + raw-F1 per channel to `h5-calibration-runtable.csv` — owner: detector lead.
- Write per-channel ship checklist (auto operating point + noise characterisation + fallback manual spec) into `docs/SIM_SPEC.md` — owner: synthesis lead.

**Drop condition:** Uncalibrated arm ≤10 alerts/1000 AND ΔF1 ≥ −0.05 vs calibrated → drop H5 (calibration dispensable); global/auto arm within ±2pp F1 AND ±3 alerts/1000 of per-channel hand calibration → scope the rule down to global/auto, revoke the manual-spec leg.

---

*End — 5 recommendations, each with Decision + Tradeoffs (numbers + sources) + Next actions (verb + artifact + owner) + numeric Drop condition. MINIPRO-17 mapping lives in upgrade-spec §8.*
