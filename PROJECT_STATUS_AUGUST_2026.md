# EBIS — Project Status: August 2026

**Runtime audit reference:** v227 forensic session · boot signature `25e76ee6` · 41 raw log blocks, fully preserved and independently annotated (`EBIS_v227_Annotated_Audit_Trail.pdf`)

---

## Executive Summary

This report documents a second, independently-audited EBIS runtime session (v227), separate from the earlier v25 stabilization session already reflected in the project README. Where the v25 audit demonstrated closed-loop actuator control, this session demonstrates the layer beneath it: reproducible causal discovery, an epistemic-uncertainty framework that decomposes confidence into named, auditable components, and a prediction-verification loop that logs both a correct forecast and a missed one without suppressing the miss.

No actuator experiments were run in this session. Trust for every investigated target (`ca50`, `volumetric_efficiency`, `intake_velocity`) remained at 0.00 throughout — the system did not convert structural discovery into causal confidence on its own, and did not claim otherwise.

## Objective

Validate, under forensic (block-by-block, unmodified) audit conditions, whether EBIS's causal-discovery pipeline: (1) reproduces its own structure across repeated runs, (2) correctly filters candidate edges against physics-derived direction constraints, (3) reports its own uncertainty as separable, named components rather than a single score, and (4) scores its own predictions against later ground truth — including when it is wrong.

## Initial Runtime State

- Boot signature `25e76ee6`, 47 files, 7/7 modules imported, 12 templates, manual mode, 2.9s boot.
- Prior investigation context restored automatically: branch `evidence verification`, last target `ca50`, 6 variables — the session did not start from a blank state.
- Engine started offline; buffer began at 5 cycles after warm-up.

## Runtime Evidence

- Kernel/hub reused from an externally-built 172-node actuator surface; 24 kernel constants (AFR, valve timings, LHV, etc.) exposed as addressable nodes.
- A data-repair audit ran twice during the session and reported clean input columns both times — no silent data correction occurred.
- Per-target graph staleness was tracked explicitly in engine cycles throughout (e.g. the `ca50` graph aged from 190 → 435 → 1020 → 1070 cycles while other targets were under investigation), rather than left implicit.

## Autonomous Causal Discovery

`ca50` was analyzed twice in the same session:

| Run | Duration | Raw candidates | Accepted | Rejected | Stored |
|---|---|---|---|---|---|
| 1 (cold) | 98.3s | 36 | 22 | 12 (forbidden_direction) | 24 (22 confirmed + 2 provisional) |
| 2 (warm re-run) | 18.0s | 27 | 14 | 9 (forbidden_direction) | 18 (14 confirmed + 4 provisional) |

Both graphs were independently re-walked by a background reliability verifier and confirmed at full coverage (24/24 hops, then 18/18 hops). Every rejection in both runs was attributed to `forbidden_direction` — a candidate edge whose orientation violated the physics-derived prior — not to noisy or insufficient data.

Two further targets were analyzed cleanly, with zero or near-zero rejected edges:

| Target | Raw | Accepted | Rejected | U_total |
|---|---|---|---|---|
| `volumetric_efficiency` | 28 | 20 | 6 (5 insufficient_evidence + 1 forbidden_direction) | 0.45 |
| `intake_velocity` (run 1) | 15 | 15 | 0 | 0.25 |
| `intake_velocity` (run 2) | 12 | 12 | 0 | 0.25 |

`intake_velocity` produced the lowest uncertainty of the session (`U_total = 0.25`, `latent = 0.06` on its core driver edges) — its manifold/intake-temperature relationships were the most cleanly identified structure observed in this audit.

The system also self-flagged a spurious-correlation risk: `ca10`/`ca50`/`ca90` are three percentiles of the same burn curve, and every edge among them was explicitly labeled a sibling/co-output relationship rather than a physical driver, even where the raw correlation weight was the largest in the graph.

## Learning Progress

The CEH signal panel decomposed uncertainty into named, independently-reported components for `ca50`: edge uncertainty (`Ue = 0.056`), coefficient stability (`σe = 0.118`), observability (`Uo = 0.225`), structure/rejection rate (`Us = 0.333`), regime freshness (`Ur = 0.000`), trust (`T = 0.00`), and latent hidden-factor risk (`L = 0.71` max). `U_total` was reported as a weighted aggregate of these named parts (`0.371`), not a single opaque figure.

Knowledge-value (an internal economy metric) rose across the session — 5.51 → 11.37 → 14.60 → 19.32 → 23.05 — alongside cumulative compute cost, giving a running efficiency figure (knowledge-value per CPU-second) rather than a cost-blind discovery process.

## Prediction Verification

Two forecasts were scored against later observed values:

- `volumetric_efficiency`: predicted +0.9, actual +1.0 — error 0.1, logged as a hit.
- `intake_velocity`: predicted +104.4, actual +106.9 — error 2.5, logged explicitly as **MISSED**.

The miss is notable because `intake_velocity` had the *lowest* measured uncertainty and *highest* stated confidence of any target in the session. The confidence label reflected structural/statistical stability (a mean-reversion fit), not a guarantee of realized accuracy — and the miss was retained in the record rather than dropped from the trace.

## Engineering Evidence Summary

- **Reproducibility** — `ca50` structure and reliability held across two independent discovery runs.
- **Physics-consistency** — 100% of rejected `ca50` edges in both runs were direction-constraint violations, not arbitrary filtering.
- **Epistemic transparency** — uncertainty reported as named, separable components, with an explicit note that trust and latent risk cannot be inferred from edge count alone.
- **Predictive honesty** — one verified hit and one verified miss both retained in the record.
- **Data integrity** — zero data repairs needed across two in-session audits.

## Current Project Maturity

This session demonstrates a working, reproducible discovery-to-prediction pipeline with auditable uncertainty reporting. It does not, on its own, demonstrate causal control — trust remained at 0.00 for all three targets because no actuator interventions were run in this session. Closed-loop actuator stabilization (raising `ca50` trust through measured intervention, though the resulting trust band remained `WEAK` rather than high-confidence) was demonstrated separately in the v25 runtime session, documented in the project README's Case Study section and preserved verbatim in `EBIS_Runtime_Stabilization_v25_Report.md`.

## Next Research Direction

- Run an actuator-experiment campaign against `volumetric_efficiency` and `intake_velocity`, following the same experiment → verify pattern already validated for `ca50` stabilization in the v25 session, to move their trust scores off 0.00.
- Investigate the `intake_velocity` prediction miss specifically — the target with the cleanest structure produced the largest forecast error, which is worth its own targeted follow-up rather than being averaged away.
- Continue background reliability re-verification as a standing check on every newly committed graph, as done throughout this session.

---

*This document reports only what the v227 runtime log recorded. No source code was inspected or reproduced in its preparation.*
