# Verdandi Correction-Agent: Action Taxonomy + Execution-Mechanism Brief

**Goal:** decide WHAT the correction agent / RL routing policy can do and HOW each action is simulated, verified, actuated.
**Downstream:** M3.5 closed auditable action vocabulary + RL action space + OPC-UA/PLC boundary story (viva).
**Twin context:** 32-machine SimPy twin (lines A/B/C + assembly + rework loop + AGV pool, 31 buffers), per-alarm ranked causal trace, seeded replay.
**Date:** 2026-09-16. **Queries run:** 10 distinct (incl. 2 contradiction-seeking). See §8.

---

## 1. Corrective-action vocabularies in literature + real MES

### 1.1 Closest prior art: ctrl-alt-recover (Imperial AISL) + DPPT

The single most relevant template for Verdandi's "simulate-before-act" correction agent:

- **Repo:** `AISL-at-Imperial-College-London/ctrl-alt-recover` — "agents do not directly drive low-level controllers. They propose high-level recovery decisions, which are checked against plant knowledge and digital-twin rollouts before being accepted." Closed loop: init twin → inject fault → monitor → LLM planning/action → KG grounding (SPARQL) → simulation validation → apply-if-safe → reprompt-with-feedback → write decision traces to `traces/` + `results/`.
- **Discrete action space (mixing module):** finite-state-machine path selection. Planning agent picks next `UML:State`; action agent maps to actuator commands. Fault labels select expected route (normal P101 emptying vs bypass P102 emptying). Before applying, "code resets all actuators to zero and applies only actuators returned for target state." Deliberately small but safety-critical space: correct branch at B203-to-emptying + exact actuator pattern, no invented valves/pumps/states.
- **Continuous action space (CSTR):** only 3 supervisory setpoints (`T_sp, L_sp, Fin_sp` with quantized increments 0.05/0.05/0.005). Agent **cannot** touch valves/pumps/gains — those stay under PID. Faults (fouling −70% UA, pump degrade ×0.5, cooling stuck at 0.3) must be solved by load reduction, not "more cooling."
- **Paper:** Vyas et al., *From Detection to Action: Using LLM Agents for Fault-Tolerant Control*, arXiv:2606.28011 (2026-06-26). Framework = (i) 6-agent workflow (monitor, plan, act, simulate, validate, reprompt), (ii) **Digital Process Plant Twin (DPPT)** exposing data + models + simulation service for **pre-execution testing**, (iii) Graph RAG over CPSMod ontology (VDI 2206/3682, UML state machine, OpenMath, DIN 17359). "Corrective actions are generated as minimal-risk state-machine recovery paths … then validated deterministically against interlocks, envelopes, and dynamic feasibility before any actuation. If no acceptable plan is found within a bounded time window, control is handed to a safety fallback."
- **Tutorial:** arXiv:2606.31635 — ships executable mixer + CSTR envs with configurable faults, observables, pluggable recovery/validation; focus is "what must be considered and tested," not benchmark scores.
- **Verdandi mapping:** copy the 6-role split (Verdandi already has monitor/trace/narrate; add plan/act/simulate/validate/reprompt), copy the "reset-then-apply-only-returned-actuators" discipline, copy quantized supervisory setpoints rather than low-level writes, copy bounded-time fallback.

### 1.2 Manufacturing agent/twin literature: recurring verbs

| Verb | Where seen | Verdandi analogue |
|---|---|---|
| **Reroute (line / path / state-machine branch)** | ctrl-alt-recover mixer bypass P102; FMS Petri-net + MBRL lookahead for AGV positioning (arXiv:2601.04887); semiconductor fab risk-constrained route scheduling (arXiv:2608.30520, −16.4% delivery, −22.6% wait) | REROUTE_WIP(line, from→to, qty) |
| **Rework divert / quarantine / hold** | Aerospace Agentic Digital Thread for non-conformities (SAE 2026-26-0763, Master Agent orchestrates corrective actions); MQTO ontology (Zenodo 2026) countermeasure procedures; MES hold/quarantine pattern (Siemens Opcenter) | DIVERT_REWORK / HOLD_LOT |
| **Buffer divert / overflow redirect** | FJSP-AGV cooperative DRL (task/machine/AGV 3-way allocation); fab congestion-aware scheduling with empirical-Bayes queue correction | DIVERT_BUFFER |
| **AGV reassign / re-task / preposition** | OpenRMF `reassign_dispatched_tasks()`; ROOSTER fleet manager (`/place_order`, pending/active jobs); VDA 5050 fleet managers; MARL task/machine/AGV allocation (IEEE TSMC 2024) | REASSIGN_AGV |
| **Feed throttle / setpoint adaptation** | CSTR `Fin_sp` reduction under fouling/cool-stuck; safe-RL demand-response with Lagrange SAC for variable-speed + discrete actuators (IEEE TII 2024) | THROTTLE_FEED |
| **Speed override / rate change** | Congestion-responsive speed/spacing vs conservative path locking (4.8s vs 1.3s hold, hycmoop 2026-04-13) | SPEED_OVERRIDE |
| **Maintenance dispatch / work-order creation** | Oxmaint PLC→CMMS rules engine (fault code → classified WO + history + PM trigger); RAG failure-recovery recommender (IEOM Bangkok 2026) | DISPATCH_MAINT (escalate-only, never auto-commit) |

**Not in scope for M3.5 auto-commit:** PID retune, interlock bypass, safety-limit edit, recipe change — all require human/MOC.

### 1.3 Real MES actuation verbs (Siemens Opcenter / Rockwell alignment)

Siemens Opcenter Execution Automation Gateway binds OPC-UA nodes (PLC tags) to work-order operation fields for automatic in-process capture; connectivity via OPC-UA + REST + RIC. The MES-side verbs relevant to Verdandi are: **hold lot, quarantine/NCR, reroute work order operation, adjust dispatch list priority, create maintenance notification**. These are IT-level transactions (REST), not OT writes — important for the viva boundary story.

---

## 2. HOW each action is executed in reality + safety-interlock boundary

### 2.1 The boundary principle (cite in viva)

- **Read-only agent default (industry norm):** "A coater's web-tension loop or welder's interlock has to respond inside bounded time, every time. A probabilistic, variable-latency model cannot guarantee that, so actuation stays with certified deterministic control, and the agent reads." (Niobia PLC/OPC-UA/MQTT brief.)
- **OT finality profile (IETF draft-das-ot-actuation-finality-00):** "A write remains an Actuation Candidate Act… A setpoint write is not actuation." A Protected Enforcement Domain binds principal/zone/device/tag/value-envelope/mode/interlock-state/policy-epoch/sink, commits evidence, issues scoped authority; the I/O-driving sink verifies live and consumes it.
- **OPC-UA ≠ safety path:** "OPC UA works for supervisory, trending, operator-grade exchange. Not recommended as sole path for inter-controller interlocks or safety functions (unbounded latency, server-PC failure, no controller-visible diagnostics). For those use deterministic fieldbus gateway (Profinet↔EtherNet/IP, DP/DP coupler)." OPC-UA Safety (Part 15) adds SafetyProvider/Consumer state machines, CRC + timeliness checks, fail-safe substitute values + operator-ack on defined errors — but that is the *safety* channel, not the agent channel.

**Verdandi viva line:** agent writes are *advisory setpoint / dispatch candidates* (MES REST + OPC-UA supervisory nodes only); PLC safety interlocks and PID loops remain authoritative and can veto. Every auto-commit is single-scope, reversible, rate-limited, and logged with pre/post snapshots.

### 2.2 Per-action execution mechanism

| M3.5 action | Real execution | Verdandi SimPy effect | Actuation boundary |
|---|---|---|---|
| `REROUTE_WIP` | MES work-order reroute / dispatch-list reprioritise (REST); PLC route-permissive checked against interlock list | Change next-machine/line pointer for N lots; respect buffer caps | Agent proposes route; PLC/MES permissive (guard) must hold or action rejected |
| `DIVERT_REWORK` | MES NCR/quarantine + rework route; AGV transport order to rework cell | Move lot to rework queue, book rework capacity | Reversible; auto-commit allowed if rework queue < cap |
| `DIVERT_BUFFER` | OPC-UA supervisory divert flag consumed by PLC divert gate; overflow logic stays in PLC | Redirect inflow to named buffer if space | Masked out when buffer full (action masking) |
| `REASSIGN_AGV` | Fleet-manager API: VDA 5050 order update / `cancelOrder` + new order; OpenRMF `reassign_dispatched_tasks()`; ROOSTER `/place_order`; yuncang `POST /api/agv/plan-path` | Rebind AGV→task, recompute travel time, hold segment reservations | Never preempt safety stop; only idle/en-route-reassignable tasks; VDA 5050-compliant |
| `THROTTLE_FEED` | Supervisory setpoint write (cf. CSTR `Fin_sp −0.005` steps); inlet-flow PID tracks | Scale arrival rate ×(1−θ) for horizon H | Quantized steps only; PLC rate limits enforced |
| `SPEED_OVERRIDE` | Supervisory speed factor within envelope; controller enforces accel/stop-distance limits | Scale process time ÷(1−σ) bounded | Envelope-clipped; saturation → throttle instead (CSTR lesson) |
| `DISPATCH_MAINT` | CMMS work order via rules engine (Oxmaint pattern) | Flag machine degraded, derate until cleared | **Escalate-only**, never auto-commit |

---

## 3. Simulate-before-act: how anyone actually implements it

### 3.1 Reference implementation (ctrl-alt-recover / DPPT — auditable, not asserted)

- **Simulation Agent** builds a job: fault-mode spec + initial conditions (PV/SP/MV/modes) + action sequence with timing/ramps + interlocks/envelopes to enforce → DPPT simulation service executes → returns trajectories + end state. Feasible iff no critical violation; else structured failure report (violated constraints, unsafe modes, unmet terminal conditions) → reprompt.
- **Validation Agent (CSTR):** checks actuator validity, unsafe exposure fraction, time-to-safe, T/L limits, persistence of recovery (must reach SAFE and hold ≥60 s in the cited prompt template).
- **Reproducibility/audit:** every run writes self-contained reviewer trace `traces/<case>/<exp>/<model>/<fault>/run_N/` with `metadata.json, events.jsonl, queries/*.sparql, subgraphs/*.ttl, prompts/*.json, responses/*.json, trajectories/*.csv (+metadata), final_result.json, manifest.json`; mixer iteration CSV logs planning-correctness, actuator accuracy, reprompt reason, token counts. This is the bar for "re-runnable/auditable": **seed + model version + KG snapshot + prompt + trajectory CSV**, not a prose claim. Verdandi's seeded SimPy replay already matches this pattern — add manifest + trajectory CSV per verification.
- **Bounded latency + fallback:** reprompt limits prevent infinite loops; no admissible plan in time window → safety fallback (Safety System executes fail-safe). Verdandi: auto-commit only reversible/single-scope; rest escalate.

### 3.2 Horizon / violation-check guidance from adjacent systems

- **Power-grid hierarchical shield (arXiv:2604.14032):** high-level RL proposes abstract actions; deterministic runtime shield filters via **fast forward simulation**; safety as runtime invariant independent of policy quality; every filter event logged. Direct template for Verdandi's RL→shield→SimPy-verify chain.
- **Fab congestion scheduling (arXiv:2608.30520):** learned queue-time predictors as costs inside risk-constrained OR rule (min delivery time s.t. extreme-congestion probability bound). Lesson: keep learner as *cost model* inside a constrained scheduler when horizon is long.
- **Steel MILP-shield scheduling (2026):** PPO over hybrid (inventory + continuous flow + discrete on/off) space with **embedded MILP safety layer** projecting to feasible set. Template for Verdandi's buffer-capacity + AGV-availability constraints.
- **Soft shielding with MCTS (arXiv:2311.12572):** condensed state + PDR-masked fixed action space + Monte-Carlo-tree-search soft shield for long-sequence overdue risk. Directly relevant to delayed-effect misattribution (§6).
- Practical horizon rule from these: **verify horizon ≥ longest transport + queue drain time** (Verdandi: AGV loop time + 31-buffer drain); check unsafe-exposure fraction + hold-SAFE duration, not just terminal state.

### 3.3 Determinism claims — skeptical note

No surveyed manufacturing agent system claims bit-reproducible LLM decisions; determinism claims attach to the **validator/simulator** (seeded model, fixed-step integration, logged initial conditions), while the LLM is treated as a *constrained candidate generator*. DPPT paper scopes twin "in fidelity/coverage/services to meet accuracy/latency of the task" — i.e., calibrated-to-decision, not a full-fidelity clone. Verdandi should claim the same: deterministic *verification*, stochastic *proposal*.

---

## 4. RL action-space designs for flow / AGV control

### 4.1 Discrete vs continuous (consensus for Verdandi scale)

- **Discrete dispatching dominates JSSP/FJSP-AGV:** learn *which dispatching rule / which job-machine-AGV binding* fires at each decision epoch (NeurIPS L2D 2020 lineage; CADRL cooperative agents split operation-sequencing / machine-selection / AGV-selection; MARL real-time framework with action-decoding into high-quality subspace, IEEE TSMC 2024).
- **Continuous only for rates:** feed throttle / speed factor as bounded continuous or quantized steps (cf. CSTR `Fin_sp` increments; TII 2024 cross-attention SAC for hybrid variable-speed + discrete actuators).
- **Verdandi recommendation:** **flat discrete M3.5 vocabulary (7 verbs × masked params) for the agent; same discrete space for RL routing policy; continuous rates quantized** (throttle θ ∈ {0, .1, .2, .3}, speed σ ∈ {0, −.1, −.2}). Inference CPU-only → small policy (MLP/GAT over 32 machines + 31 buffers + AGV states), discrete logits cheap.

### 4.2 Action masking (mandatory)

- **Invalid-action masking** (logits → −∞ for infeasible) standard in FJSP-AGV (e.g., `A_t = D × AV_t`, resample over available AGVs). **CTPN + MBRL (arXiv:2601.04887):** Coloured Timed Petri Net gives formal mask + shrinks search; MBRL lookahead positions AGVs.
- **Policy-based masking under uncertainty (arXiv:2601.09293, Jan 2026):** Weibull machine failures + random arrivals; compares non-gradient (override invalid probs) vs gradient-based (negative gradients on invalid) masking. Verdandi: mask on buffer-full, AGV-busy/charging, machine-down, interlock-denied route, rework-cap-full.
- **PDR-masked fixed space (2311.12572):** dispatching-rule masks generate "fixed and advantageous" space. Verdandi analogue: mask REROUTE targets to lines whose input buffer < 80%.

### 4.3 Safety shields / constrained MDPs / hierarchy

- **Shield taxonomy:** hard action-projection (safety layer w/ linearized costs, OpenReview constrained-HRL); probabilistic shields via safety-value iteration (arXiv:2503.07671); MILP-embedded projection (steel 2026); MCTS soft shield for overdue risk (2311.12572); runtime forward-sim shield decoupled from policy (power-grid 2604.14032 — recommended Verdandi pattern: **SimPy rollout IS the shield**).
- **Constrained HRL:** high-level route/assignment policy + low-level feasibility controller; only low-level monitored but high-level made safety-aware via cost feedback. Maps to Verdandi: RL proposes (line, buffer, AGV), SimPy shield enforces caps/interlocks, violations feed cost.
- **Training:** PPO + mask + shield + GPU in-twin (matches plan: train in-twin on GPU, inference CPU-only). Offline-RL caution: OOD overestimation on unseen actions (Mach. Learn. 2025 offline-JSSP) — keep behavior-cloning warm-start from PDR logs + conservative penalty if using historical data.

---

## 5. Failure modes (incl. contradiction evidence)

### 5.1 Action oscillation / scheduling nervousness [CONTRADICTION-1: rerouting can make things worse]

- **Scheduling nervousness (1980s term, live in 2026 ERP critique):** "small changes ripple and regenerate; AI trained on oscillating schedule learns oscillation as signal, then recommends more of it, faster" (Automating Confusion 2026-08-09). Direct warning for Verdandi: reroute-on-every-alarm amplifies.
- **AGV route oscillation:** "If routes change faster than the environment changes, control logic is amplifying noise" (hycmoop 2026-05-28). Prescribed controls: hysteresis thresholds limiting replan frequency, separate mission vs intersection priority, escape-node reservation, p95 (not mean) travel-time metric, twin calibrated by real telemetry.
- **Nissan Smyrna 2026:** two AMRs dispatched to same spot — "robots fine, software-to-material-flow interface hard" (2026-09-04). Multi-agent dispatch without global reservation = collision.
- **Deadlock-avoidance hurting throughput:** conservative path locking → 4.8s vs 1.3s segment holds, full-fleet reschedules on single sensor fault (hycmoop 2026-04-13).
- **Verdandi mitigations:** per-action cooldown + hysteresis (no repeat reroute for H min unless severity ↑); commit only if SimPy delta-throughput > margin over hold-SAFE baseline; p95 flow-time + deadlock-count in verify gate; reservation-aware AGV mask.

### 5.2 RL underperforming heuristics [CONTRADICTION-2]

- **Offline-RL distributional shift:** OOD action overestimation → "poor or unsafe decisions" on deployment (Offline RL for JSSP, Mach. Learn. 2025; BNAIC 2024 preprint). RL trained on narrow logs loses to PDRs out-of-distribution.
- **Value-vs-policy split:** PPO suits rigid deterministic JSSP; value methods struggle on hierarchical FJSP decomposition (arXiv:2505.03323). Wrong algorithm family for the structure loses to hand-tuned PDR.
- **General RL-for-FTC limits (DPPT §2, citing Tang, Bloor, Sitapure):** low sample efficiency, poor cross-system generalisation, brittle reward design — the paper's explicit justification for LLM-supervisory + simulation-validation over pure RL control.
- **Verdandi mitigations:** PDR baselines (SPT/EDD/CR + nearest-AGV) as both mask generators and beat-to-ship gates; RL ships only if > PDR on seeded fault suite with p95 + worst-cell metrics; keep PDR fallback live (cf. DPPT safety fallback).

### 5.3 Delayed-effect misattribution + multi-fault conflicts

- **Delayed effects:** rework diverts and feed throttles pay off after queue drain; naive credit assignment rewards the *last* action. Mitigations: verify horizon ≥ drain time; hold-SAFE persistence check; MCTS soft shield for long-sequence overdue risk; log action→effect lag per action type for narration.
- **Multi-fault conflicts:** two alarms proposing incompatible actions (reroute A→B vs B→A; throttle vs speed-up). Mitigations: single-scope rule (one action per alarm per window); global conflict check in shield (simulate *combined* action set, not each alone); priority by causal-trace rank + safety severity; escalate on conflict (human resolves).
- **Stale-data confident wrongness:** "model rerouting cold-chain on 6-hour-old reading is wrong with confidence" (IoT World 2026-07-09). Mitigation: freshness gate — refuse auto-commit if twin state older than X or sensor disagreement unresolved.

---

## 6. Proposed M3.5 closed vocabulary (auditable) + verify/actuate per action

> Closed set. Params enumerated. Everything else → escalate.

| # | Action (params) | SimPy simulate | Verify gate (all must pass) | Actuation (real) | Auto-commit? |
|---|---|---|---|---|---|
| A1 | `REROUTE_WIP(lot_set, from_line, to_line, max_qty)` | Rewire routing table for horizon H; check target buffer caps + assembly starvation | Δthroughput > +2% vs baseline; no buffer >95%; no new deadlock; guard permissive holds | MES WO reroute (REST) + supervisory route flag (OPC-UA) | YES if single-scope + reversible |
| A2 | `DIVERT_REWORK(lot_set, reason)` | Push to rework queue; book capacity; add rework delay | Rework queue < cap; drain-time < H; no starvation downstream | MES NCR + AGV transport order (VDA 5050) | YES if cap holds |
| A3 | `DIVERT_BUFFER(lot_set, buffer_id)` | Redirect inflow; level dynamics | Buffer < 80% post-divert; masked otherwise | OPC-UA divert flag; PLC gate executes | YES if masked-pass |
| A4 | `REASSIGN_AGV(agv_id, task_id)` | Rebind + travel-time recompute + reservation check | AGV idle/reassignable; no reservation conflict; Δmakespan ≥ 0 | Fleet API cancel + order (VDA 5050 / OpenRMF / ROOSTER) | YES if no preempt of safety stop |
| A5 | `THROTTLE_FEED(line, theta∈{.1,.2,.3}, H)` | Arrival ×(1−θ) for H | Starvation-free; WIP −X%; hold-SAFE persistence | Supervisory `Fin_sp`-style setpoint step | YES (quantized only) |
| A6 | `SPEED_OVERRIDE(machine, sigma∈{−.1,−.2}, H)` | Process-time scale; envelope clip | Within accel/envelope; saturation → prefer A5 | Supervisory speed factor; PLC clips | YES if envelope holds |
| A7 | `DISPATCH_MAINT(machine, code)` | Derate machine | — (human gate) | CMMS work order | NEVER auto; escalate |

**Global commit rules:** one action per alarm per cooldown window; combined-simulation on multi-alarm; freshness gate; trajectory CSV + manifest logged; reprompt ≤ N then escalate (ctrl-alt-recover pattern).

---

## 7. RL action-space recommendation (for training in-twin, GPU; inference CPU)

- **Space:** MultiDiscrete over {A1..A6} × params (target line/buffer/AGV index, θ/σ level), A7 excluded (human). Mask vector from twin state each step (buffer/machine/AGV/interlock).
- **Policy:** small GAT-or-MLP actor-critic (PPO + invalid-action masking, cf. 2601.09293); hierarchical option: high-level (line/buffer choice) → low-level (AGV binding) with shield at low level.
- **Shield:** SimPy forward rollout as runtime invariant (power-grid pattern) + MILP/capacity projection for buffers; log every veto as training cost (constrained MDP via Lagrange).
- **Reward:** throughput − WIP penalty − p95 lateness − shield-veto penalty − oscillation penalty (action-switch cost); compare vs PDR suite before promotion.
- **Reproducibility:** seed + fault-suite version + policy checkpoint + mask snapshot per episode; CPU inference = forward pass on masked logits only.

---

## 8. Sources + evidence levels

**Evidence scale:** L1 = runnable code / production deployment; L2 = peer-reviewed / preprint with artifact; L3 = vendor/official docs & standards; L4 = trade press / analysis (contradiction signals, mechanism color).

| # | Source | URL / path | Level | Supports |
|---|---|---|---|---|
| 1 | ctrl-alt-recover repo (Imperial AISL) README + code | https://github.com/AISL-at-Imperial-College-London/ctrl-alt-recover | L1 | Supervisory-only agents, FSM + setpoint spaces, reset-then-apply, traces/ contract, reprompt+fallback |
| 2 | Vyas et al., From Detection to Action (DPPT + Graph RAG + 6 agents) | https://arxiv.org/html/2606.28011 | L2 | Pre-execution testing, minimal-risk paths, deterministic interlock/envelope/feasibility checks, bounded-time fallback |
| 3 | Tutorial: Autonomous Fault-Tolerant Control with Knowledge-Grounded LLM Agents | https://arxiv.org/html/2606.31635 | L2 | Executable mixer/CSTR envs, what-to-test checklist |
| 4 | FALCON predecessor repo | https://github.com/AISL-at-Imperial-College-London/fault-handling-agentic-llms-for-controlled-operations | L1 | OpenModelica + LLM corrective-action loop lineage |
| 5 | IETF draft-das-ot-actuation-finality-00 | https://datatracker.ietf.org/doc/html/draft-das-ot-actuation-finality-00 | L3 | Setpoint-write ≠ actuation; enforcement-domain + evidence commit |
| 6 | Niobia PLC/OPC-UA/MQTT brief (read-only boundary) | https://niobia.ai/learn/plc-opc-ua-mqtt | L3/L4 | Deterministic-control rationale for supervisory-only agents |
| 7 | OPC-UA Safety Part 15 (SafetyProvider/Consumer, FSV, OA) | https://reference.opcfoundation.org/specs/OPC-10000-15/7.2 + https://opcfoundation.org/wp-content/uploads/2022/09/OPCF-OPCUA-Safety-EN.pdf | L3 | Safety channel mechanics; why agent avoids it |
| 8 | Industrial Monitor Direct: OPC-UA not for interlocks/safety | https://industrialmonitordirect.com/blogs/knowledgebase/siemens-to-rockwell-plc-integration-opc-server-vs-gateway | L3 | Interlock path must be fieldbus, not OPC-UA |
| 9 | Siemens Opcenter Automation Gateway (OPC-UA↔WO binding, REST/RIC) | https://www.siemens.com/en-us/company/insights/industrial-operations-x/architecture-hub/op-center/ | L3 | MES hold/reroute/dispatch verbs via REST, tag binding via OPC-UA |
| 10 | OpenRMF `reassign_dispatched_tasks()` | https://github.com/open-rmf/rmf_ros2/blob/main/rmf_fleet_adapter/include/rmf_fleet_adapter/agv/RobotUpdateHandle.hpp | L1 | AGV re-task primitive + same-fleet constraint |
| 11 | ROOSTER fleet manager (`/place_order`, pending/active jobs) | https://github.com/ROOSTER-fleet-management/rooster_fleet_manager | L1 | Fleet task-allocation API shape |
| 12 | VDA 5050 fleet-manager side project (REST/WS + MQTT) | https://github.laiyagushi.com/justgoogledit/fleet-manager | L1 | cancelOrder/pause/charge controls |
| 13 | TARS VDA 5050 fleet manager | https://github.com/AI-SPARC/fleet-management | L1 | AGV allocation/routing stack reference |
| 14 | OpenFMS fuzzy dispatch + traffic resolution | https://github.com/hazeezadebayo/OpenFMS | L1 | Dispatcher + conflict-resolution pattern |
| 15 | CTPN + MBRL FMS w/ dynamic masking + AGV lookahead | https://arxiv.org/pdf/2601.04887 | L2 | Formal mask + lookahead preposition |
| 16 | PPO + action masking under arrivals/failures (Weibull) | https://arxiv.gg/abs/2601.09293 | L2 | Mask strategies (gradient vs non-gradient) |
| 17 | Invalid-action masking FJSP-AGV (`D × AV_t`) | http://arxiv.org/pdf/2305.13824v1 | L2 | Masked resampling math |
| 18 | GAT + DRL FJSP-AGV / energy-saving multi-AGV | https://doi.org/10.1007/s10696-026-09671-8 | L2 | GAT policy architecture |
| 19 | MARL real-time FJSP-AGV + action decoding | https://doi.org/10.1109/tsmc.2024.3520381 | L2 | 3-way allocation + decoding into good subspace |
| 20 | Safe RL assembly lines: PDR masks + MCTS soft shield | https://arxiv.org/abs/2311.12572v1 | L2 | Overdue-risk shield, condensed state |
| 21 | Hierarchical shield: RL proposes, deterministic sim filters (power grid) | https://arxiv.org/html/2604.14032v1 | L2 | Runtime-invariant shield template |
| 22 | RL + MILP safety layer, hybrid space (steel) | https://doi.org/10.21227/3qd2-zt33 | L2 | Embedded projection for flow+on/off |
| 23 | Safe demand-response: Lagrange SAC hybrid actions | https://doi.org/10.1109/tii.2024.3514183 | L2 | Continuous+discrete under constraints |
| 24 | Offline RL JSSP: OOD overestimation [CONTRADICTION-2] | https://doi.org/10.1007/s10994-025-06826-w | L2 | Why RL loses to PDRs OOD |
| 25 | Rainbow/value-vs-policy scheduling analysis | https://doi.org/10.48550/arxiv.2505.03323 | L2 | Algorithm-family mismatch failure |
| 26 | Scheduling nervousness / AI amplifies oscillation [CONTRADICTION-1] | https://automatingconfusion.substack.com/p/twelve-erps-shipped-ai-none-of-them | L4 | Reroute-every-alarm failure mechanism |
| 27 | AGV traffic-control failure modes (oscillation, deadlock, starvation) | https://www.hycmoop.com/news/robotics-automation/agv-amr/When-does-an-AGV-traffic-control-algorithm-fail.html | L4 | Replan-hysteresis, escape nodes, p95, twin calibration |
| 28 | Deadlock-avoidance hurting throughput (4.8s vs 1.3s holds) | https://www.hycmoop.com/news/robotics-automation/agv-amr/AGV-traffic-control-algorithm--Why-deadlock-avoidance-slows-throughput-more-than-congestion.html | L4 | Conservative-locking cost |
| 29 | Nissan Smyrna AMR pileup (interface, not robots) | https://www.thenews.com.pk/latest/1414843-nissans-factory-robots-got-stuck-in-their-own-traffic-jam | L4 | Dispatch-without-reservation failure |
| 30 | Fab congestion-aware scheduling (−16.4%/−22.6%) | https://arxiv.org/abs/2608.30520 | L2 | Learner-as-cost-inside-constrained-scheduler |
| 31 | Stale-data confident rerouting failure | https://iotworld.co/2026/07/ai-cannot-act-on-what-operations-cannot-see/ | L4 | Freshness-gate rationale |
| 32 | Agentic Digital Thread for aerospace NCRs | https://doi.org/10.4271/2026-26-0763 | L2 | Master-Agent NCR corrective-action orchestration |
| 33 | MQTO ontology (countermeasures) | https://doi.org/10.5281/zenodo.19571023 | L2/L3 | Countermeasure taxonomy grounding |
| 34 | RAG failure-recovery recommender (IEOM 2026) | https://ieomsociety.org/proceedings/bangkok2026/468.pdf | L2 | KG+RAG corrective-action synthesis |
| 35 | US 12085930 note | No direct fetch (patent DB not queried this pass) — treat as related-art pointer only; DPPT + ctrl-alt-recover above are the citable mechanisms | L3* unverified | Do not cite as mechanism until verified |

**Queries executed (10):** (1) corrective-action taxonomy reroute/rework/divert agent twin 2026; (2) OPC-UA/PLC actuation + safety interlock boundary; (3) AGV fleet-manager API reassignment; (4) simulate-before-act / pre-execution testing horizon; (5) RL action space routing/AGV masking/shield 2026; (6) autonomous rerouting failure/oscillation [contradiction]; (7) RL dispatch underperform heuristic [contradiction]; (8) ctrl-alt-recover Imperial twin rollouts; (9) MES hold/quarantine/divert/rework + OPC-UA/PLC; (10) constrained-MDP / hierarchical shield flow control.

---

## 9. Viva-ready boundary story (one paragraph)

Verdandi's correction agent is a *supervisory candidate generator*, never a controller: it emits closed-vocabulary candidates (A1–A6) that are rolled out on the seeded SimPy twin over horizon H ≥ AGV-loop + buffer-drain time and checked for interlock/guard permissives, buffer/AGV feasibility, unsafe-exposure fraction, and hold-SAFE persistence with full trace artifacts; only reversible single-scope passes auto-commit, conflicts and A7 escalate, and any timeout falls back to a safe hold. Real-plant twins of this pattern bind via MES REST (hold/quarantine/reroute/dispatch) and OPC-UA supervisory nodes, while PLC interlocks, PID loops, and the safety channel (OPC-UA Safety / fieldbus) retain veto — a setpoint write is not actuation.

---

*File: `follow-ups/03-agent-actions-topology-physics/01-background/agent-actions.md` — production mechanisms only; tutorials skipped per brief.*
