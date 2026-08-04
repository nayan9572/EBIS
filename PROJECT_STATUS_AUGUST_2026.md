# EBIS — Project Status: August 2026

**Runtime evidence reference:** v25 autonomous stabilization session, boot signature `39bc7861` · build `ebis_DEV_BUILD_v25_B0_P6_OPEN_WITH_DOCS.zip` (188 entries, integrity OK) · complete verbatim transcript preserved in `EBIS_Runtime_Stabilization_v25_Report.md`

> **Note on dating:** This is the August 2026 status report. The runtime session it documents (v25, boot `39bc7861`) is EBIS's first closed-loop control session — the point where the system moved from *discovering* causal structure to *acting on it and verifying the result*. A separate, earlier session (v227, boot `25e76ee6`, recorded July 2026) exercised the discovery layer only, with no actuator experiments; that session is summarized in the Appendix for context, not as this month's result.

---

## Executive Summary

This report documents EBIS's first fully closed-loop autonomous session: causal discovery, autonomous actuator selection, a failed stabilization attempt, self-detected graph staleness, an automatic recovery re-run, and a second attempt that passed verification and held under a 30-second persistence check.

This is a different capability than anything shown before it. Earlier sessions (see Appendix) showed EBIS could discover and reproduce causal structure — but trust stayed at 0.00 throughout, because no actuator ever moved. In this session, EBIS ran 40 experiments across 12 actuator nodes, learned 178 sensitivities, selected an actuator autonomously, tried, failed, healed itself, and tried again — successfully. Decision-level trust rose from `0.0132 (UNRELIABLE)` on the failed attempt to `0.1673 (WEAK)` on the passing one. The system did not overstate this: `WEAK` is the runtime's own label, not a rounded-up success claim.

## Objective

Validate, under complete unedited transcript conditions, whether EBIS's closed-loop control pipeline can: (1) autonomously select an actuator from measured sensitivities rather than a fixed rule, (2) detect and recover from its own control failure without user intervention, (3) pass a multi-objective verification check (target stabilized, side-effect not worsened), and (4) sustain that result under a persistence check rather than a single-instant read.

## Initial Runtime State

- Boot signature `39bc7861`, 62 files, 7/7 modules imported, 12 templates, manual mode, 13.8s boot.
- Boot warnings logged and not hidden: 4 modules loaded from `legacy/` (shadowing `src/`); CER not live.
- Prior investigation context restored automatically: branch `evidence verification`, last target `ca50`, 6 variables.
- Engine resumed live with the externally-built 172-node actuator surface already warmed and buffered.

## Autonomous Multi-Node Experiment

A `full experiment karo` run swept 12 actuator nodes against `ca50` — phasing (`mbt_a`, `mbt_b`, `kp_nom`, `ki_nom`, `kd_nom`), closed-loop turbulence and Wiebe parameters, and a kernel-compression term — across 960 cycles in 287.7 seconds.

| Result | Count | Example |
|---|---|---|
| STRONG intervention verdict | 2 | `burn_duration ← burn_acceleration`, `knock_intensity ← burn_acceleration` |
| WEAK intervention verdict | 12 | e.g. `ca50 ← ignition_delay_index` |
| Insufficient | 7 | — |

Each of the 12 nodes was logged through a five-stage audit (`observation → evidence → learning → prediction-calibration → decision-impact`) with an explicit `trust +0.000` when a step contributed no trust gain — the audit recorded null results as plainly as positive ones. All 12 measured sensitivities were written to experiment memory for the stabilizer to use afterward.

A separate upstream hypothesis test tried to move `volumetric_efficiency` indirectly via an `rpm` perturbation (bounded ±5%). The downstream variable did not move, and the change was rolled back rather than kept — this was attempted twice in the session, both times with the same negative, logged, non-retained result.

## Closed-Loop Stabilization: Attempt 1 (Failed)

**Baseline:** `ca50 = -1.1°`, oscillation `= 0.95`.

**Reasoning:** DIFC identified `ignition_delay_index → ca50` as the dominant cause (`R² = 0.9969`, `weight = 0.7983`). Since `ignition_delay_index` is a measured variable and not directly actuatable, EBIS selected `phasing/mbt_b` as the actuator — the node with the highest measured sensitivity (`538.177`) among all 12 eligible actuators. The other 11 were rejected explicitly, each with its own logged sensitivity and reason (e.g. `mbt_a` at `0.103`, `ki_nom` at `84.65`).

**Trust at decision time:** `0.0132 (UNRELIABLE)` — the runtime's own reason: `freshness STALE — re-run DIFC`. EBIS proceeded anyway and logged the low-trust basis rather than blocking on it.

**Execution:** `mbt_b` was nudged (`0.0075 → 0.0078`); `ca50` converged toward the mean (`-0.4°`) but oscillation remained high (`std = 3.4°`).

**Verification:** `VERIFY: FAIL` — `ca50 = -1.29°`, `std = 3.32°`. Three closed-loop re-nudges (`mbt_b` stepped `0.0078 → 0.0084 → 0.0091 → 0.0098`) did not resolve the oscillation. The runtime logged its own conclusion: *"Oscillation persists — spark-only control insufficient."*

## Self-Healing: Automatic Recovery

Without user action, EBIS detected that its causal graph had gone stale mid-attempt and triggered a fresh DIFC run on its own (*"Self-heal: graph stale + control failing → auto re-running DIFC"*). The re-run completed in line with the session's other discovery passes (15 edges, reliability-verified at full 15/15 hop coverage) before the second stabilization attempt began.

## Closed-Loop Stabilization: Attempt 2 (Passed)

**Baseline:** `ca50 = +2.0°`, oscillation `= 0.95`, against the freshly refreshed graph.

**Reasoning:** Same causal driver (`ignition_delay_index → ca50`) and same actuator choice (`phasing/mbt_b`, `sens = 538.177`) — the decision was reproducible across both attempts, not re-derived arbitrarily.

**Trust at decision time:** `0.1673 (WEAK)` — up from `0.0132 (UNRELIABLE)` on the first attempt. Reason given: `limited by reliability_factor=0.48`. The runtime did not report this as high-confidence; `WEAK` is its own label.

**Execution:** Seven `spark` lever nudges were applied, each logged with the resulting `ca50` reading (`+1.6° → +1.4° → +1.5° → +2.8° → +1.5° → +0.9°`). The mean converged (`ca50 = +0.9°`) but the runtime flagged continued oscillation (`std = 3.2°`) and ran its own causal ranking of the oscillation source, identifying `burn` as the highest-weight *controllable* contributor (`ignition` was flagged but not controllable).

**Verification:**

| Check | Result |
|---|---|
| Initial verify | `PASS` — `ca50 = +1.8°`, `std = 2.74°`, "held near setpoint, low oscillation" |
| Multi-objective check | `PASS` — `ca50` stabilized **and** knock not worsened (knock actually improved slightly: `26.5 → 27.1`, net `-0.6` on the tracked delta) |
| Persistence check (+30s) | `STABLE` — `ca50 = +1.0°`, `std = 2.2°`, knock `= 27.5` |
| Evidence basis | `prediction n=40`, `causal edges=45`, `confidence=moderate` |

## Prediction Verification

A single reversion-based forecast for `ca50` was scored against a later observed value: predicted `+3.3°`, actual `+2.4°` — error `0.9` (≈30% of σ), logged as a verified hit.

## Session Close

The session ended cleanly on a `bye` command: 40 experiments and 178 sensitivities preserved to persistent memory, engine stopped, all threads shut down clean. A final DIFC run on `ca50` (21 edges) completed and was reliability-verified at full 21/21 hop coverage after the shutdown command had already been issued — captured as-is in the transcript rather than trimmed for tidiness. Total session duration: 3522.9s (~58.7 minutes). Session result: **PASS**.

## Engineering Evidence Summary

- **Autonomous actuator selection** — chosen from 12 measured sensitivities, not a fixed rule; rejected alternatives logged with reasons.
- **Failure is not hidden** — the first stabilization attempt's `FAIL` verdict, its three failed re-nudges, and the exact reason (`"spark-only control insufficient"`) are all in the record.
- **Self-healing** — stale-graph detection and DIFC re-run triggered automatically, without a user prompt, mid-session.
- **Honest trust reporting** — decision trust moved `UNRELIABLE → WEAK`, not `UNRELIABLE → high-confidence`; the runtime's own labels were retained as-is.
- **Multi-objective verification** — stabilization was checked against both the target (`ca50`) and a side-effect (`knock`), not the target alone.
- **Persistence, not a single-instant read** — the pass was re-checked 30 seconds later before being accepted.

## Current Project Maturity

This session is EBIS's first demonstrated instance of the full discover → act → fail → self-heal → verify loop running autonomously in one continuous session. It moves the project past the discovery-only stage described in the Appendix: trust is no longer pinned at 0.00 for `ca50`, because an actuator intervention was actually run and independently verified. The achieved trust band (`WEAK`) is itself an honest signal of where the project stands — this is evidence of a working control loop, not yet evidence of a high-confidence one.

## Next Research Direction

- Extend the same experiment → verify pattern validated here for `ca50` to `volumetric_efficiency` and `intake_velocity`, which remained at trust `0.00` in the discovery-only session (Appendix) and were not targeted by this session's actuator experiments.
- Investigate why the `mbt_b` actuator path stayed in the `WEAK` trust band even after a passing verification — the `reliability_factor=0.48` cited as the limiting factor is worth its own targeted follow-up.
- Continue tracking the unexplained `trust(target)` figure that appeared inside the second attempt's correction-loop lines (drifting `0.71 → 0.70 → 0.69`) — the transcript does not define its formula or relate it to the decision-level trust reported elsewhere, so it should be traced to source before being treated as equivalent to that score.
- Carry forward the background reliability re-verification pattern (every newly committed graph independently re-walked) as a standing check on future sessions.

---

*This document reports only what the v25 runtime log recorded. No source code was inspected or reproduced in its preparation.*

---

# Appendix — Discovery-Only Session for Context (v227, July 2026)

*This appendix summarizes an earlier, separate runtime session — boot signature `25e76ee6`, recorded July 2026 — included here for context on where the discovery layer stood before this month's closed-loop session. The verbatim transcript remains the sole evidence source and lives in `EBIS_v227_Annotated_Audit_Trail.pdf`. Nothing in this appendix is this month's result.*

The v227 session exercised causal discovery only. No actuator experiments were run, and trust for every investigated target (`ca50`, `volumetric_efficiency`, `intake_velocity`) stayed at `0.00` throughout — the system did not convert structural discovery into causal confidence, and did not claim otherwise.

**Initial state.** Boot signature `25e76ee6`, 47 files, 7/7 modules, 12 templates, 2.9s boot. Prior context restored: branch `evidence verification`, last target `ca50`.

**Reproducibility.** `ca50` was analyzed twice:

| Run | Duration | Raw candidates | Accepted | Rejected | Stored |
|---|---|---|---|---|---|
| 1 (cold) | 98.3s | 36 | 22 | 12 (`forbidden_direction`) | 24 (22 confirmed + 2 provisional) |
| 2 (warm re-run) | 18.0s | 27 | 14 | 9 (`forbidden_direction`) | 18 (14 confirmed + 4 provisional) |

Both graphs were independently reliability-verified at full coverage (24/24, then 18/18 hops). Every rejection in both runs was a physics-direction violation, not noisy or insufficient data.

**Clean targets.** Two further targets were analyzed with near-zero rejections:

| Target | Raw | Accepted | Rejected | U_total |
|---|---|---|---|---|
| `volumetric_efficiency` | 28 | 20 | 6 (5 insufficient_evidence + 1 forbidden_direction) | 0.45 |
| `intake_velocity` (run 1) | 15 | 15 | 0 | 0.25 |
| `intake_velocity` (run 2) | 12 | 12 | 0 | 0.25 |

`intake_velocity` produced the session's lowest uncertainty (`U_total = 0.25`, `latent = 0.06` on core driver edges) — its manifold/intake-temperature relationships were the most cleanly identified structure in the audit.

**Self-flagged spurious correlation.** `ca10`/`ca50`/`ca90` are three percentiles of the same burn curve; every edge among them was explicitly labeled a sibling/co-output relationship rather than a physical driver, even where the raw correlation weight was the graph's largest.

**Uncertainty decomposition.** The CEH signal panel reported `ca50` uncertainty as named, separable components rather than one score: edge uncertainty (`Ue = 0.056`), coefficient stability (`σe = 0.118`), observability (`Uo = 0.225`), structure/rejection rate (`Us = 0.333`), regime freshness (`Ur = 0.000`), trust (`T = 0.00`), latent risk (`L = 0.71` max) — aggregating to `U_total = 0.371`.

**Prediction verification.** Two forecasts were scored:

- `volumetric_efficiency`: predicted `+0.9`, actual `+1.0` — error `0.1`, logged as a hit.
- `intake_velocity`: predicted `+104.4`, actual `+106.9` — error `2.5`, logged explicitly as **MISSED**, despite this target having the session's lowest measured uncertainty and highest stated confidence. This gap was not re-tested in the August (v25) session above, since that session targeted `ca50`/`spark_angle_deg` — it remains open.

**Data integrity.** A data-repair audit ran twice and reported clean columns both times.

**What this session did not show.** No actuator ever moved; there was no stabilization attempt, no failure, no self-heal, and no persistence check — that entire capability is what the August (v25) session above demonstrates for the first time.
