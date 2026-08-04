# EBIS Runtime Stabilization Report — v25 Session

## Runtime Session Information

This document is the primary runtime evidence record for the EBIS v25 autonomous stabilization session. It contains the complete, unedited console transcript from boot through shutdown, reproduced verbatim per the project's evidence-preservation policy. No lines have been summarized, reordered, or removed.

This session is distinct from the v227 forensic causal-discovery audit covered in `PROJECT_STATUS_AUGUST_2026.md`. Where that audit exercised causal discovery only (trust held at 0.00 throughout, no actuator interventions), this session exercises the closed-loop control path: causal discovery, an autonomous multi-node actuator experiment, and two live stabilization attempts on `ca50`.

## Session Metadata

| Field | Value |
|---|---|
| Build | `ebis_DEV_BUILD_v25_B0_P6_OPEN_WITH_DOCS.zip` (188 entries, integrity OK) |
| Project root | `/content/ebis_extracted` |
| Version | v25 |
| Boot signature | `39bc7861` (boot) / `39bc78610bdf` (final session signature) |
| Files / Modules / Templates | 62 files · 7/7 modules · 12 templates |
| Boot mode | manual |
| Boot time | 13.8s |
| Boot warnings | 4 module(s) loaded from `legacy/` (shadowing `src/`); CER not live |
| Restored investigation context | branch=`evidence verification`, last target=`ca50`, 6 variables |
| Primary targets investigated | `ca50`, `spark_angle_deg` |
| Experiments recorded (session total) | 40 |
| Sensitivities recorded (session total) | 178 |
| Session result | PASS |
| Session duration | 3522.9s (~58.7 minutes) |

## Engineering Description

The session opened with a runtime integrity check (`boot check`), confirming the environment booted cleanly with a stable orchestrator identity and no stale threads. The engine was then started (`engine chalao`), reusing an externally-built 172-node actuator surface and warming up before entering a live cycle buffer.

A full signal inventory was taken (46 numeric buffer variables), followed by two manual column drops (`ca10`, `ca90`) ahead of the first DIFC causal-discovery run. `ca50` was analyzed via DIFC, producing 21 accepted edges from 200 cycles of data; the resulting graph was independently reliability-verified at full coverage (21/21 hops) by a background verifier. A `spark_angle_deg` analysis was run separately, producing 14 accepted edges (14/14 hops verified).

An autonomous multi-node experiment (`full experiment karo`) then swept 12 actuator nodes against `ca50` (phasing, closed-loop turbulence, Wiebe, and kernel parameters), running 12 sub-experiments across 960 cycles. This produced 2 strong intervention verdicts, 12 weak, and 7 insufficient, each logged with an evidence-audit trail.

Two autonomous stabilization attempts (`ca50 stable karo` / `stable karo`) followed. The first failed its verification check (oscillation above threshold) after three closed-loop re-nudges; the system detected its own causal graph had gone stale mid-attempt and auto-triggered a fresh DIFC run. The second attempt, run against the refreshed graph, passed verification and held on a +30s persistence check. A single reversion-based prediction for `ca50` was scored against a later observed value and logged as a verified hit. The session closed with a clean shutdown (`bye`), preserving 40 experiments and 178 sensitivities.

## Complete Runtime Output (verbatim)

```text
================================================================
  EBIS MASTER LAUNCHER
================================================================
  ✓ ZIP located   : /content/ebis_DEV_BUILD_v25_B0_P6_OPEN_WITH_DOCS.zip
  ✓ Integrity OK  : 188 entries, no corrupted members
  ✓ Extracted     : /content/ebis_extracted
  ✓ Project root  : /content/ebis_extracted
════════════════════════════════════════════════════════════
  EBIS v25  ·  boot verified  ·  sig 39bc7861
  62 files · 7/7 modules · 12 templates · manual mode · 13.8s
  ⚠ 4 module(s) loaded from legacy/ (shadowing src/) — run 'boot check' · CER not live  (run 'boot check' for detail)
════════════════════════════════════════════════════════════
  Starting EBIS telemetry…
  ✓ stdout mirror installed (terminal output unchanged)
  ✓ telemetry server started on port 8771

Starting public tunnel...
Waiting for DNS...
✅ Public dashboard verified
https://sender-macro-scenic-flux.trycloudflare.com
────────────────────────────────────────────────────────────
  🟢  EBIS Live Dashboard (read-only, external observer)
      https://sender-macro-scenic-flux.trycloudflare.com
      click → opens in a new tab · auto-updates · closing it does NOT stop EBIS
────────────────────────────────────────────────────────────

Public Dashboard URL:
https://sender-macro-scenic-flux.trycloudflare.com

Sharing Status:
✅ Public
✅ All modules imported successfully

  EBIS · calm but observant
  28 exp · signal 0.00 · 0 edges
  ↩ Investigation context restored: branch=evidence verification  last=ca50  vars=6

ENGINE VIEWPORTStatus: RUNNINGCycle : 7045RPM   : 2000AFR   : 14.7CA50  : +9.1°Spark : 24.9°Knock : 🔴HIGHOsc   : 0.63
────────────────────────────────────────────
  You ▸ boot check
  [ROUTER] CHAT

  ─ RUNTIME INTEGRITY ─
    verdict: ✓ OK
    env_booted            : True
    env_root              : /content/ebis_extracted
    sys_path_root_pos     : 2
    orch_present          : True
    orch_lifecycle        : EMPTY
    orch_buffer_size      : 0
    orch_id_stable        : True
    signature_match       : True
    stale_threads         : []


────────────────────────────────────────────
  You ▸ engine chalao
  [ROUTER] COMMAND

→ Resuming engine from cycle 0
  [engine] building kernel...
  [engine] reusing externally-built kernel + hub (172-node actuator surface)
  [boot] STATE.theta_hub mirrored from constructor_or_internal_build
  [boot] hub config 'engine' registered — kernel constants ab first-class: 'set node engine/<attr> <value>'
  [engine] kernel config (24 numeric attrs): AFR=14.7 · EVC=370.0 · EVO=130.0 · IVC=560.0 · IVO=340.0 · LHV=44000000.0 ...
  [engine] (full values: 'system inventory' → KERNEL CONFIGURATION)
  [engine] warming up (25 cycles)...
  [engine] live — 5 cycles in buffer

────────────────────────────────────────────

  [observer] unknown (n=28 experiments)
  You ▸ help
  [ROUTER] CHAT

  ─ EBIS RUNTIME STATE ─
    organism:         blocked_by_code NO · blocked_by_evidence YES · 1 open · top: graph drift validation   (type 'status')
    engine:           running (buffer 200) · cycle 240
    DIFC:             idle
    reliability:      idle  ·  prediction-verify pending: 0  ·  background jobs: 2 {'engine': 1, 'render': 1}
    commit:           v0  ·  investigation: ca50
    persistence:      restored 261s ago
    graph:            EMPTY · age 240 cyc
    active targets:   ['ca50']
    buffer vars:      46 numeric · 4 low-variance
    edges:            0 valid · 0 unknown · 0 rejected
    experiments:      28 recorded
    epistemics [ca50]: trust(T)=0.82 · latent(L)=n/a · uncertainty(U_total)=0.25
    economy:          0.0 know-value/CPU-sec · 61.169 CPU-sec · 0.0 know-value

  ─ WHAT YOU CAN DO NOW ─
    > drivers dikhao           (inspect current causes)
    > full experiment karo     (perturb + measure)
    > signal inventory         (all buffer vars + health)
    > signal inventory <var>   (full card: value/description/node/unit/authority/driver)
    > system inventory         (kya exist karta · kya connected — connection audit)
    > show valid|unknown|rejected  (edge explorer)
    > graph evolution          (how edges changed across runs)
    > timeline                 (investigation history)
    > freshness                (engine-vs-graph age)
    > trust evidence           (trust store breakdown)
    > efficiency               (knowledge per compute · DCU)
    > glossary                 (what knowledge/efficiency/trust/signal mean)
    > ab kya karu               (narrator recommends next step)


  ─ HELP BRANCHES (type to open) ─
    help all            EVERY command, grouped (full list)
    help commands       COMMANDS (engine + analysis)
    help system         SYSTEM & RUNTIME (status · health · integrity)
    help explore        EXPLORE (read the causal graph)
    help experiments    EXPERIMENTS (perturb + measure)
    help control        CONTROL (act on the engine)
    help trust          TRUST & ECONOMY
    help narration      NARRATION (ask the organism)
    help glossary       GLOSSARY


────────────────────────────────────────────
  You ▸ signal inventory
  [ROUTER] CHAT

  ─ BUFFER INVENTORY (46 numeric vars, target ca50) ─
    air_fuel_ratio                 var=0          cur=14.7 ⚠low-var ·no edges
    blowdown_intensity             var=0.03334    cur=3.795 ·no edges
    breathing_efficiency           var=0.000581   cur=0.4805 ·no edges
    burn_acceleration              var=2.3e-05    cur=0.04203 ·no edges
    burn_duration                  var=1.585      cur=19.99 ·no edges
    ca10                           var=7.196      cur=-11.99 ·no edges
    ca50                           var=13.89      cur=0.004349 ·no edges
    ca90                           var=13.98      cur=7.999 ·no edges
    charge_density                 var=0.00263    cur=1.025 ·no edges
    combustion_stability           var=9.3e-05    cur=0.9659 ·no edges
    compression_heating            var=0.000528   cur=1.418 ·no edges
    compression_heating_ratio      var=0.000528   cur=1.418 ·no edges
    dilution_penalty               var=0          cur=4.179e-05 ·no edges
    discharge_efficiency           var=0.000257   cur=0.518 ·no edges
    engine_speed_rpm               var=0          cur=2000 ⚠low-var ·no edges
    exhaust_backpressure_ratio     var=0.004873   cur=0.9097 ·no edges
    exhaust_pressure_pa            var=3.434e+05  cur=1.016e+05 ·no edges
    exhaust_temperature_k          var=70.8       cur=775.2 ·no edges
    exhaust_velocity               var=1.407      cur=23.86 ·no edges
    flame_speed_peak               var=0.2123     cur=8.036 ·no edges
    flow_stability_index           var=0.002336   cur=0.9296 ·no edges
    ignition_delay_index           var=4.234      cur=-18 ·no edges
    indicated_work_j               var=1096       cur=620.1 ·no edges
    intake_temp_k                  var=591.3      cur=389.9 ·no edges
    intake_velocity                var=24.41      cur=103.2 ·no edges
    knock_intensity                var=8.254      cur=29.69 ·no edges
    manifold_pressure_pa           var=5.368e+07  cur=1.147e+05 ·no edges
    manifold_temp_k                var=471.6      cur=323.2 ·no edges
    material_liner_safety_factor   var=0.06018    cur=2.597 ·no edges
    material_liner_stress_mpa      var=58.52      cur=96.28 ·no edges
    mean_flame_speed               var=0.04131    cur=3.366 ·no edges
    ncm_residual_mass_kg           var=0          cur=2.409e-05 ⚠low-var ·no edges
    ncm_residual_pressure_pa       var=2.785e+05  cur=9.935e+04 ·no edges
    ncm_residual_temp_k            var=268.5      cur=868.9 ·no edges
    peak_pressure_pa               var=5.064e+11  cur=8.956e+06 ·no edges
    prev_residual_mass_kg          var=0          cur=2.378e-05 ·no edges
    prev_residual_pressure_pa      var=1.456e+05  cur=9.889e+04 ·no edges
    prev_residual_temp_k           var=3.366e+04  cur=875.9 ·no edges
    residual_fraction              var=0.000551   cur=0.04179 ·no edges
    rpm                            var=0          cur=2000 ⚠low-var ·no edges
    scavenging_penalty             var=0.000195   cur=0.1819 ·no edges
    spark_angle_deg                var=2.493      cur=333.8 ·no edges
    trapped_mass_kg                var=0          cur=0.0005691 ·no edges
    turbulence_intensity           var=0.000279   cur=2.388 ·no edges
    volumetric_efficiency          var=0.001751   cur=0.9276 ·no edges
    volumetric_efficiency_real     var=0.001751   cur=0.9276 ·no edges

  ⚠ no-variation (variance-gated out of DIFC): ['air_fuel_ratio', 'engine_speed_rpm', 'ncm_residual_mass_kg', 'rpm']
  · no valid edges yet: ['air_fuel_ratio', 'blowdown_intensity', 'breathing_efficiency', 'burn_acceleration', 'burn_duration', 'ca10']


────────────────────────────────────────────

  [observer] ca50 std trend: 2.8→3.5° (+0.35°/interval, 3 consecutive increases)
  You ▸ help all
  [ROUTER] CHAT

  ─ ALL COMMANDS (107) · grouped by category ─

  COMMANDS (engine + analysis)
    engine chalao / rokko              start / stop the engine
    <var> analyse karo                 run DIFC on a target (replaces focus)
    <var> aur <var> analyse karo       analyse multiple targets
    <var> add karo / <var> hatao       extend / shrink active targets
    rerun difc                         re-run DIFC on current buffer
    difc status                        lifecycle, running, snapshot, edges
    rpm 2500                           change engine regime

  SYSTEM & RUNTIME (status · health · integrity)
    status / health / diagnostics      full runtime state: organism, engine, graph, census, economy
    organism status / organism health  blocked-by-code vs blocked-by-evidence + open threads
    event log / runtime governance     background threads, recent events, errors
    boot check / integrity             runtime integrity: env, sys.path, orch lifecycle, signature drift
    show errors / log                  recent errors + DIFC trace log for this session
    why difc failed                    why the last DIFC run produced nothing (lifecycle + reason + source)
    snapshots                          snapshot archive + last DIFC result lineage
    repair report                      data-repair audit for the current session
    staleness / difc age               engine-vs-graph age (how stale the causal model is)

  EXPLORE (read the causal graph)
    <var> ke drivers                   causal edge list for a target
    <var> ka path                      multi-hop causal chain
    ascii                              causal map (cognition tree)
    show valid|unknown|rejected        edge explorer — directed edge list per bucket
    show accepted|provisional|stored|census DIFC census — raw→valid→accepted/unknown→stored NUMBERS
    signal inventory                   all buffer vars + variance + edge health
    buffer columns                     list buffer columns + excluded + derived transforms
    buffer add <name> = <expr>         add a derived column (arithmetic over existing cols)
    buffer remove <col>                drop a column from the buffer (now + future cycles); restore with 'buffer restore'
    <var> spread / spread distribution a variable's value distribution: mean/std/CV/quartiles + histogram
    system inventory                   connection audit: vars · DIFC connectivity · actuators · targets
    show forbidden <var>               direction-gate audit: kaun drivers DAG-rule se blocked
    <var> explain scope                scope-filter vs direction-filter: kaun vars discovery me the/nahi
    graph evolution                    how edges changed across runs
    causal drift                       how much the causal graph shifted between runs (needs ≥2 runs)
    upstream <var>                     upstream causal chain feeding a variable
    physics dag / dag                  physics dependency DAG (engine-internal variable graph)
    edge history <driver> <target>     how one edge's strength evolved across runs
    timeline                           investigation history

  EXPERIMENTS (perturb + measure)
    full experiment karo               autonomous chain→node→perturb→rank
    <var> ka full experiment karo      experiment on a specific target
    experiments                        experiment state + history
    baseline capture <label> / list baselines snapshot a reference state · list captured baselines
    baseline delta <label>             compare current state vs a captured baseline
    replay                             replay the last experiment's perturbation sequence
    challenge this / disprove          falsification: try to break the last claim
    ambiguous                          which decisions are epistemically ambiguous + why
    prediction accuracy                predicted vs actual error

  CONTROL (act on the engine)
    latent mode <ADVISORY|AUTO|CUSTOM> latent execution mode: Advisory(human)·Auto(organism consumes)·Custom(per-gate)
    latent profile <CONSERVATIVE|BALANCED|AGGRESSIVE|ADAPTIVE> AUTO threshold profile (Adaptive = Forecast-driven dynamic)
    latent config / latent awareness   show latent mode config / full gate decision objects (value·source·authority·suggested)
    latent forecast                    Forecast: simulate Gate-2 threshold impact + writes recommendation (reaching-G3, est. accepted)
    latent awareness                   Awareness: regime assessment + endorse/veto threshold recommendation (risk-gated)
    latent planning                    Planning: keep-vs-apply options from recommendation+awareness (impact·confidence·risk)
    latent decide                      run the full Forecast→Awareness→Planning loop and show recommended action
    gate G2 0.45 / gate G2 dynamic / gate G2 authority forecast per-gate (CUSTOM): set value · Fixed/Dynamic · authority (User/Awareness/Forecast/Hybrid)
    threshold / thresholds             view all 4 gate thresholds with owner·value·source·authority
    node <cfg>/<node> range <lo> <hi>  CONTINUOUS node variation → node becomes a buffer variable → node→var sensitivity; 'range off' clears
    analyse <var> <lo> <hi> <policy>   RUNTIME VARIABLE MANAGER: activate a runtime variable (e.g. 'analyse ethanol_fraction 0 0.3 grid'). policy = static/random/grid/lhs; multi: 'analyse ethanol_fraction 0 0.3 grid + spark_angle_deg 320 345 lhs'
    runtime variables / runtime status list currently active runtime variables: node, range, policy, current value
    runtime stop <var>                 deactivate a runtime variable (clears its range, node returns to fixed)
    rpm range <lo> <hi> / afr range <lo> <hi> CONTINUOUS variation (driver-like, persists) → ongoing variance; 'range off' clears
    rpm sweep <lo> <hi> / afr sweep <lo> <hi> ONE-SHOT sweep across a range → buffer variance → driver sensitivity
    spark +2 karo / spark -2 karo      perturb spark, see shift
    <var> hide karo                    latent variable test (hold one out)
    stable karo / <var> stabilize karo auto-stabilization (multi-lever: coordinates ALL measured actuators, signed)
    sensitivity convergence / kitna stable per-driver→target reliability: n_stable, converged/stable/noisy
    promote <driver> / forecast compare approve a driver for the causal forecast (Bridge B); compare A vs B
    prediction skill                   skill vs naive baselines (persistence/mean) over verified predictions — justification
    promotion policy auto|manual       auto-promote validated edges, or require manual approval
    bridge b                           Bridge-B forecast evidence (shadow causal vs live)
    correction factor                  current sensitivity correction factor + drivers
    sweep karo                         causal sensitivity scan
    list controllable                  show all controllable hub nodes
    <var> ke liye node                 show that variable's control nodes + range (then pick one)
    <config>.<node> ka full experiment karo single-node experiment on a user-chosen node
    node permanent karo                commit last-experiment's best node permanently (measure→commit)
    experiment result                  re-show last full-experiment's per-node result table
    set node <config>/<node> <value>   permanently change a node + auto re-analyse
    set window <N>                     set experiment cycle window before 'full experiment karo'
    comparison dikhao                  before/after causal table for last node change
    restore node <config>/<node> / restore all roll back user node changes
    unlock <var> / release             unfreeze an intervention-frozen variable
    bridge profile                     node-variable profile: which nodes act as causal bridges
    <var> ke liye kya karu             recommended node to move a variable

  TRUST & ECONOMY
    trust evidence                     trust store breakdown per edge
    evidence breakdown / trust stores  per-factor trust detail (freshness · evidence · consistency)
    roi                                return-on-investigation: knowledge gained vs compute spent
    attention state                    observer attention — where residual evidence points
    awareness / residual state         observer awareness: free-run residual status (report-only)
    ceh audit / canonical audit        CEH self-audit: signal integrity check
    efficiency / knowledge economy     knowledge per CPU-second
    ceh / dashboard                    all epistemic signals raw & separate (E/T/Ev/L/σe/rU/Ug/P) — no composite
    freshness                          engine-vs-graph age

  NARRATION (ask the organism)
    ab kya karu                        recommended next step (state-aware)
    strategy                           observer's ranked recommendation (analyse/wait/experiment) + reason
    decision trace                     history of recommendations made (read-only)
    kahan dekhu / where to look        where to investigate next (leverage-ranked)
    how wrong / prediction error       how far off the last prediction was
    goal / focus                       current optimization target the organism is tracking
    <var> kya hai                      self-explanation of a variable
    uncertainty kyun                   why a result is uncertain
    measured / AFR ka CA50 pe effect   measured-vs-unmeasured check
    ebis kya hai                       architecture explanation
    narrator / normal mode             turn the conversational narrator on / off
    anchor trace on / off              show/hide the interrogation anchor-trace reasoning
    prove this / iska proof            evidence trail behind the last claim (yeh kaise aaya)
    summary / session summary          what happened this session: stable findings
    <var> ka relation                  how two variables relate (edge + direction + strength)

  GLOSSARY
    manual / how to / kaise karu       step-by-step usage guide with examples (rpm/afr/node change, experiment, stabilise)
    glossary                           plain-language: knowledge/efficiency/trust/signal/R²/latent

    (one category: 'help <name>'  ·  back to index: 'help')


────────────────────────────────────────────
  You ▸ buffer remove ca10
  [ROUTER] CHAT

  ✓ Column 'ca10' buffer se dropped (now + future cycles). Reversible: 'buffer restore ca10'.
  ⚠ Agar 'ca10' confounder/mediator tha to baaki edges me omitted-variable bias aa sakta.


────────────────────────────────────────────
  You ▸ buffer remove ca90
  [ROUTER] CHAT

  ✓ Column 'ca90' buffer se dropped (now + future cycles). Reversible: 'buffer restore ca90'.
  ⚠ Agar 'ca90' confounder/mediator tha to baaki edges me omitted-variable bias aa sakta.


────────────────────────────────────────────
  You ▸ analyse ca50
  [ROUTER] COMMAND
  Active targets: ['ca50']  (focus = ca50)

  [COMMAND] → DIFC requested (200 cycles in buffer)
Analysis running in background...


────────────────────────────────────────────
  DIFC · Initializing... · 5%
  [STAGE:Buffer] total_buffer=200  difc_cursor=0  new_since_cursor=200
  [STAGE:WindowSelection] rows=200  cols_before_clean=45  cols_after_clean=44
  DIFC · Data ready: 200 cycles × 40 vars · 12%
  DIFC · EXPERIMENT mode — causal discovery (200 cycles)... · 20%
  DIFC: 200 cycles → 10 vars → 113 features → solver 8.37s → 90 edges  [⚠collinear]
  [observer] ⏳ DIFC[ca50] slow run (>25s) — abort NAHI, +35s grace (soft timeout)
  DIFC · Physics validation... · 55%  · ~50s left
  [observer] ca50: scope = 10 vars (7 DAG-ancestors + 3 data-prescreened) — 35 vars is run me discovery me nahi the (har run top-correlated rotate hote)
  [observer] ca50: raw=39 valid=23 accepted=21 unknown=0 rejected=18 (stored=21: 21 confirmed + 0 provisional)
  [observer] ca50: rejection reasons → 11× insufficient_evidence · 7× forbidden_direction
  DIFC · Sensitivity calibration... · 65%  · ~54s left
  DIFC · SBL learning update... · 72%  · ~47s left
  DIFC · ACIS reasoning... · 80%  · ~30s left
  DIFC · Querying per-target decisions... · 87%  · ~18s left
  DIFC · Multi-objective tradeoff... · 93%  · ~9s left
  DIFC · Building causal graph... · 97%  · ~3s left
  DIFC · ✅ Done — 21 edges · 100%

  [observer] DIFC complete — causal structure updated
  [observer] Economy: 5.3403 knowledge-value · discovery 30.218 CPU-sec · maintenance 222.002 CPU-sec
  [observer] epistemic: U_total=0.24 · latent=0.95 (max; mean 0.79)  (type 'ceh' for breakdown)
  You ▸ ascii
  [ROUTER] CHAT

  EBIS COGNITION MAP
  Branch: evidence verification
  Root:   ca50 investigation
  graph source: COMMITTED v1
  reliability: VERIFYING (coverage 0/21 hops, background verify chal raha)
  trust = confidence × maturity × freshness (fused) — 'UNRELIABLE' ≠ low confidence; it means low fused trust (raise maturity with experiments)
  background verifiers: 1 active
  ──────────────────────────────────────────────────────────────
  ca50
  ├── ← ✓ burn acceleration  +0.580  [conf 0.60 · unc 0.0031]  lag same-cyc?  trust:WEAK  [~latent 0.75]
  ├── ← ✓ burn duration  +0.696  [conf 0.59 · unc 0.0031]  lag same-cyc?  trust:WEAK  [~latent 0.77]
  ├── ← ✓ ignition delay index  +0.798  [conf 0.57 · unc 0.0031]  lag same-cyc?  trust:MODERATE  [~latent 0.76]
  ├── +3 more drivers — type expand
  │
  causes:
  └── → ✓ ignition delay index  +0.938  [conf 0.61 · unc 0.03]  lag same-cyc?  [~latent 0.74]
  ──────────────────────────────────────────────────────────────
  [4 of 7 DIRECT edges shown (type expand) · CEH E = full scope graph, not just focus-edges]
  +14 edges among other scope vars (not ca50-direct) — type `show details` for full graph
  legend: [conf · unc U]  U=edge uncertainty (1-r², unexplained variance) or 4-component when present (~U = 1-conf proxy)  latent L = hidden-factor risk


────────────────────────────────────────────
  DIFC: 200 cycles → 10 vars → 113 features → solver 38.11s → 90 edges  [⚠collinear]
  You ▸ ca50 experiment karo
  [ROUTER] COMMAND

Causal paths for ca50 (90 found):
  burn_acceleration → burn_duration  [+0.290]
  burn_acceleration → ca50  [+0.580]
  burn_acceleration → ignition_delay_index  [+0.544]
  burn_acceleration → knock_intensity  [-4.078]
  burn_acceleration → mean_flame_speed  [-5.465]

  ──────────────────────────────────────────────────────────
  ⚠ Signal weak (ca50: 0.15) — more cycles recommended


────────────────────────────────────────────
  [observer] reliability verified: ca50 — coverage 21/21 hops (72.4s background)
  You ▸ full experiment ca50
  [ROUTER] COMMAND

  → AUTONOMOUS EXPERIMENT: target=ca50
    chain detection → node selection → perturbation → measure → rank
    node→buffer column: ON (node→var analysis)
  → exploring ca50 by perturbing its driver ignition  (path: ca50 ← ignition)
  [observer] AEX: var 1/1 · ca50 → mapping calibration nodes
  [observer] AEX: var 1/1 · node 1/12  phasing.mbt_a → sweeping (continuous excitation)...

  [observer] New engine cycles arrived — causal model now stale (re-run DIFC to refresh)
  [observer] AEX: var 1/1 · node 2/12  phasing.mbt_b → sweeping (continuous excitation)...
  [observer] AEX: var 1/1 · node 3/12  phasing.kp_nom → sweeping (continuous excitation)...
  [observer] AEX: var 1/1 · node 4/12  phasing.ki_nom → sweeping (continuous excitation)...
  [observer] AEX: var 1/1 · node 5/12  phasing.kd_nom → sweeping (continuous excitation)...

  [observer] combustion: unstable  ca50_std=3.73°  knock=28.437
  [observer] AEX: var 1/1 · node 6/12  closed.turbulence_gain → sweeping (continuous excitation)...

  [observer] combustion: moderate  ca50_std=2.58°  knock=28.177

  [observer] combustion: unstable  ca50_std=7.60°  knock=28.007
  [observer] AEX: var 1/1 · node 7/12  closed.turbulence_exp → sweeping (continuous excitation)...

  [observer] combustion: moderate  ca50_std=7.60°  knock=29.956
  [observer] AEX: var 1/1 · node 8/12  closed.wiebe_alpha → sweeping (continuous excitation)...

  [observer] combustion: unstable  ca50_std=1.98°  knock=26.726
  [observer] AEX: var 1/1 · node 9/12  closed.wiebe_beta → sweeping (continuous excitation)...

  [observer] combustion: moderate  ca50_std=4.66°  knock=28.900
  [observer] AEX: var 1/1 · node 10/12  closed.char_len_scale → sweeping (continuous excitation)...
  [observer] AEX: var 1/1 · node 11/12  closed.laminar_temp_exp → sweeping (continuous excitation)...
  [observer] AEX: var 1/1 · node 12/12  kernel.compression_tke_weight → sweeping (continuous excitation)...

  [observer] combustion: unstable  ca50_std=7.46°  knock=28.125
  ✓ experiment complete in 287.7s
    variables analyzed: 1  experiments: 12  cycles: 960

    ca50:  12 nodes  (12 successful)
      phasing.mbt_a                   14.0000 → 15.1200  ca50Δ=+0.1159 (pred n/a (node→var chain))
      phasing.mbt_b                   0.0075 → 0.0081  ca50Δ=+0.3229 (pred n/a (node→var chain))
      phasing.kp_nom                  0.3000 → 0.3240  ca50Δ=+0.3026 (pred n/a (node→var chain))
      phasing.ki_nom                  0.0800 → 0.0864  ca50Δ=+0.5418 (pred n/a (node→var chain))
  → 12 node sensitivities written to experiment memory (stabilizer can now use them)
  [exp-audit]            phasing.mbt_a  evidence→29
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000

  [observer] combustion: moderate  ca50_std=1.27°  knock=28.686
  DIFC: 80 cycles → 10 vars → 113 features → solver 32.15s → 89 edges  [⚠collinear]
  [exp-audit]            phasing.mbt_b  evidence→30
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 206 features → solver 48.84s → 87 edges  [⚠collinear]
  [exp-audit]           phasing.kp_nom  evidence→31
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 113 features → solver 27.1s → 86 edges  [⚠collinear]
  [exp-audit]           phasing.ki_nom  evidence→32
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 206 features → solver 42.74s → 89 edges  [⚠collinear]
  [exp-audit]           phasing.kd_nom  evidence→33
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 206 features → solver 45.09s → 83 edges  [⚠collinear]
  [exp-audit]   closed.turbulence_gain  evidence→34
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 206 features → solver 49.27s → 90 edges  [⚠collinear]

  [observer] combustion: unstable  ca50_std=5.25°  knock=28.637
  [exp-audit]    closed.turbulence_exp  evidence→35
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000

  [observer] combustion: moderate  ca50_std=3.36°  knock=28.347
  DIFC: 80 cycles → 10 vars → 113 features → solver 27.17s → 82 edges  [⚠collinear]
  [exp-audit]       closed.wiebe_alpha  evidence→36
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 204 features → solver 40.6s → 90 edges  [⚠collinear]
  [exp-audit]        closed.wiebe_beta  evidence→37
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 206 features → solver 49.19s → 87 edges  [⚠collinear]
  [exp-audit]    closed.char_len_scale  evidence→38
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 206 features → solver 44.18s → 89 edges  [⚠collinear]
  [exp-audit]  closed.laminar_temp_exp  evidence→39
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 113 features → solver 27.19s → 89 edges  [⚠collinear]
  [exp-audit] kernel.compression_tke_w  evidence→40
              observation PASS · evidence PASS · learning PASS · prediction-calibration SKIPPED · decision-impact PASS
              └─ learning: evidence +1 · intervention verdict · sensitivity ✓  |  calibration SKIPPED (prediction NaN (node→target causal bridge not learned yet))  |  trust +0.000
  DIFC: 80 cycles → 10 vars → 112 features → solver 29.46s → 88 edges  [⚠collinear]
  → 12 experiments recorded to history (trust evidence will now reflect them)
  intervention verdicts [ca50]: 2 strong · 12 weak · 7 insufficient
    STRONG  ('burn_duration', 'burn_acceleration')
    STRONG  ('knock_intensity', 'burn_acceleration')
    weak    ('burn_acceleration', 'ca50')  WEAK
    weak    ('ca50', 'ignition_delay_index')  WEAK
    weak    ('ignition_delay_index', 'burn_duration')  WEAK
    weak    ('ignition_delay_index', 'ca50')  WEAK
    weak    ('ignition_delay_index', 'spark_angle_deg')  WEAK
    weak    ('ignition_delay_index', 'turbulence_intensity')  WEAK
    weak    ('knock_intensity', 'burn_duration')  WEAK
    weak    ('knock_intensity', 'volumetric_efficiency')  WEAK
    weak    ('residual_fraction', 'ca50')  WEAK
    weak    ('turbulence_intensity', 'burn_duration')  WEAK
    weak    ('turbulence_intensity', 'ca50')  WEAK
    weak    ('turbulence_intensity', 'spark_angle_deg')  WEAK
  → 12 edge lifecycle confidences updated from experiment agreement


────────────────────────────────────────────
  You ▸ analyse spark_angle_deg 320 345 lhs
  [ROUTER] COMMAND
  Active targets: ['spark_angle_deg']  (focus = spark_angle_deg)

  [COMMAND] → DIFC requested (200 cycles in buffer)
  DIFC · Initializing... · 5%
Analysis running in background...


────────────────────────────────────────────
  [STAGE:Buffer] total_buffer=200  difc_cursor=200  new_since_cursor=0
  [STAGE:Buffer] cursor RESET to 0 (new_since_cursor was <30) → new_buf=200
  [STAGE:WindowSelection] rows=200  cols_before_clean=45  cols_after_clean=44
  DIFC · Data ready: 200 cycles × 40 vars · 12%
  DIFC · EXPERIMENT mode — causal discovery (200 cycles)... · 20%
  DIFC: 200 cycles → 9 vars → 98 features → solver 4.17s → 72 edges  [⚠collinear]
  DIFC · Physics validation... · 55%  · ~13s left
  [observer] spark_angle_deg: scope = 9 vars (1 DAG-ancestors + 8 data-prescreened) — 36 vars is run me discovery me nahi the (har run top-correlated rotate hote)
  [observer] spark_angle_deg: raw=21 valid=16 accepted=14 unknown=0 rejected=7 (stored=14: 14 confirmed + 0 provisional)
  [observer] spark_angle_deg: rejection reasons → 4× forbidden_direction · 3× insufficient_evidence
  DIFC · Sensitivity calibration... · 65%  · ~23s left
  DIFC · SBL learning update... · 72%  · ~24s left
  DIFC · ACIS reasoning... · 80%  · ~15s left
  DIFC · Querying per-target decisions... · 87%  · ~9s left
  DIFC · Multi-objective tradeoff... · 93%  · ~4s left
  DIFC · Building causal graph... · 97%  · ~1s left
  DIFC · ✅ Done — 14 edges · 100%

  [observer] DIFC complete — causal structure updated
  [observer] Economy: 9.6026 knowledge-value · discovery 6.519 CPU-sec · maintenance 676.386 CPU-sec
  [observer] epistemic: U_total=0.37 · latent=0.82 (max; mean 0.78)  (type 'ceh' for breakdown)
  You ▸ ascii
  [ROUTER] CHAT

  EBIS COGNITION MAP
  Branch: evidence verification
  Root:   spark investigation
  graph source: COMMITTED v1
  reliability: VERIFYING (coverage 0/14 hops, background verify chal raha)
  trust = confidence × maturity × freshness (fused) — 'UNRELIABLE' ≠ low confidence; it means low fused trust (raise maturity with experiments)
  background verifiers: 1 active
  ──────────────────────────────────────────────────────────────
  spark
  ├── ← ✓ turbulence intensity  +0.884  [conf 0.59 · unc 0.06]  lag same-cyc?  ⚠collinear  trust:UNRELIABLE  [~latent 0.78]
  ├── ← ✓ ignition delay index  +0.758  [conf 0.58 · unc 0.06]  lag same-cyc?  trust:UNRELIABLE  [~latent 0.74]
  │
  causes:
  ├── → ✓ turbulence intensity  +0.856  [conf 0.59 · unc 0.02]  lag same-cyc?  [~latent 0.75]
  └── → ✓ ca50  +0.535  [conf 0.49 · unc 0.00]  lag same-cyc?  [~latent 0.87]
  ──────────────────────────────────────────────────────────────
  Session: 1 exchanges
  · ca50 experiment karo
  [4 direct edges shown — all evidence-backed · CEH E = full scope graph]
  +23 edges among other scope vars (not spark-direct) — type `show details` for full graph
  legend: [conf · unc U]  U=edge uncertainty (1-r², unexplained variance) or 4-component when present (~U = 1-conf proxy)  latent L = hidden-factor risk


────────────────────────────────────────────
  DIFC: 200 cycles → 9 vars → 98 features → solver 19.52s → 72 edges  [⚠collinear]
  [observer] reliability verified: spark_angle_deg — coverage 14/14 hops (33.6s background)

  [observer] New engine cycles arrived — causal model now stale (re-run DIFC to refresh)
  You ▸ ca50 stable karo
  [ROUTER] COMMAND

→ Auto-stabilization: ca50



  [observer] System: causal model stale — type 'rerun difc' to refresh

────────────────────────────────────────────
[Baseline: ca50=-1.1° spark=28.0°]


  Objective [ca50]: stabilize: minimize CA50 oscillation, hold near MBT-proxy setpoint
    source: autonomous default — no explicit objective given · setpoint=+0.0° (set ORCH.target_objectives['ca50'] to override)


  STABILIZATION PROOF
  ────────────────────────────────────────────────────────
  Problem (Observer):  ca50=-1.1° osc=0.95
  Cause (Canonical):   ignition_delay_index → ca50  R²=0.9969  weight=0.7983  seen=1x  DORMANT
  Path (EIL):          ca50 ← ignition
  Decision (EIL):      actuate phasing/mbt_b  (sens=538.177, n_exp=2)
  Authority:           EIL+canonical+hub
  Rejected actuators:  mbt_a(sens=0.103), kp_nom(sens=12.607), ki_nom(sens=84.65) ...
  Trust:               0.0132  (UNRELIABLE) — freshness STALE — re-run DIFC
  Kyun yeh node:       'ignition' ek measured VARIABLE hai — directly turn nahi hota; mbt_b woh engine-knob hai jo experiments me ignition ko move karta dikha (sens=538.177), aur ignition → ca50.
[dash] DECISION {"target": "ca50", "objective": "stabilize: minimize CA50 oscillation, hold near MBT-proxy setpoint", "goal": 0.0, "current": -1.06, "error": -1.06, "tolerance": null, "chosen_node": "phasing/mbt_b", "cause": "ignition_delay_index", "sensitivity": 538.177, "n_exp": 2, "reason": "highest measured |sensitivity| among eligible actuators", "trust": 0.0132, "trust_band": "UNRELIABLE", "trust_reason": "freshness STALE \u2014 re-run DIFC", "rejected_drivers": [{"var": "burn_duration", "weight": 0.696, "confidence": 0.8857, "reason": "lower weight\u00d7confidence than ignition_delay_index (0.6165 vs 0.7194)"}, {"var": "turbulence_intensity", "weight": 0.6634, "confidence": 0.824, "reason": "lower weight\u00d7confidence than ignition_delay_index (0.5467 vs 0.7194)"}, {"var": "burn_acceleration", "weight": 0.58, "confidence": 0.8524, "reason": "lower weight\u00d7confidence than ignition_delay_index (0.4944 vs 0.7194)"}, {"var": "spark_angle_deg", "weight": 0.5353, "confidence": 0.741, "reason": "lower weight\u00d7confidence than ignition_delay_index (0.3967 vs 0.7194)"}], "rejected_actuators": [{"actuator": "phasing/mbt_a", "sensitivity": 0.103, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (0.103 vs 538.177)"}, {"actuator": "phasing/kp_nom", "sensitivity": 12.607, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (12.607 vs 538.177)"}, {"actuator": "phasing/ki_nom", "sensitivity": 84.65, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (84.650 vs 538.177)"}, {"actuator": "phasing/kd_nom", "sensitivity": 168.833, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (168.833 vs 538.177)"}, {"actuator": "closed/turbulence_gain", "sensitivity": 0.054, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (0.054 vs 538.177)"}, {"actuator": "closed/turbulence_exp", "sensitivity": 4.05, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (4.050 vs 538.177)"}, {"actuator": "closed/wiebe_alpha", "sensitivity": 6.027, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (6.027 vs 538.177)"}, {"actuator": "closed/wiebe_beta", "sensitivity": 22.313, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (22.313 vs 538.177)"}, {"actuator": "closed/char_len_scale", "sensitivity": 7.329, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (7.329 vs 538.177)"}, {"actuator": "closed/laminar_temp_exp", "sensitivity": 2.672, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (2.672 vs 538.177)"}, {"actuator": "kernel/compression_tke_weight", "sensitivity": 30.04, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (30.040 vs 538.177)"}]}
  Control levers:      spark(-143.88, n=10)  [single-lever · measured · signed]
  ────────────────────────────────────────────────────────

  [DIFC evidence — not directly routable]:
    volumetric_efficiency: gain=+76.842  n=4
      upstream actuators: intake/initial_throttle, intake/valve_diameter_ratio, rpm
      coverage gap: intake/initial_throttle, intake/valve_diameter_ratio, rpm — potential upstream path exists but causal visibility incomplete (no DIFC evidence)
  [upstream experiment] volumetric_efficiency has DIFC evidence (gain=+76.842, n=4) but no direct actuator.
    Hypothesis: rpm 2000.00→2100.00 (bounded ±5%) — does downstream chain activate?
    [unverified] downstream volumetric_efficiency did not move (-0.0338). Rolling back rpm to 2000.00.

  mbt_b 0.0075→0.0078 (EIL hub actuator) → expecting ca50 shift...

  lever [spark -0.01] → expecting ca50 shift (Σ-allocated · signed · reliability-weighted) · trust(target)=0.08
      spark    w=1.00 sens=-143.88 n=10(stable 0) · noisy
    1 measured lever — widens as more actuator→ca50 sensitivities get measured ('full experiment')
  ca50=-0.4°  error=-0.4°
  Mean converged (ca50=-0.4°) but oscillating (std=3.4°)
  Oscillation source analysis (DIFC causal ranking):
    ignition        weight=0.798  conf=0.90  ctrl=—  ⚠latent
    burn            weight=0.696  conf=0.89  ctrl=✓  ⚠latent
    turbulence      weight=0.663  conf=0.82  ctrl=✓  ⚠latent
    burn            weight=0.580  conf=0.85  ctrl=✓  ⚠latent
  → Controllable oscillation source: burn (weight=0.696, conf=0.89)
    Actuator available via hub — multi-path stabilization can target it

[dash] VERIFY {"result": "FAIL", "ca50": -1.29, "std": 3.32, "error": -1.29, "setpoint": 0.0, "reason": "std 3.3(<3?) / |err| 1.3(<2?)", "next": "re-nudge / try stronger actuator"}

  [verify] Stability NOT held: ca50=-1.3° std=3.3° knock=28.9
  [observer] Stabilize: not held — closed-loop re-correction (max 3)...
  [observer] Stabilize: re-nudge 1/3 mbt_b 0.0078→0.0084
  [observer] Stabilize: re-nudge 2/3 mbt_b 0.0084→0.0091
  [observer] Stabilize: re-nudge 3/3 mbt_b 0.0091→0.0098
  [observer] Verify: Oscillation persists — spark-only control insufficient
  [observer] Self-heal: graph stale + control failing → auto re-running DIFC (no user action needed)...
  DIFC · Initializing... · 5%
  [observer] Verify: Knock still elevated — retard spark or check AFR
  [observer] Verify: Knock corrective: spark retard +2.9° (BTDC 25.1°)
  [STAGE:Buffer] total_buffer=200  difc_cursor=200  new_since_cursor=0
  [STAGE:Buffer] cursor RESET to 0 (new_since_cursor was <30) → new_buf=200
  [STAGE:WindowSelection] rows=200  cols_before_clean=45  cols_after_clean=44
  DIFC · Data ready: 200 cycles × 42 vars · 12%
  DIFC · EXPERIMENT mode — causal discovery (200 cycles)... · 20%
  DIFC: 200 cycles → 10 vars → 114 features → solver 9.7s → 72 edges  [⚠collinear]
  [observer] ⏳ DIFC[ca50] slow run (>25s) — abort NAHI, +35s grace (soft timeout)
  DIFC · Physics validation... · 55%  · ~22s left
  [observer] ca50: scope = 10 vars (8 DAG-ancestors + 2 data-prescreened) — 35 vars is run me discovery me nahi the (har run top-correlated rotate hote)
  [observer] ca50: raw=27 valid=17 accepted=15 unknown=0 rejected=12 (stored=15: 15 confirmed + 0 provisional)
  [observer] ca50: rejection reasons → 7× insufficient_evidence · 5× forbidden_direction
  DIFC · Sensitivity calibration... · 65%  · ~32s left
  DIFC · SBL learning update... · 72%  · ~29s left
  DIFC · ACIS reasoning... · 80%  · ~18s left
  DIFC · Querying per-target decisions... · 87%  · ~11s left
  DIFC · Multi-objective tradeoff... · 93%  · ~5s left
  DIFC · Building causal graph... · 97%  · ~2s left
  DIFC · ✅ Done — 15 edges · 100%

  [observer] DIFC complete — causal structure updated
  [observer] Economy: 13.2133 knowledge-value · discovery 19.045 CPU-sec · maintenance 640.289 CPU-sec
  [observer] epistemic: U_total=0.20 · latent=0.90 (max; mean 0.76)  (type 'ceh' for breakdown)
  DIFC: 200 cycles → 8 vars → 152 features → solver 53.01s → 56 edges  [⚠collinear]
  [observer] reliability verified: ca50 — coverage 15/15 hops (69.2s background)

  [observer] New engine cycles arrived — causal model now stale (re-run DIFC to refresh)
  You ▸ re-nudge
  [ROUTER] CHAT

  ⚠ variable: ca50  DIFC scope: 15 valid edges  signal=0.18  difc=STALE

  ⚠ edges are 230 cycles old — re-run DIFC for current state

  ✓ ORCH: n=40  phase=BOOTSTRAP  0 contradictions

  ⚠ DIFC: 2 drivers  1 effects  signal=0.18

  [observer] combustion: stable  ca50_std=2.03°  knock=26.683

  drivers  ·  40 exp
  ca50  signal=0.18 [WEAK]  phase=BOOTSTRAP  n=40 exp  [STALE]

  ↓ caused by (evidence weight):
     ignition  █████░░░  +0.975  PRIMARY  ⚠  ~lat 0.73  unc 0.01  trust:UNRELIABLE  seen=2x ^
       ignition increases ca50 (limited evidence, seen 2x)
     burn    █████░░░  +0.796  PRIMARY  ⚠  ~lat 0.77  unc 0.01  trust:UNRELIABLE  seen=1x
       burn increases ca50 (limited evidence, seen 2x)

  → ca50 propagates to:
     ignition  █████░░░  +0.781  WEAK

  Ignition increases ca50 (limited evidence, single-regime).
  This was observed consistently across the tested regime (heuristic).
  Burn increases ca50 (limited evidence, single-regime).
  Caution: the burn relationship shows signs of a hidden variable — an unmeasured factor may be confounding this signal.

  n=40 — next step:
  · ca50 hide karo — isolate its effect
  · ascii — causal map
  ~ knock elevated (26.89) — check causal drivers
  ~ trust(ca50)=0.50 · weakest: regime_diversity=0.33 · recover: change rpm (e.g. 'rpm 2500')
  ~ ca50: edges stale (240 cycles old) — re-run DIFC

  EBIS COGNITION MAP
  Branch: drivers investigation
  Trail:  evidence verification → drivers investigation
  Root:   ca50 investigation
  graph source: COMMITTED v2
  reliability: VERIFIED (coverage 15/15 hops)
  trust = confidence × maturity × freshness (fused) — 'UNRELIABLE' ≠ low confidence; it means low fused trust (raise maturity with experiments)
  ──────────────────────────────────────────────────────────────
  ca50
  ├── ← ✓ ignition delay index  +0.787  [conf 0.61 · unc 0.03]  lag same-cyc?  ⚠collinear  trust:UNRELIABLE  [~latent 0.73]
  ├── ← ✓ burn acceleration  +0.707  [conf 0.61 · unc 0.03]  lag same-cyc?  ⚠collinear  trust:UNRELIABLE  [~latent 0.73]
  ├── ← ✓ burn duration  +0.796  [conf 0.59 · unc 0.0083]  lag same-cyc?  trust:UNRELIABLE  [~latent 0.77]
  ├── +1 more drivers — type expand
  │
  causes:
  ├── → ✓ ignition delay index  +0.781  [conf 0.62 · unc 0.03]  lag same-cyc?  [~latent 0.72]
  ├── → ✓ knock intensity  -0.907  [conf 0.59 · unc 0.01]  lag same-cyc?  [~latent 0.80]
  └── → ✓ burn acceleration  +0.775  [conf 0.57 · unc 0.08]  lag same-cyc?  [~latent 0.77]
  │
  ⚠ Tradeoff detected:
  ├── ↑ ignition delay index  +0.781  (improves)
  ├── ↑ burn acceleration  +0.775  (improves)
  └── ↓ knock intensity  -0.907  (worsens)
  ──────────────────────────────────────────────────────────────
  Session: 1 exchanges
  · ca50 experiment karo
  [6 of 7 DIRECT edges shown (type expand) · CEH E = full scope graph, not just focus-edges]
  +14 edges among other scope vars (not ca50-direct) — type `show details` for full graph
  legend: [conf · unc U]  U=edge uncertainty (1-r², unexplained variance) or 4-component when present (~U = 1-conf proxy)  latent L = hidden-factor risk


  [observer] System: causal model stale — type 'rerun difc' to refresh

────────────────────────────────────────────
  You ▸ try stronger actuator
  [ROUTER] CHAT

  ⚠ variable: ca50  DIFC scope: 15 valid edges  signal=0.18  difc=STALE

  ⚠ edges are 295 cycles old — re-run DIFC for current state

  drivers  ·  40 exp
  Ignition increases ca50 (limited evidence, single-regime).
  This was observed consistently across the tested regime (heuristic).
  Burn increases ca50 (limited evidence, single-regime).
  Caution: the burn relationship shows signs of a hidden variable — an unmeasured factor may be confounding this signal.

  n=40 — next step:
  · ca50 hide karo — isolate its effect
  · ascii — causal map
  ~ knock elevated (26.82) — check causal drivers
  ~ ca50: edges stale (305 cycles old) — re-run DIFC


────────────────────────────────────────────
  You ▸ prediction
  [ROUTER] CHAT

  drivers  ·  40 exp

  ──────────────────────────────────────────────────────────
  →  Prediction: ca50
  horizon=15 cycles  ·  forecast value = time-series (statistical); ORCH/DIFC sections below are context, not in the number
  ──────────────────────────────────────────────────────────
  current value:  +2.861°
  recent trend:   stable  (-0.0090°/cycle)

  Projection (reversion × 15 cycles):
    ca50: +2.861 → +3.302  Δ=+0.441  [moderate confidence]
    basis: mean-reversion (oscillating; mean +3.34)

  Baselines to beat (justification):
    persistence (next=now):  +2.861
    mean (recent):           +3.344
  Skill: not yet measured — run 'verify' after 15 cycles to build the track record

  ORCH calibrated sensitivity (spark→ca50):
    -7.3875 units per unit change
    n=10 exp (n_stable=0) · source=measured · noisy  confidence=0.95
    example: if spark +1°, ca50 → -7.387

  DIFC structural drivers of ca50:  ⚠ from stale analysis
    ignition     → ca50                  effect=+0.975/unit
    burn         → ca50                  effect=+0.796/unit
    (1 self/sibling/duplicate edge(s) hidden — co-output, not physical driver)
    basis: DIFC causal graph — stale (re-run analysis for current estimate)  [explanatory — NOT in forecast value above]

  Predictions improve with each experiment (ORCH learning).
  Trend projection assumes no parameter changes.
  ~ causal model stale — new cycles arrived
  ~ knock elevated (25.35) — check causal drivers
  ~ ca50: edges stale (400 cycles old) — re-run DIFC


────────────────────────────────────────────

  [verify] prediction ca50: predicted=+3.3 actual=+2.4  error=0.9 (30% of σ) ✓
  DIFC · Initializing... · 5%
  [STAGE:Buffer] total_buffer=200  difc_cursor=200  new_since_cursor=0
  [STAGE:Buffer] cursor RESET to 0 (new_since_cursor was <30) → new_buf=200
  [STAGE:WindowSelection] rows=200  cols_before_clean=45  cols_after_clean=44
  DIFC · Data ready: 200 cycles × 40 vars · 12%
  DIFC · EXPERIMENT mode — causal discovery (200 cycles)... · 20%
  DIFC: 200 cycles → 10 vars → 114 features → solver 7.39s → 90 edges  [⚠collinear]
  [observer] ⏳ DIFC[ca50] slow run (>25s) — abort NAHI, +35s grace (soft timeout)
  DIFC · Physics validation... · 55%  · ~40s left
  [observer] ca50: scope = 10 vars (7 DAG-ancestors + 3 data-prescreened) — 35 vars is run me discovery me nahi the (har run top-correlated rotate hote)
  [observer] ca50: raw=35 valid=23 accepted=20 unknown=0 rejected=15 (stored=20: 20 confirmed + 0 provisional)
  [observer] ca50: rejection reasons → 9× insufficient_evidence · 6× forbidden_direction
  DIFC · Sensitivity calibration... · 65%  · ~48s left
  DIFC · SBL learning update... · 72%  · ~42s left
  DIFC · ACIS reasoning... · 80%  · ~27s left
  DIFC · Querying per-target decisions... · 87%  · ~16s left
  DIFC · Multi-objective tradeoff... · 93%  · ~8s left
  DIFC · Building causal graph... · 97%  · ~3s left
  DIFC · ✅ Done — 20 edges · 100%

  [observer] DIFC complete — causal structure updated
  [observer] Economy: 17.8082 knowledge-value · discovery 43.368 CPU-sec · maintenance 626.058 CPU-sec
  [observer] epistemic: U_total=0.24 · latent=0.87 (max; mean 0.77)  (type 'ceh' for breakdown)
  DIFC: 200 cycles → 9 vars → 99 features → solver 28.23s → 72 edges  [⚠collinear]
  [observer] reliability verified: ca50 — coverage 20/20 hops (47.2s background)
  You ▸ stable karo
  [ROUTER] COMMAND

→ Auto-stabilization: ca50



────────────────────────────────────────────
[Baseline: ca50=+2.0° spark=25.0°]


  Objective [ca50]: stabilize: minimize CA50 oscillation, hold near MBT-proxy setpoint
    source: autonomous default — no explicit objective given · setpoint=+0.0° (set ORCH.target_objectives['ca50'] to override)


  STABILIZATION PROOF
  ────────────────────────────────────────────────────────
  Problem (Observer):  ca50=+2.0° osc=0.95
  Cause (Canonical):   ignition_delay_index → ca50  R²=0.9952  weight=0.8008  seen=3x  DORMANT
  Path (EIL):          ca50 ← ignition
  Decision (EIL):      actuate phasing/mbt_b  (sens=538.177, n_exp=2)
  Authority:           EIL+canonical+hub
  Rejected actuators:  mbt_a(sens=0.103), kp_nom(sens=12.607), ki_nom(sens=84.65) ...
  Trust:               0.1673  (WEAK) — limited by reliability_factor=0.48
  Kyun yeh node:       'ignition' ek measured VARIABLE hai — directly turn nahi hota; mbt_b woh engine-knob hai jo experiments me ignition ko move karta dikha (sens=538.177), aur ignition → ca50.
[dash] DECISION {"target": "ca50", "objective": "stabilize: minimize CA50 oscillation, hold near MBT-proxy setpoint", "goal": 0.0, "current": 2.05, "error": 2.05, "tolerance": null, "chosen_node": "phasing/mbt_b", "cause": "ignition_delay_index", "sensitivity": 538.177, "n_exp": 2, "reason": "highest measured |sensitivity| among eligible actuators", "trust": 0.1673, "trust_band": "WEAK", "trust_reason": "limited by reliability_factor=0.48", "rejected_drivers": [{"var": "burn_duration", "weight": 0.7609, "confidence": 0.8666, "reason": "lower weight\u00d7confidence than ignition_delay_index (0.6594 vs 0.7355)"}, {"var": "burn_acceleration", "weight": 0.6167, "confidence": 0.8273, "reason": "lower weight\u00d7confidence than ignition_delay_index (0.5102 vs 0.7355)"}, {"var": "spark_angle_deg", "weight": 0.4927, "confidence": 0.744, "reason": "lower weight\u00d7confidence than ignition_delay_index (0.3666 vs 0.7355)"}], "rejected_actuators": [{"actuator": "phasing/mbt_a", "sensitivity": 0.103, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (0.103 vs 538.177)"}, {"actuator": "phasing/kp_nom", "sensitivity": 12.607, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (12.607 vs 538.177)"}, {"actuator": "phasing/ki_nom", "sensitivity": 84.65, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (84.650 vs 538.177)"}, {"actuator": "phasing/kd_nom", "sensitivity": 168.833, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (168.833 vs 538.177)"}, {"actuator": "closed/turbulence_gain", "sensitivity": 0.054, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (0.054 vs 538.177)"}, {"actuator": "closed/turbulence_exp", "sensitivity": 4.05, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (4.050 vs 538.177)"}, {"actuator": "closed/wiebe_alpha", "sensitivity": 6.027, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (6.027 vs 538.177)"}, {"actuator": "closed/wiebe_beta", "sensitivity": 22.313, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (22.313 vs 538.177)"}, {"actuator": "closed/char_len_scale", "sensitivity": 7.329, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (7.329 vs 538.177)"}, {"actuator": "closed/laminar_temp_exp", "sensitivity": 2.672, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (2.672 vs 538.177)"}, {"actuator": "kernel/compression_tke_weight", "sensitivity": 30.04, "n_experiments": 2, "reason": "lower measured sensitivity than phasing/mbt_b (30.040 vs 538.177)"}]}
  Control levers:      spark(-143.88, n=10)  [single-lever · measured · signed]
  ────────────────────────────────────────────────────────

  [DIFC evidence — not directly routable]:
    volumetric_efficiency: gain=+76.842  n=4
      upstream actuators: intake/initial_throttle, intake/valve_diameter_ratio, rpm
      coverage gap: intake/initial_throttle, intake/valve_diameter_ratio, rpm — potential upstream path exists but causal visibility incomplete (no DIFC evidence)
  [upstream experiment] volumetric_efficiency has DIFC evidence (gain=+76.842, n=4) but no direct actuator.
    Hypothesis: rpm 2000.00→1900.00 (bounded ±5%) — does downstream chain activate?
    [unverified] downstream volumetric_efficiency did not move (+0.0205). Rolling back rpm to 2000.00.

  mbt_b 0.0098→0.0094 (EIL hub actuator) → expecting ca50 shift...

  lever [spark +0.01] → expecting ca50 shift (Σ-allocated · signed · reliability-weighted) · trust(target)=0.71
      spark    w=1.00 sens=-143.88 n=10(stable 0) · noisy
    1 measured lever — widens as more actuator→ca50 sensitivities get measured ('full experiment')

  [observer] New engine cycles arrived — causal model now stale (re-run DIFC to refresh)
  ca50=+1.6°  error=+1.6°

  lever [spark +0.01] → expecting ca50 shift (Σ-allocated · signed · reliability-weighted) · trust(target)=0.70
      spark    w=1.00 sens=-143.88 n=10(stable 0) · noisy
    1 measured lever — widens as more actuator→ca50 sensitivities get measured ('full experiment')
  ca50=+1.4°  error=+1.4°

  lever [spark +0.01] → expecting ca50 shift (Σ-allocated · signed · reliability-weighted) · trust(target)=0.70
      spark    w=1.00 sens=-143.88 n=10(stable 0) · noisy
    1 measured lever — widens as more actuator→ca50 sensitivities get measured ('full experiment')
  ca50=+1.5°  error=+1.5°

  lever [spark +0.01] → expecting ca50 shift (Σ-allocated · signed · reliability-weighted) · trust(target)=0.70
      spark    w=1.00 sens=-143.88 n=10(stable 0) · noisy
    1 measured lever — widens as more actuator→ca50 sensitivities get measured ('full experiment')
  ca50=+2.8°  error=+2.8°

  lever [spark +0.02] → expecting ca50 shift (Σ-allocated · signed · reliability-weighted) · trust(target)=0.69
      spark    w=1.00 sens=-143.88 n=10(stable 0) · noisy
    1 measured lever — widens as more actuator→ca50 sensitivities get measured ('full experiment')
  ca50=+1.5°  error=+1.5°

  lever [spark +0.01] → expecting ca50 shift (Σ-allocated · signed · reliability-weighted) · trust(target)=0.69
      spark    w=1.00 sens=-143.88 n=10(stable 0) · noisy
    1 measured lever — widens as more actuator→ca50 sensitivities get measured ('full experiment')
  ca50=+0.9°  error=+0.9°
  Mean converged (ca50=+0.9°) but oscillating (std=3.2°)
  Oscillation source analysis (DIFC causal ranking):
    ignition        weight=0.801  conf=0.92  ctrl=—  ⚠latent
    burn            weight=0.761  conf=0.87  ctrl=✓  ⚠latent
    burn            weight=0.617  conf=0.83  ctrl=✓  ⚠latent
    spark           weight=0.493  conf=0.74  ctrl=✓  ⚠latent
  → Controllable oscillation source: burn (weight=0.761, conf=0.87)
    Actuator available via hub — multi-path stabilization can target it

[dash] VERIFY {"result": "PASS", "ca50": 1.8, "std": 2.74, "error": 1.8, "setpoint": 0.0, "reason": "held near setpoint, low oscillation", "next": "stable \u2014 hold"}

  [verify] Stability held: ca50=+1.8° std=2.7° knock=27.1
  [observer] Verify: Δ (steady-vs-steady): ca50_std 3.2→2.7 (+0.4), knock 26.5→27.1 (-0.6)
  [observer] Verify: MULTI-OBJECTIVE PASS: ca50 stabilized, knock not worsened
  [observer] Verify: Persistence proof (+30s): ca50=+1.0° std=2.2° knock=27.5 ✓ STABLE
  [observer] Verify: Evidence: prediction n=40, causal edges=45, confidence=moderate
  DIFC · Initializing... · 5%
  [STAGE:Buffer] total_buffer=200  difc_cursor=200  new_since_cursor=0
  [STAGE:Buffer] cursor RESET to 0 (new_since_cursor was <30) → new_buf=200
  [STAGE:WindowSelection] rows=200  cols_before_clean=45  cols_after_clean=44
  DIFC · Data ready: 200 cycles × 40 vars · 12%
  DIFC · EXPERIMENT mode — causal discovery (200 cycles)... · 20%
  DIFC: 200 cycles → 10 vars → 114 features → solver 9.29s → 90 edges  [⚠collinear]
  [observer] ⏳ DIFC[ca50] slow run (>25s) — abort NAHI, +35s grace (soft timeout)
  DIFC · Physics validation... · 55%  · ~40s left
  [observer] ca50: scope = 10 vars (7 DAG-ancestors + 3 data-prescreened) — 35 vars is run me discovery me nahi the (har run top-correlated rotate hote)
  [observer] ca50: raw=37 valid=22 accepted=20 unknown=1 rejected=16 (stored=21: 20 confirmed + 1 provisional)
  [observer] ca50: rejection reasons → 10× insufficient_evidence · 6× forbidden_direction
  DIFC · Sensitivity calibration... · 65%  · ~48s left
  DIFC · SBL learning update... · 72%  · ~41s left
  DIFC · ACIS reasoning... · 80%  · ~26s left
  DIFC · Querying per-target decisions... · 87%  · ~15s left
  DIFC · Multi-objective tradeoff... · 93%  · ~7s left
  DIFC · Building causal graph... · 97%  · ~3s left
  DIFC · ✅ Done — 21 edges · 100%

  [observer] DIFC complete — causal structure updated
  [observer] Economy: 21.0808 knowledge-value · discovery 48.937 CPU-sec · maintenance 628.231 CPU-sec
  [observer] epistemic: U_total=0.25 · latent=0.93 (max; mean 0.76)  (type 'ceh' for breakdown)
  DIFC: 200 cycles → 9 vars → 97 features → solver 26.74s → 72 edges  [⚠collinear]
  [observer] reliability verified: ca50 — coverage 21/21 hops (52.0s background)

  [observer] New engine cycles arrived — causal model now stale (re-run DIFC to refresh)
  You ▸ bye
  EBIS: shutting down — step by step...
    ✓ knowledge preserved (40 experiments, 178 sensitivities)
    ✓ engine stopped
    · organism updater not running
    ✓ all EBIS threads stopped — clean
  EBIS: session ended.
================================================================
  ✓ PASS  (3522.9s)  root=/content/ebis_extracted  version=v25  sig=39bc78610bdf
================================================================
```

## Engineering Notes

- **Both stabilization decisions routed through the same actuator.** `phasing/mbt_b` was selected in both attempts as the highest-measured-sensitivity actuator (sens=538.177) for reaching the un-actuatable causal variable `ignition_delay_index`. This was a measured-sensitivity ranking, not a direct causal-graph actuation.
- **Trust stayed low in both attempts.** `UNRELIABLE` (0.0132, freshness stale) on the failed first attempt; `WEAK` (0.1673, "limited by reliability_factor=0.48") on the passing second attempt. Neither attempt was reported by the system as high-confidence.
- **DIFC reruns during this session consistently flagged collinearity** (`⚠collinear`) and rejected roughly 40–60% of raw candidate edges per run, mostly for `insufficient_evidence` or `forbidden_direction`.
- **Graph staleness triggered an automatic DIFC re-run mid-session** ("Self-heal: graph stale + control failing → auto re-running DIFC") after the first stabilization attempt failed verification.
- **An upstream hypothesis test on `volumetric_efficiency` was logged as unverified both times it was tried** (via an `rpm` perturbation): the downstream variable did not move either time, and the change was rolled back rather than retained.
- **One prediction was scored in this session:** a `ca50` reversion forecast (predicted +3.3°, actual +2.4°) landed within tolerance (error 0.9, ≈30% of σ) and was logged as a hit.
- **The session's final DIFC run (21 edges, 21/21 hops verified) completed after the "bye" command was issued**, i.e. after the point the session was already being wound down — this is reflected as-is in the transcript above.

## Evidence References

- `README.md` — Case Study — v25 Autonomous Stabilization Session (summary of this transcript).
- `PROJECT_STATUS_AUGUST_2026.md` — v227 forensic causal-discovery audit (a separate, discovery-only session; complementary evidence for the DIFC pipeline behavior referenced here).
- This document — the complete, unedited transcript and primary evidence source for the v25 session.
