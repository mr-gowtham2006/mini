# H1-contra — Falsification attempt: ablation-gated physics depth

Date: 2026-09-13 · Role: devil's advocate · Hypothesis: H1 (each new physics term must move raw-F1 ≥3pp or AC@1 ≥10pp vs ablated twin, else cut)
Falsification criterion targeted: retained-but-below-delta terms; registered alternative A1 (fidelity ≠ detection — low-fi parity, gains from seed/mask/calibration not physics).
Searches run: 5 (all 2026-09-13). Fetches: 2 (PMLR pmlr-v205-truong23a verified; MDPI sensors-21-19-6678 fetch 403, snippet-only → UNVERIFIED full text).

## Contra evidence

### C-H1-1 (HIT — direct tripwire analogue): lower fidelity matches high fidelity on detection
- Zhang et al., J. Intell. Manuf. 2024, "Investigating the influence of fidelity on the capability of a digital twin to detect material extrusion failures" (ideas.repec.org/a/spr/joinma/v35y2024i5d10.1007_s10845-023-02144-x): varied fidelity of six DT attributes; "for the two failure cases considered, lower-fidelity DTs can deliver comparable capability to what are considered DTs with high or ultra-high fidelity", plus lower config cost, storage, simulation and detection time. Verdict: HIT — exact A1 mechanism (below-delta retention unjustified), detection-parity at lower fidelity.
- General-aviation multi-fidelity DT + FMEA (arxiv 2604.22777 snippet): "performance gap of only 0.6% yields 4.3x (CPU) and potentially 19–38x (GPU) inference acceleration"; ablation: "using only paired-mirror residuals can reach the performance ceiling of F1=0.99". Verdict: HIT — the added-fidelity arm sits far below any 3pp-equivalent delta while costing multiples of compute.

### C-H1-2 (HIT): adding fidelity can HURT learning (not just fail to help)
- Truong et al., CoRL 2023, "Rethinking Sim2Real" (PMLR 205:859–870, full page fetched + verified): "adding fidelity does not help with learning; performance is poor due to slow simulation speed (preventing large-scale learning) and overfitting to inaccuracies in simulation physics." 2 simulators × 3 robots, real-world eval. Verdict: HIT — stronger than the tripwire: added physics is not merely below-delta, it reverses the sign via overfitting + throughput loss. Directly pressures H1-P3 (wall-clock leg).

### C-H1-3 (HIT): twins stall on data/decision gaps, not physics
- Forbes Tech Council 2026-08-06 "Why Digital Twins Failed": "first wave of digital twins failed because teams asked 'What should we model?' when they should have asked 'What decision are we trying to improve?'"
- MES Engineer 2026-08-09 "Digital Twin Fidelity Creep": failure is "the data plumbing underneath" — historian/unified namespace, OPC UA modelling, OT/IT security — not missing physics.
- Softarex 2026-08-10: twin failures "show up as 'the model is wrong'" but root causes are late signal (batch ETL after decision window), unlabeled signal (no ground truth), broker topology.
- TradeNexus Pro 2026-05-02: "If even 1 or 2 of [PLC/historian/MES/maintenance/quality/ERP] sources are inconsistent, delayed, or incomplete, the twin starts to drift."
- context-clue.com (data-layer postmortem): "Garbage in, garbage out – models based on flawed inputs lead to wrong insights."
- doi 10.1080/00207543.2025.2514728 (industry case): key gaps were missing conveyor/presence sensors — digitisation gaps, not physics depth. Verdict: HIT (convergent, 6 sources) — predicts H1 ablation deltas will vanish once seeds/masks are fixed because the binding constraint is data, per A1.

### C-H1-4 (MIXED — cuts both ways): selective multi-fidelity works, uniform depth does not
- MDPI Machines 14(5):480 (2026-04-24) "FMEA-Guided Selective Multi-Fidelity Modeling": "When the HFM is uniformly applied to all components, the real-time performance deteriorates, whereas the uniform application of the LFM results in the inability to capture detailed phenomena such as harmonics or ripples." Selective hybrid balances both. Verdict: HIT against *unconditional* depth (uniformly adding thermal/current/wear/impulse terms = the HFM-everywhere arm that deteriorates); compatible with H1's per-term gating — i.e. contra vs "add all physics", not vs the ablation rule itself.
- OSTI 2424805 (multi-fidelity PIML damage diagnosis): "high-fidelity physics simulations that do not cover the (test and damage) parameter space do not improve the performance of diagnostic PIML models built using data from many low-fidelity" runs. Verdict: HIT — added physics with no parameter-space coverage = below-delta by construction.
- ResearchSquare rs.3.rs-8164519/v1: "task-relevant signal fidelity — not absolute detail — determines the optimal model choice." Verdict: HIT — restates the ablation gate as the correct bar, denying any default presumption that H1's predicted pass terms (wear-knee, current coupling) clear it.

### C-H1-5 (MISS as contra): thermal-model literature shows fit quality, not detection deltas
- LPTN/TNN papers (energies-13-01-37, arxiv 2103.16323v2, ejee open-phase fault): lumped thermal models predict temperature within ~0.4–2°C — but none report detection-F1 or traceback deltas vs an ablated twin. Verdict: MISS — no falsifying observation; absence of ablation reporting is a gap, not a tripwire hit.

## Falsification verdict: NOT FALSIFIED after 5 searches — but pressured
No source ran the registered battery (same-seed ablated-vs-full, raw-F1 + AC@1), so the tripwire is untriggered. However the contra direction is well-evidenced: low-fi parity (C-H1-1), sign-reversal via overfitting/speed (C-H1-2), data-gap dominance (C-H1-3), and uniform-depth deterioration (C-H1-4) jointly imply H1's predicted passes (wear-knee, current coupling) are the claims most at risk — the ablation battery must confirm them, not assume them. Falsification strength: **Not falsified after 5 searches** (strong circumstantial pressure on the P1 pass predictions).
