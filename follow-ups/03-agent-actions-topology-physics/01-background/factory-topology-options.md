# Factory topology options for a traceability-first sim — Verdandi background note

Date: 2026-09-16 · Follow-up 03 (`agent-actions-topology-physics`), `01-background`.
Purpose: decide whether to keep/reshape Verdandi's 32-machine config (A0–9 / B0–9 / C0–7 + ASM0–2 + RWK0, 31 buffers + SBUF, AGV cap 2).
Constraint: does NOT re-derive follow-up-01 C1–C6 channel decisions (drive-current CH8, energy CH9, wear-knee, thermal/impulse probation, air header CH10) — see `../01-sim-depth-physics-ml/04-synthesis/upgrade-spec.md`. Topology only.
Verdict up front: the user's critique is substantially correct — serial length past ~4–5 machines adds near-zero diagnostic information. The reshape should trade serial length for branching factor + a second shared resource + one redundant-parallel cell + one divergent branch. Recommended: Option A (§5).

Research base: 10 distinct web queries (2026-dated), incl. 2 contradiction-seeking (§4). SKIP generic smart-factory overviews per brief.

---

## §1. How published diagnosis/RCA sims structure their plants (counts + why)

**1a. Tennessee Eastman Process (TEP) — 5 units, 52 vars, 21 faults.** Reactor + condenser + separator + compressor + stripper, recycle loop included. The tracing value comes from the *recycle* (a global feedback loop: faults recirculate) and strong coupling, not unit count. Standard benchmark: Reinartz et al. 2021 extended dataset (28 faults × magnitudes × 6 modes); 450+ papers benchmarked on it (overview: arXiv 2401.10266). Lesson for Verdandi: one recycle/rework loop is worth more traceability than +6 serial machines. ([TEP extended dataset](https://www.sciencedirect.com/science/article/pii/S0098135421000594), [IEEE DataPort TE set](https://ieee-dataport.org/documents/tennessee-eastman-simulation-dataset))

**1b. causRCA — ONE machine (CNC vertical lathe), 92 vars, 104-edge expert graph, 19 fault scenarios.** Joint CD+RCA benchmark (Mehling et al., Procedia CIRP 139, 2026): 170 normal + 100 HIL fault recordings across coolant/hydraulics/probe subsystems. Tracing value from *depth within one machine's subsystems* (coolant → hydraulics → probe causal subgraphs), not plant breadth. Direct support for the critique: a publishable RCA benchmark needs exactly one richly-instrumented unit, not 32 thin ones. ([paper](https://www.sciencedirect.com/science/article/pii/S221282712500976X), [repo](https://github.com/causalgraph/causRCA))

**1c. causalAssembly — 5 stations × 2 processes (10 ops), layered DAG ground truth.** Real assembly-line measurements + distributional-random-forest synthetic twin; ground truth is a *layered* DAG across stations (Göbler et al., PMLR 236). Assembly (converging) structure gives each stage a distinct causal signature. Note the count: 5 stations sufficed for a causal-discovery benchmark. ([PDF](https://proceedings.mlr.press/v236/gobler24a/gobler24a.pdf))

**1d. Multistage manufacturing (SoV line) — real plants have 55–75 stations, but diagnosis runs on sparse patterns.** Automotive body assembly: 150–250 parts, 55–75 stations (Wisconsin MPAC review); yet the diagnosability literature (Ding–Shi–Ceglarek 2002; Zhou et al. 2003) works with *state-space variation-propagation models* where only a few dominant patterns matter (Wang & Shi 2020 "holistic sparse" framing). Real-plant station count is a throughput fact, not a diagnosis requirement. ([MPAC review](https://mpac.engr.wisc.edu/pdf/paper35.pdf), [Wang & Shi NSF copy](https://par.nsf.gov/servlets/purl/10293460))

**1e. Flow-line throughput sims — 3 stations is the norm.** Tecnomatix flow-line study: source → 3 stations → drain, buffers as the experimental variable; FerruForm axle line: tempo stages with 3–4 parallel machines per stage + gantry robots. Small serial counts; experimental leverage from buffers/parallelism/shared robots. ([IJERT Tecnomatix note](https://www.ijert.org/analysis-of-throughput-parameters-in-a-flow-line-manufacturing-system-with-varying-the-buffer-limits), [FerruForm thesis](https://www.diva-portal.org/smash/get/diva2:1015842/FULLTEXT01.pdf))

**1f. Semiconductor cluster tools — 1 robot + few chambers, shared-robot contention is the phenomenon.** 2026 DRL-scheduling literature (Comp. & Ind. Eng. 211, 111698; Sci. Rep. Dec 2025) models a PVD tool as VTM/ATM robots + chambers with residency-time + cleaning constraints; Intel SEMI fault-isolation challenge similarly targets tool-level isolation. Lesson: a *shared handler* (robot) coupling a few chambers creates richer diagnosis structure than a long line. ([C&E 2026](https://www.sciencedirect.com/science/article/abs/pii/S0360835225008447), [Sci Rep cluster-tool sim](https://www.nature.com/articles/s41598-025-31722-7))

**1g. NIST Simantha — source/machine/buffer/sink/maintainer primitives, async lines with finite buffers.** Our closest methodological cousin (Python DES, degradation + maintenance); lines are composed from primitives with no prescription of large-N. ([NIST Simantha](https://www.nist.gov/services-resources/software/simantha-simulation-manufacturing))

Takeaway: no published diagnosis/RCA benchmark justifies 10-deep homogeneous serial lines. Counts that recur: 1 rich machine (causRCA), 5 stations (causalAssembly), 5 units + recycle (TEP), 3 stations + buffers (flow-line sims).

---

## §2. Which topological features create tracing VALUE (and who exploits them)

| Feature | Tracing value | Exploited by |
|---|---|---|
| Converging flow (assembly) | Faults from distinct feeders superpose at one point → attribution problem with GT (which feeder?) | causalAssembly (5-station layered DAG); Verdandi ASM0–2 already has this — KEEP |
| Diverging flow (sort/split) | Routing decisions create counterfactual branches ("had the part gone left…") — agent-rerouting gold | Metal-stamping inspection-allocation sims (ProModel); missing in Verdandi — ADD |
| Shared resource contention (AGV, air header, operators, robot) | One root → multi-station symptoms; parity channels (CH8/CH10) separate sensor-vs-process | TEP recycle compressor; cluster-tool robots; Verdandi AGV cap 2 + C6 air header — KEEP, ADD second |
| Rework/feedback loop | Cycles break DAG assumptions, create hop-counted cascade chains (CASCADE_CHILD depth≤3 already specced) | TEP recycle; Verdandi RWK0 (max 2 passes) — KEEP |
| Gateway/bottleneck machines | Single-point observability choke: one sensor placement covers max fault pairs; bottleneck-ID benchmarks | Toyota active-period method; bottleneck-analysis lit (SciDirect S2213846318301172) — Verdandi ASM2 is the natural gateway — INSTRUMENT |
| Parallel redundant stations | Masking/failover signatures: fault present but symptom absent until sibling saturates — unique traceable phenomenon serial lines cannot produce | FerruForm tempo stages (4 + 3 + 3 parallel); missing in Verdandi — ADD |
| Inspection/test stations | Delayed-observation structure: fault at stage i observed at stage j>i → hop-distance labels; allocation itself is a research problem (ISAPDI, Computers & OR June 2026) | ISAPDI lit ([SciDirect S0305054826002066](https://www.sciencedirect.com/science/article/abs/pii/S0305054826002066)); missing in Verdandi — ADD cheaply (a quality-gate flag, not new physics) |

Verdandi already owns the two highest-value features (assembly convergence, rework loop) plus one shared resource (AGV) and a second specced (air header C6). The gaps are exactly: divergence, redundancy, inspection delay, second contention.

---

## §3. Diagnosability-by-design (sensor placement, structural isolation, zonal decomposition)

- **Minimum-sensor isolability.** Frisk & Krysander (DX-07): place sensors for *maximum fault isolability*; Yassine–Rosich–Ploix (AQTR 2010): optimal placement subject to diagnosability specs without computing testable subsystems. Implication: isolability is a property of *which* variables are sensed at *branching/coupling* points, not of machine count. ([AQTR 2010 record](https://orbilu.uni.lu/handle/10993/4482?&locale=fr))
- **Structural decomposition (NASA).** Daigle et al.: sensor placement as residual selection over structurally decomposed submodels; single-fault diagnosability = all fault pairs distinguishable; then optimise decomposition for distributed diagnosis. Direct precedent for Verdandi's topology-masked PCMCI+ (F4): zones should follow the decomposition, i.e. feeder-line / assembly / rework / shared-resource zones — *not* 32 flat nodes. ([NASA PDF](https://ntrs.nasa.gov/api/citations/20190001644/downloads/20190001644.pdf))
- **MMP diagnosability, 3 levels.** Hu & Shi (ASME J. Manuf. Sci. Eng. 124:2): diagnosability conditions (a) within station, (b) between stations, (c) overall process, from fixture geometry + sensor locations. Serial length helps only level (b), and only up to the point where adjacent-station signatures decorrelate. ([ASME](https://asmedigitalcollection.asme.org/manufacturingscience/article/124/2/313/462054/Fault-Diagnosis-of-Multistage-Manufacturing))
- **Model validation for diagnosis.** Diedrich et al. (KR 2026 / EAAI 2026): validate system descriptions by quotient of diagnosable vs non-diagnosable faults (q_F). Gives Verdandi a metric for the reshape: compute q_F before/after; keep the config change iff q_F rises. ([extended abstract](https://kr.org/KR2026/FinalVersionsRPR/OnValidatingPropositionalLogicSystemDescriptions.pdf))
- **Sensor-value framing.** Kulkarni et al. (Sensors 2021, PMC8512200): fault–sensor dependency matrix + selection on detection probability / tolerance / time. Use to justify *which* new sensors (inspection gate, redundant-cell load share, second resource level) rather than more of the same. ([PMC8512200](https://pmc.ncbi.nlm.nih.gov/articles/PMC8512200))
- **SDG propagation reasoning.** SDG+QTA framework (Chem. Eng. Res. Des. 85:10; Ali et al. PMC8945140): signed arcs give completeness (rarely miss the true fault) at the cost of spurious candidates — the same precision/recall trade the flip-gate + T5 healthy-window battery manages. Branching topologies produce *discriminating* sign patterns; pure serial lines produce identical sign chains (all downstream nodes show the same qualitative trend → spurious set = whole downstream line). This is the formal version of the user's critique. ([PMC8945140](https://pmc.ncbi.nlm.nih.gov/articles/PMC8945140/))

---

## §4. Engaging the critique (incl. 2 contradiction-seeking queries)

**The critique is correct for homogeneous serial segments.** Three independent lines of evidence:

1. *(Contradiction query 1: "topology complexity hurts diagnosability")* — **DiagMLP (arXiv 2501.02766): topology-agnostic MLP matches/beats GNN-based diagnosers on 5 microservice datasets**; trace preprocessing already encodes topology, so the graph module adds nothing. Transfer: if per-machine channels (CH0–CH10) already encode line position via blocking/starvation offsets, extra serial machines add parameters, not signal — while costing replay CPU and diluting the ≥10pp graph-ablation bar (T2). Related: graph-only models lose to log encoders on fault classification (arXiv 2604.14019) — structure without discriminating features is dead weight.
2. *(Contradiction query 2: "serial observability equivalence / diminishing returns")* — **Diminishing marginal returns for sensor networks** (water-distribution case: detection likelihood saturates ~2–50 sensors; Guelph study): each added sensor past the knee buys ~zero. Same shape holds along a serial line: machines 5–10 sit behind the same blocking/starvation wavefront as machines 1–4. **Inspection-station allocation** (Rakiman & Bon: *fewer* stations sometimes *increase* production time; ISAPDI 2026): placement dominates count — one inspection gate after ASM2 beats sensors on A6–A9.
3. **Observability-equivalence argument (stated formally).** In a homogeneous serial line with finite buffers, machines i and i+1 are distinguishable only if (a) the inter-buffer decouples their blocking/starvation states within the detection window, or (b) their fault signatures differ (class-scaled current k_m, wear α_m). With T=300, CAL_WIN=120, dur 8–25: a fault at A3 vs A7 produces the same downstream starvation wave and the same upstream blockage wave, time-shifted by < buffer-drain time. If that shift < window resolution, A3≡A7 (observability-equivalent). Length adds numbers, not tracing value — exactly as the user said.

**What ADDS information (each raises branching factor or coupling rank):**
- +1 shared resource (operator pool / power bus / second AGV zone): creates cross-line correlations no serial machine can (C6 air header is the first; a second makes contention *attribution* a problem: which resource is the root?).
- Parallel redundant pair: creates masking signatures (fault hidden until failover) — a new fault *class*, not a new fault *location*.
- Divergent branch (e.g. post-ASM pass/fail or model-mix split): routing counterfactuals for the rerouting agent — the downstream task needs somewhere to reroute *to*.
- Inspection gate with delay: converts location ambiguity (A3 vs A7) into hop-distance labels the AC@1 depth≤3 metric can score.
- Multi-root overlap (already specced MULTI_ROOT): needs ≥2 *independent* feeder roots — 3 feeders already suffice; a 4th–10th machine per feeder adds nothing here either.

---

## §5. Concrete alternative configs (ranked; costs relative to current 32-machine twin)

Common to all: keep SimPy DES, T=300, 36-stream SeedSequence, C1–C6 channels, 7 fault classes + CASCADE_CHILD/MULTI_ROOT/S-vs-P tags. Machine-step physics is O(1)/machine-step; replay wall scales ~linearly in machines + part-movement events.

### Option A — "Short + branchy" (RECOMMENDED)
- Config: feeders A/B 10→5, C 8→4 (14) + ASM0–2 (3) + RWK0 (1) + redundant parallel pair PAR0–1 replacing one serial segment (2) + divergent packaging branch PKG0–1 post-ASM, split by quality flag (2) + 1 inspection gate IASN0 (counts as machine slot, no physics beyond flag) → **~22 machines, ~21 buffers**.
- New traceable phenomena: failover masking (PAR), routing counterfactuals + reroute target (PKG), delayed-observation hop labels (gate), second-contention attribution if paired with operator-pool resource (cheap counter, no ODE).
- CPU: ≈ −30% machine-steps + fewer part events → est. −25 to −35% wall vs baseline.
- Migration: MEDIUM. `twin.py`: machine/buffer tables are already parametric lists — rewire routing (divergent split on REJECT flag exists in part-carried flags; reuse), add PAR load-share rule (~30 lines), gate flag (~15 lines). `SIM_SPEC.md`: topology table + AC@1 zone map update.

### Option B — "Minimal diagnosability rig" (cheapest, best for ablations)
- Config: A/B 3 each + C 2 + ASM 2 (merge ASM1/2, keep gateway ASM2) + RWK 1 → **11 machines, ~10 buffers**. One shared resource (AGV), one loop, one convergence. q_F calibration anchor.
- New phenomena: none — but every remaining machine is observability-distinct; cleanest T2 ablation (+≥10pp gap easiest to show).
- CPU: est. −55 to −65% wall; fastest M0b battery iterations.
- Migration: SMALL (pure deletion + table edits; no new routing logic). Risk: too thin to demo agent rerouting (no branch target) — pair with A, don't ship alone.

### Option C — "Cell + cluster-tool" (max new science, highest cost)
- Config: A/B shortened to 4 each (8) + replace line C with 4-chamber cluster cell sharing one robot handler (4 + robot counter) + ASM0–2 + RWK0 + PKG branch (2) → **~20 machines + 1 robot resource**.
- New phenomena: shared-robot contention/delay propagation (semiconductor pattern, §1f), chamber-vs-handler attribution, residency-time-class faults.
- CPU: robot scheduling is event-dense; est. −10 to −20% wall (savings partly eaten by handler logic).
- Migration: LARGE. New handler process in `twin.py` (~150–250 lines), new fault kind (handler-delay), new zone in causal mask. Justify only if agent-rerouting research targets shared-handler cells.

### Option D — "Keep 32 as scale arm" (not a reshape — a battery role)
- Config: unchanged 32. Role: scale-robustness control (does AC@1 hold at 32 nodes? does flip-gate stay <40%?). No new phenomena; observability-equivalent tail machines documented as such via q_F before/after vs Option A.
- CPU: baseline. Migration: ZERO (docs-only: label A5–A9/B5–B9/C4–C7 as scale-arm nodes in `SIM_SPEC.md`).

**Ranking: A > B (as ablation companion to A) > D (docs-only scale role) > C (defer unless handler-cell rerouting is in scope).** Downstream multiple-choice: present A (reshape target), B (fast ablation rig — build alongside A), D (keep-32-as-scale-arm), C (ambitious cell variant).

---

## Sources (all 2026-dated queries; key links inline above)
TEP extended dataset (Reinartz 2021); TEP condition-monitoring overview (arXiv 2401.10266); causRCA paper + repo (Procedia CIRP 139; GitHub causalgraph/causRCA); causalAssembly (PMLR 236); SoV diagnosability (Ding–Shi–Ceglarek 2002; Zhou 2003; Wang & Shi 2020); cluster-tool DRL sims (C&IE Jan 2026; Sci Rep Dec 2025); Simantha (NIST); sensor placement (Frisk/Krysander DX-07; Yassine AQTR 2010; Kulkarni PMC8512200; NASA Daigle structural); SDG (Chem Eng Res Des 85:10; PMC8945140); DiagMLP (arXiv 2501.02766); log-vs-graph diagnosis (arXiv 2604.14019); diminishing returns (Guelph water study); inspection allocation (Rakiman & Bon; ISAPDI 2026); AGV/Petri-net deadlock (arXiv 2508.00724); Hu & Shi MMP diagnosability (ASME); Diedrich KR2026 validation.
