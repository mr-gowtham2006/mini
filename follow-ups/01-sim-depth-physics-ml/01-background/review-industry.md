# Industry review: production sensing, historians, ML training data, RCA practice (Verdandi twin upgrade)

Date: 2026-09-13. Scope: INDUSTRY evidence only (official docs, standards, practitioner guides, engineering reports, credible blogs, OSS repos). Peer-reviewed theory is a teammate's lane — excluded here.
Twin baseline: 32-machine SimPy DES (SIM_SPEC §§2–8; config.py Table 3.1), 7 channels today (vibration/signal obs, temperature, throughput, quality flag, state, buffer level, event log). M0.2 acquisition pending (Linear MINIPRO-17).

## 1. Search strategy log (verbatim queries, retrieved → screened → kept)

| # | Verbatim query | Angle | Retrieved | Screened | Kept |
|---|---|---|---|---|---|
| Q1 | factory historian sampling rates vibration temperature motor current OSIsoft PI AVEVA Siemens channels logged | (a) historian channels/rates | 8 | 8 | 5 |
| Q2 | Siemens Insights Hub AVEVA digital twin production physics modeling capabilities limitations | (b) twin platforms | 8 | 8 | 4 |
| Q3 | PTC ThingWorx GE Vernova digital twin manufacturing production deployment machine monitoring | (b) twin platforms | 8 | 8 | 3 |
| Q4 | OPC UA companion specification machinery monitoring sampling rate ISA-95 IEC 62264 historian MES | (a)+(standards) | 8 | 8 | 3 |
| Q5 | ML anomaly prediction factory training data labeling class imbalance windowing LSTM transformer vibration | (c) ML data design | 8 | 8 | 4 |
| Q6 | sim-to-real transfer manufacturing synthetic data lessons learned factory simulation | (c) Sim2Real | 8 | 8 | 4 |
| Q7 | alarm flood management EEMUA 191 cause effect matrix operator root cause cascade manufacturing | (d) RCA practice | 8 | 8 | 4 |
| Q8 | PyRCA root cause analysis production deployment topology causal graph microservice factory | (d) RCA tooling | 8 | 8 | 3 |
| Q9 | ISO 13374 MIMOSA OSA-CBM condition monitoring data processing blocks vibration standard | (a)+(standards) | 8 | 8 | 3 |
| Q10 | motor current energy meter compressed air pressure OEE downtime codes factory data collection practice | (a) missing channels | 8 | 8 | 5 |
| Q11 (contradiction) | digital twin ROI failure manufacturing disappointing results why twins fail plant | contradiction | 8 | 8 | 5 |
| Q12 (contradiction) | predictive maintenance false positives plant alert fatigue failed deployment lessons | contradiction | 8 | 8 | 5 |

Total: 96 retrieved → 96 screened (title/snippet relevance) → ~20 unique sources kept (dedupe across queries). Levels: L3 = official docs/standards, L4 = practitioner/engineering/vendor-technical, L5 = opinion/blog. Full table: `source-table-industry.md`.

## 2. Thematic synthesis

### T1. What real plants actually log (historian practice)

Consensus (HIGH): plants log far more channels than our 7, at tiered rates. OSIsoft/AVEVA practice [I03]: raw vibration waveforms run ≥10k values/sec for seconds-long bursts, FFT'd, then only derived features (RMS, envelope, spectral bands) are historized — waveforms are NOT stored continuously. PI scan classes [I04]: 1 s (fast loops/alarms), 5 s (process values/flows/temps), 20 s (slow analogs/levels), 10 min (totals/energy/lab). Cement/power-plant PI integrations [I05] converge on the same tag shortlist: vibration, temperature, pressure, motor current draw, flow rate, speed — for 15k–80k tags/plant, filtered to maintenance-relevant signals via AF asset hierarchies. Temperature is logged with rate-of-change deadbands (e.g. 0.25 °F/day) [I03], not per-second values.
Implication: our 1 sample/step for vibration+temperature is a historian-grade derived-feature rate, NOT a waveform rate — defensible for MES-level twins, indefensible if we claim bearing-fault physics. OPC UA for Machine Tools [I08] explicitly caps MES/analytics use cases at 1 Hz max sampling as sufficient.

### T2. The channels we miss that plants consider basic (MINIPRO-17 input)

Consensus (HIGH): motor current, energy, compressed-air pressure, OEE/downtime codes, and operator/shift context are first-class plant signals, and every one is absent from our twin.
- Motor current: clip-on current sensors are the default retrofit for legacy machines (no PLC integration needed) [I19]; current-interval monitoring feeds OEE directly [I18].
- Energy: plants start from existing line master meters + main-motor currents; energy-per-unit tied to the OEE event stream is the standard calculation (ISO 50001 driver) [I16, I17]. Idle-but-powered machines still draw energy/air/cooling for zero output — the exact signal our DOWN/BLOCKED/STARVED offsets cannot produce.
- Compressed air: header + receiver + branch taps with PLC cycle signals or current-sensor activity proxies; 2–4 week deployments minimum to capture shift/maintenance variability [I20].
- Downtime/OEE codes: 3-level reason trees (category → subcategory → specific code); every unplanned stop ≥2 min gets a specific code, never "Other" [I21, I22]. Operator-entered codes + shift handover notes are the labeling substrate ML teams actually train on — and their inconsistency is the #1 data complaint [I12, I13].
- What this means: our channel 5 (state) + channel 7 (events) approximate downtime codes, but without reason-code hierarchy, shift context, or energy/current coupling.

### T3. What production twin platforms model — and skip (Siemens / AVEVA / PTC / GE)

Consensus (MEDIUM-HIGH): commercial "twins" are overwhelmingly data-integration + KPI/visualization layers over historian/MES data, NOT multi-physics simulators.
- Siemens Insights Hub Factory Twin = Technomatix Plant Simulation DES fed by shopfloor IIoT data (bi-directional, headless) — i.e. flow/queue simulation like ours, not thermal/electrical physics [I06]. Deep multi-physics lives in Simcenter Executable Twin via reduced-order modeling for real-time use [I07] — explicitly a compression of offline FE/CFD, never run at full fidelity online.
- AVEVA: PI System (historian) + AF asset models + Event Frames for alarm/upset capture [I05]; AVEVA Process Simulation as operating twin for unmeasured-variable inference [I07-note]. Physics is steady-state process simulation, not transient machine physics.
- PTC ThingWorx: Kepware connectivity → real-time condition monitoring, OEE/line-performance dashboards, "MES-lite" bottleneck/time-loss views [I09]. Bharat Forge zero-downtime and Eaton Factory Insights cases report OEE/downtime outcomes, no physics claims.
- GE Vernova Proficy CSense: process digital twins claiming waste −75%, quality complaints −38%, throughput +5–20%, OEE +10% [I10] — vendor numbers, treat as L4-marketing, but the pattern (statistical/ML process models, not first-principles) is consistent across all four vendors.
- Net: NOBODY ships per-machine transient thermal + electrical + pneumatic co-simulation at production scale. The industry consensus architecture is DES flow + statistical/ML anomaly layer + historian — which validates our SimPy-core choice and says physics upgrades should be lumped-parameter (first-order lags), not FE-grade.

### T4. ML training-data design for factory anomaly prediction (practitioner consensus)

Consensus (HIGH on imbalance/normal-only; MEDIUM on windowing specifics):
- Normal-only training is the norm: far more normal than fault data, so two-class training biases to the majority class; standard practice labels pre-maintenance windows anomalous / post-maintenance normal and trains one-class or autoencoder models [I11]. Free sim ground truth (our `faults[]`) is therefore a genuine structural advantage — practitioners wish they had it [I12].
- Imbalance handling: SMOTE-family oversampling on windowed FFT features is the published production-line recipe (50% overlap sliding windows → FFT → oversample minority) [I13]; imbalanced-learn's mandated pattern is resampling INSIDE the CV pipeline (leakage otherwise) [I15]. Any ML dataset we emit must be split by episode/seed with resampling inside folds, never globally pre-balanced.
- Windowing: vibration practice = fixed windows + 50% overlap + FFT per window [I13]; prediction horizons in production are days (14-day failure window in the Nebulaworks deployment [I12]), not steps. Our T=300 episodes are micro-windows by that standard — fine for detector training, insufficient for prognostics without episode chaining.
- Sim2Real (transfer lessons, HIGH consensus on direction): synthetic-only models collapse on real data (RF-DETR detector 0.2516 mAP sim-only → 0.8853 with just 50 paired real images + 500 synthetic via diversity sampling [I14]); domain randomization (lighting/material/geometry) beats photorealism for reflective industrial parts [I14b]; concept-centric review of 44 studies structures transfer as modeling→learning→transfer→validation pipeline stages [I14c]. NVIDIA's assembly sim-to-real hits 83–99% over 600 trials but only with PLAI + careful real-world system design [I14d]. Lesson for us: domain-randomize fault params (we already sample mag/dur/d/drop/mult/reject ranges — good), keep a real2sim2real hook open, and NEVER claim transfer without paired-real validation.

### T5. Production RCA / traceback practice (how operators actually trace cascades)

Consensus (HIGH): operators do NOT walk causal graphs. They fight alarm floods with management systems, and RCA is a post-hoc engineering activity.
- EEMUA 191 (the global alarm-management standard [I23]): target ≈1 alarm/10 min; floods (10+ alarms/10 min) are a documented contributor to Three Mile Island/Bhopal/Milford Haven-class incidents. ABB/IChemE practical guide [I24]: every alarm needs defined response, rationalization, and review — alarm review minimizes count consistent with protection.
- Causal reality: flood alarms ARE causally related along material/energy/information flow from one root cause [I25]; cause-effect analysis combining process + alarm data is the decision-support pattern [I26]; Bayesian-network flood-reduction with explicit root-cause identification exists but is advisory [I27].
- PyRCA (Salesforce OSS, the closest production RCA tooling [I28]): two algorithm families — (1) anomalous-metric detection (ε-diagnosis) and (2) topology/causal-graph scoring (Bayesian inference, random walk); PC causal discovery with domain-knowledge constraints (root/leaf nodes, forbidden/required links) via dashboard; benchmark + simulation-data generator included. Built for microservice KPIs, NOT factory flow — but its topology-mask + walk + domain-constraint pattern is structurally identical to our veto-mask + depth-≤3 walk + partition design. That convergence is independent validation of our architecture.
- Gap: our walk has no alarm-flood concept (one fault → one alarm path). Real plants get 10–100 alarms per root cause; grouping causal sequences into one HMI entry [I25] is the operator-facing requirement our narration layer should eventually mirror.

## 3. Consensus levels

| Claim | Level | Basis |
|---|---|---|
| Historians store derived vibration features (RMS/envelope/bands), not waveforms | HIGH | [I03, I04, I05] converge |
| 1 Hz sufficient for MES-level machine monitoring | HIGH | OPC UA Machine Tools spec [I08] + PI scan classes [I04] |
| Motor current / energy / air pressure / downtime codes are standard plant signals | HIGH | [I16–I22] converge |
| Production twins = DES + historian + ML, not multi-physics | MEDIUM-HIGH | 4 vendors converge; vendor marketing discounts applied |
| Normal-only training + in-pipeline resampling for imbalance | HIGH | [I11, I13, I15] |
| Synthetic-only models fail on real data; small paired-real sets fix most of it | HIGH | [I14, I14b, I14c, I14d] |
| Alarm floods managed via EEMUA 191 rationalization, not causal walk | HIGH | [I23, I24, I25] |
| Topology/walk RCA with domain constraints is the production pattern (PyRCA) | MEDIUM | single OSS ecosystem, microservice-origin |
| Vendor ROI numbers (GE waste/throughput, PTC downtime) | LOW | vendor case studies, no independent audit |

## 4. Mapping: each practice → CURRENT TWIN GAP + MINIPRO-17 acquisition plan

Current twin (7 channels) vs plant reality. Ranked by ML-value per CPU cost (ML-engineer POV per CONTEXT scope decision).

| # | Plant practice | Twin gap | Acquisition proposal (MINIPRO-17) | Priority |
|---|---|---|---|---|
| G1 | Motor current per machine (default retrofit signal [I18, I19]) | NO current channel; DOWN/BLOCKED/STARVED invisible electrically | CH8 motor current: `I = I_idle + k·load + faultdev`, idle>0 so DOWN still draws standby; load from buffer/cycle state | P0 — cheapest high-value channel, couples every state to a physical signal |
| G2 | Energy-per-unit + idle power waste [I16, I17] | No energy; idle machines cost nothing in-twin | CH9 energy: integrate CH8 per part (`kWh/part`); report idle-energy waste per episode | P0 — derived from CH8, near-zero CPU |
| G3 | Compressed-air pressure header/branch [I20] | No pneumatics; no shared-resource physics beyond AGV | CH10 air pressure: plant-level header `P(t)` first-order lag + per-cycle draw dips; BLOCKED machines hold pressure, STARVED drop draw | P1 — adds genuine shared-resource coupling (air as second AGV-like contention) |
| G4 | OEE/downtime reason codes, 3-level, ≥2 min coded [I21, I22] | State+events exist but no code hierarchy, no shift context | Extend CH7: reason-code enum on DOWN/BLOCKED/STARVED transitions (3-level tree, ≤30 codes covering 70–80% modes per [I22]); add shift-id + operator-note stub fields | P1 — unlocks supervised labeling parity with plant data |
| G5 | Temperature RoC deadbands, slow thermal dynamics [I03] | Temp = uniform band, NO thermal inertia (CONTEXT) | Thermal inertia: `T += (T_target(state,load) − T)/τ`, τ per class; T_target elevated under BLOCKED (heat soak) | P1 — first-order lag only, keeps <600s budget |
| G6 | Vibration RMS/envelope historized at 1 Hz; waveforms burst-only [I03, I04, I08] | Single obs signal at 1/step; no RMS-vs-waveform distinction | Keep 1/step obs as "RMS-grade"; add documented CLAIM BOUNDARY: twin does not resolve bearing frequencies; optionally burst-mode waveform stub for 1–2 machines | P2 — mostly documentation + naming |
| G7 | 14-day prediction horizons; overhaul resets baseline [I12] | T=300 micro-episodes; no maintenance-event resets | Episode chaining for prognostics (future milestone); add MAINT_EVENT flag resetting rolling history (mirrors [I12] Problem 4) | P2 — design hook now, build later |
| G8 | Sensor-health/droupout flags as first-class features [I12] | Loss fault exists but no sensor-availability indicator | Add sensor-health flag channel on CH1/CH2 during loss windows (mirrors Nebulaworks fix) | P1 — small, high ML-training value |
| G9 | EEMUA 191 flood management; causal-sequence grouping [I23, I25] | One fault → one alarm path; no flood concept | Future: multi-fault episodes + alarm-grouping narration (chain-cards already the fallback per SIM_SPEC K3) | P2 — narration-layer work, not acquisition |
| G10 | Sim2Real: domain randomization + paired-real validation [I14–I14c] | Fault params already randomized (good); no real-data hook | Keep FAULT_RANGES randomization; document transfer-claim boundary: no Sim2Real claim without paired-real validation | P0 — documentation, zero CPU |

Sampling-rate recommendation (from T1+T3): keep twin step = 1 sample for channels 1–3, 5–6, 8–10 (MES/historian grade, consistent with OPC UA 1 Hz [I08] and PI scan classes [I04]). Do NOT add sub-step integration except the thermal first-order lag (analytic update, no substeps). No waveform-rate simulation — industry itself doesn't historize it.

## 5. Gaps & Contradictions (nothing suppressed)

C1. **The twin-fidelity trap (STRONG contradiction).** Forbes (2026) [C01]: twins failed because they "modeled assets instead of the web of decisions, data flows, incentives and human behaviors" — simulation engines were never the bottleneck. Industry4-1 fab-floor case [C02]: an ACCURATE twin predicted a bearing failure 5 days out and the plant ran the machine anyway — no decision authority in the loop. Gartner via Appit [C03]: 75% of manufacturing twin programs stall past pilot; only ~15% escape pilot [C04]. Direct challenge to this follow-up: deeper physics does not fix adoption/authority/data-foundation failures. Counter-position: our wedge (ranked-trace + provenance + replay, F6) IS a decision/authority interface, not more physics — the upgrade spec should weight trace-explainability ≥ physics depth.
C2. **Alert fatigue vs detection sensitivity (STRONG contradiction).** Production PdM postmortems [C05, C06, C07]: false positives destroy operator trust faster than misses destroy value — "85% accurate" models get muted; zero-value sensor readings misread as bearing failure spiked FP rates [I12]; start-ups/changeovers must be filtered before escalation [C08]. Challenge: our quantile detector + 4–7σ fault mags may be tuned for sensitivity in a way plants would reject. Counter-position: VETO_ASM2 + flip-gate + chain-cards fallback are already precision-side mechanisms; the spec should add a precision-side acceptance lens (alerts/1000 healthy windows, cf. [I13b]).
C3. **ROI is back-loaded and conditional (MEDIUM).** Twin value appears only after slow trust curves; first months are pure cost (instrument, clean, integrate, validate) [C09]; maintenance-plan pairing required — vendors' 18-month payback claims don't survive year-2 labor/spares/training accounting [C10]. Challenge: MINIPRO-17 channel additions have integration-style costs even in-twin (calibration, threshold re-tuning per channel). Implication: each new channel must ship with its Q_DET calibration + noise spec, not just its equation.
C4. **Generic models fail; context is everything (MEDIUM).** Saudi-plants analysis [C06b]: failure mode is "poor integration, generic data modelling," alerts cut off from work-tracking get muted; Ordo [C06c]: unclear asset mappings + incomplete operating-state info break industrial AI before the model runs. Supports our partition/topology-mask design (plant-specific structure, not generic graph learning — consistent with killed learned-graph per SIM_SPEC §1.3).
C5. **Vendor outcome numbers are unaudited (disclosure).** GE Proficy (waste −75%, OEE +10%) [I10], PTC case studies [I09] are marketing-grade evidence. Used here only for architecture-pattern claims (DES+dashboard+ML), never for effect sizes.

## 6. What this means for the twin upgrade spec (handoff to 04-synthesis)

1. Add CH8 (current) + CH9 (energy, derived) + CH10 (air pressure, header-level) + reason-code hierarchy + thermal inertia + sensor-health flags. Everything else is documentation/boundaries.
2. Keep SimPy core, 1 sample/step, T=300, <600s. No sub-step physics; first-order lags only.
3. Dataset design: episode-seeded splits, resampling inside folds, normal-only training support, MAINT_EVENT reset flags, sensor-availability features.
4. Claim boundaries: no bearing-frequency resolution, no Sim2Real claim without paired-real data, precision-side metrics alongside F1.
5. Weight trace-explainability ≥ physics depth (C1 lesson): the wedge is ranked-trace + provenance + replay, and physics serves it.

Source details: `source-table-industry.md`. No peer-reviewed theory covered (teammate lane). No sources invented; vendor claims labeled as such.
