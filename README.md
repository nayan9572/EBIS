<div align="center">

# 🧠 EBIS — Engine Behavior Intelligence System

### *Evidence-driven Runtime Intelligence for Scientific Decision Support*

![Status](https://img.shields.io/badge/status-active%20research-brightgreen)
![Phase](https://img.shields.io/badge/phase-DIFC%20Evolution-blue)
![Domain](https://img.shields.io/badge/domain-Causal%20Engine%20Intelligence-orange)
![License](https://img.shields.io/badge/license-see%20LICENSE-lightgrey)

**Project Status:** 🟢 Active Research (July 2026)

</div>

---

EBIS has evolved beyond a conventional engine simulation project into a research platform focused on **runtime causal reasoning, evidence-backed decision support, trust estimation, uncertainty management, and continuous scientific validation**. Rather than producing static predictions, EBIS is designed to continuously observe system behavior, discover candidate causal relationships, validate them against evidence, estimate confidence, and recommend the next most valuable experiment.

> 📄 **Current Research Status:** See `PROJECT_STATUS_JULY_2026.md` for the latest project maturity, milestones, findings, and roadmap.

---

## 📑 Table of Contents

- [Project Status — July 2026](#project-status--july-2026)
- [What is EBIS?](#what-is-ebis)
- [Recent Progress](#recent-progress)
- [🔬 Major Findings](#-major-findings)
- [🧬 Theoretical Foundation — CEH](#-theoretical-foundation--causal-entropy-hypothesis-ceh)
- [🏛️ Runtime Authority Architecture](#️-runtime-authority-architecture)
- [🛣️ Current Research Roadmap](#️-current-research-roadmap)
- [🌍 Long-Term Vision](#-long-term-vision)
- [📚 Project Resources](#-project-resources)
- [🤝 Development Philosophy](#-development-philosophy)
- [🚧 Current Availability](#-current-availability)
- [📈 Future Direction](#-future-direction)
- [📜 Intellectual Property Notice](#-intellectual-property-notice)
- [Closing Statement](#closing-statement)

---

## Project Status — July 2026

During the recent development cycle, the project has matured significantly in both architecture and research direction.

### Current Maturity

- ✅ Fuel integration phase completed.
- ✅ Runtime authority architecture refined.
- ✅ DIFC-based causal reasoning expanded.
- ✅ Canonical runtime truth and governance model formalized.
- ✅ Multiple forensic architecture audits completed.
- ✅ Documentation and engineering governance substantially improved.
- ✅ Second full forensic runtime audit completed (v227) — causal discovery, epistemic decomposition, and prediction verification independently validated.

The current objective is no longer limited to building an engine model. The broader goal is to develop an **evidence-driven scientific reasoning system** that continuously improves its understanding through observation, validation, uncertainty estimation, and controlled experimentation.

### Latest Runtime Milestone — v227 Forensic Audit

A second fully-annotated runtime audit (build v227, boot signature `25e76ee6`) validated the causal-discovery-to-prediction pipeline end-to-end across three independent targets (`ca50`, `volumetric_efficiency`, `intake_velocity`):

- **Causal discovery, repeated and reproduced.** Two independent DIFC runs on `ca50` (98.3s cold, then 18.0s warm) produced consistent causal structure, each independently re-verified at full coverage by a background reliability check (24/24 hops, then 18/18 hops).
- **Epistemic uncertainty made auditable.** Uncertainty was reported as named, separable components — edge fit, coefficient stability, observability, structural rejection rate, regime freshness, and trust — rather than a single composite score.
- **Prediction verification, including an honest miss.** `volumetric_efficiency` was predicted at +0.9 and resolved at +1.0 (hit); `intake_velocity` was predicted at +104.4 and resolved at +106.9 — a 2.5-unit miss, logged explicitly rather than omitted.
- **Trust stayed at zero without experiments.** Trust for every target remained 0.00 through the session because no actuator interventions were run — structural discovery was not converted into causal confidence on its own.

Full block-by-block detail is in `PROJECT_STATUS_AUGUST_2026.md`.

### Case Study — v25 Autonomous Stabilization Session

A separate runtime session (build `ebis_DEV_BUILD_v25_B0_P6_OPEN_WITH_DOCS.zip`, boot signature `39bc7861`) exercised the closed-loop control path that a discovery-only audit does not: DIFC causal discovery on `ca50`, a 12-node autonomous actuator sweep, and two live stabilization attempts.

- **First stabilization attempt failed verification.** With the causal path routed through `ignition_delay_index → ca50` and actuated via `phasing/mbt_b` (the highest measured-sensitivity actuator available), `ca50` held near its error target but oscillation (`std=3.3°`) exceeded the pass threshold. Three closed-loop re-nudges did not resolve it; the system flagged its own causal graph as stale and automatically re-ran DIFC before trying again.
- **Second attempt passed**, after a fresh DIFC pass whose 20 edges were independently reliability-verified at full coverage (20/20 hops): `ca50` converged to `+1.8°` with `std=2.74°`, and a +30s persistence check confirmed the hold (`ca50=+1.0°`, `std=2.2°`).
- **Trust in the actuator path stayed low in both attempts** — `UNRELIABLE` (0.0132) on the failed attempt, `WEAK` (0.1673) on the passing one. The system reported this explicitly rather than presenting the pass as high-confidence.
- A single reversion forecast for `ca50` (predicted `+3.3°`, actual `+2.4°`, error `0.9` ≈ 30% of σ) was scored within tolerance and logged as a verified hit.
- The session closed cleanly after 40 recorded experiments and 178 recorded sensitivities.

The complete, unedited runtime transcript is preserved verbatim in `EBIS_Runtime_Stabilization_v25_Report.md`.

---

## What is EBIS?

**EBIS (Engine Behavior Intelligence System)** is a runtime intelligence framework that combines physics, causal discovery, evidence evaluation, and decision support into a unified architecture.

Instead of treating discovered relationships as permanent knowledge, EBIS continuously evaluates every hypothesis using runtime evidence. New observations may strengthen existing beliefs, reduce confidence, reveal uncertainty, or invalidate previous conclusions.

The long-term vision is to enable systems that can not only **predict** behavior, but also **explain why** behavior occurs, **quantify how trustworthy** those explanations are, **identify remaining knowledge gaps**, and **recommend the most informative next experiment**.

---

## Recent Progress

Recent work has focused on strengthening the scientific foundations of EBIS rather than simply adding features.

Major progress includes:

- Completion of the **Fuel Integration phase**.
- Refinement of runtime authority and governance architecture.
- Expansion of the **DIFC** causal reasoning framework.
- Improved canonical truth management and runtime consistency.
- Formalization of documentation, implementation context, and architectural rules.
- Completion of multiple forensic audits to identify structural gaps and guide future development.

These improvements move EBIS closer to becoming a research-grade runtime reasoning platform while preserving the existing orchestrator and physics-driven architecture.

---

## 🔬 Major Findings

The recent evolution of EBIS has shifted the project's emphasis from feature development toward building a scientifically rigorous runtime reasoning framework. Several architectural and research findings now guide all future development.

### Single Authority Architecture

One of the strongest conclusions from multiple forensic audits is that every critical responsibility should have **one authoritative owner**.

Rather than introducing parallel implementations, EBIS now evolves by extending existing components while preserving a single source of truth throughout the execution pipeline.

This philosophy reduces ambiguity, simplifies validation, improves maintainability, and makes every runtime decision traceable.

### Evidence Before Belief

EBIS treats every discovered relationship as a **working scientific hypothesis**, not as permanent knowledge.

Relationships are expected to:

- accumulate evidence,
- survive repeated validation,
- be challenged under changing operating conditions,
- lose confidence when contradicted,
- and eventually be promoted, revised, or rejected.

Knowledge therefore remains dynamic rather than static.

### Trust and Uncertainty as First-Class Concepts

Prediction alone is not sufficient.

Every important conclusion should answer questions such as:

- How reliable is this conclusion?
- What evidence supports it?
- Which operating regimes have been observed?
- What uncertainty still remains?
- What information is still missing?

Consequently, trust estimation, uncertainty quantification, evidence quality, and confidence management have become **core architectural responsibilities** instead of optional diagnostics.

### Continuous Scientific Validation

Scientific knowledge should never become permanent simply because it worked once.

EBIS continuously re-evaluates existing beliefs using:

- runtime observations,
- statistical validation,
- physics consistency,
- multi-regime evidence,
- prediction residuals,
- and targeted experimentation.

The objective is continuous learning rather than static calibration.

### Knowledge Gaps Drive Research

Recent development has also shifted the project's objective.

Rather than asking only:

> *"What does the model predict?"*

EBIS increasingly asks:

- What remains unknown?
- Which uncertainty matters most?
- Which experiment would reduce uncertainty the fastest?
- Which evidence would most improve trust?

This represents an important transition from prediction toward **evidence-guided scientific reasoning**.

---

## 🧬 Theoretical Foundation — Causal Entropy Hypothesis (CEH)

EBIS is grounded in the **Causal Entropy Hypothesis (CEH)**, which provides the conceptual framework for reasoning about uncertainty, causal evolution, and knowledge formation.

Within this perspective, entropy is interpreted as the **realized unfolding of causal structure** rather than simply a measure of disorder or missing information.

This theoretical viewpoint motivates several important design principles throughout EBIS:

- uncertainty should be represented explicitly rather than ignored,
- hidden causal variables continuously influence observable behavior,
- causal knowledge must remain revisable as new evidence appears,
- scientific confidence should increase through accumulated evidence instead of assumptions,
- and experimental validation is an essential part of knowledge creation.

CEH therefore serves not only as a theoretical proposal but also as an **engineering principle** that influences runtime reasoning, evidence management, trust estimation, and future scientific exploration.

---

## 🏛️ Runtime Authority Architecture

EBIS follows a strict single-authority runtime architecture in which every major responsibility has exactly one canonical owner. This design minimizes ambiguity, improves traceability, and ensures that scientific reasoning remains auditable.

| Layer | Primary Responsibility |
|---|---|
| **Orchestrator** | Coordinates runtime execution and maintains canonical system state. |
| **Calibration Hub** | Owns calibration values and controlled parameter evolution. |
| **Theta Kernel** | Executes the core physics model and remains the authoritative physics engine. |
| **Outputs Layer** | Produces standardized runtime observations and derived metrics. |
| **Observer** | Detects anomalies, residuals, instability, and runtime inconsistencies. |
| **DIFC** | Discovers, evaluates, and refines candidate causal relationships. |
| **Trust Layer** | Quantifies evidence quality, confidence, uncertainty, and belief maturity. |

### Architectural Principles

The current architecture is guided by several long-term engineering principles:

- One responsibility → one authoritative owner.
- Extend existing components rather than introducing parallel subsystems.
- Preserve the integrity of the Theta physics kernel.
- Every decision should remain explainable through evidence.
- Runtime evolution should be incremental, measurable, and scientifically defensible.

These principles emerged from multiple architecture reviews and continue to guide all future development.

---

## 🛣️ Current Research Roadmap

The current phase of EBIS is focused on transforming the system into a mature evidence-driven scientific reasoning platform.

### Phase 1 — Strengthen Evidence Pipeline
Improve how observations, experiments, and runtime measurements contribute to confidence estimation and long-term knowledge.

### Phase 2 — Trust & Uncertainty
Further develop mechanisms that quantify:
- evidence quality,
- confidence,
- uncertainty,
- knowledge maturity,
- and operating regime coverage.

The objective is to make every recommendation accompanied by a transparent estimate of reliability.

### Phase 3 — Intelligent Experiment Planning
Enable EBIS to recommend the next most informative experiment instead of relying solely on predefined testing procedures.

Future experimentation should maximize expected knowledge gain while minimizing unnecessary cost and risk.

### Phase 4 — Knowledge Promotion
Formalize the lifecycle through which scientific hypotheses evolve.

Candidate relationships should gradually progress from:

1. Initial Observation
2. Candidate Hypothesis
3. Validated Evidence
4. High-Confidence Knowledge
5. Long-Term Scientific Memory

Each promotion stage must be supported by accumulated evidence rather than isolated observations.

### Phase 5 — Real-World Validation
Continue validating EBIS using diverse engine operating conditions and experimental datasets.

Long-term objectives include:
- broader operating regime coverage,
- stronger agreement with physical measurements,
- improved robustness,
- and increased confidence in deployment as a scientific decision-support platform.

---

## 🌍 Long-Term Vision

The long-term vision of EBIS extends beyond engine simulation.

The project aims to become an evidence-driven runtime intelligence framework capable of:

- continuously learning from observations,
- reasoning under uncertainty,
- explaining causal behavior,
- recommending scientifically valuable experiments,
- maintaining trustworthy knowledge,
- and supporting engineering decisions with transparent evidence.

The guiding philosophy remains simple:

> *"Knowledge should be earned through evidence, continuously challenged by reality, and refined as new observations emerge."*

---

## 📚 Project Resources

The repository contains documentation describing the project's architecture, implementation history, research direction, and engineering decisions.

Key documents include:

- **`PROJECT_STATUS_JULY_2026.md`** — Current project maturity, milestones, findings, and roadmap.
- **`PROJECT_STATUS_AUGUST_2026.md`** — Forensic causal-discovery audit (v227): reproducibility, epistemic decomposition, and prediction verification.
- **`EBIS_v227_Annotated_Audit_Trail.pdf`** — Full annotated log for the v227 forensic session (41 raw log blocks, independently annotated).
- **`causal_entropy_preprint_2026-06-06.pdf`** — Preprint describing the Causal Entropy Hypothesis (CEH) theoretical foundation referenced above.
- **`EBIS_Runtime_Stabilization_v25_Report.md`** — Complete verbatim runtime transcript for the v25 autonomous stabilization session (causal discovery, autonomous experimentation, and closed-loop actuator control on `ca50`).
- **`LICENSE`** — Governing terms of use for the repository.

These documents are intended to provide transparency into the project's evolution and the reasoning behind major architectural decisions.

---

## 🤝 Development Philosophy

EBIS is developed according to a set of engineering principles that remain consistent across all implementation phases.

The project emphasizes:

- Physics-first engineering.
- Evidence-driven scientific reasoning.
- Incremental architectural evolution.
- Single-authority ownership.
- Transparent decision making.
- Continuous validation.
- Long-term maintainability.

The objective is not to maximize the number of features, but to maximize the **scientific reliability** of every capability that becomes part of the system.

---

## 🚧 Current Availability

**Development Status:** Active Research

EBIS is an actively evolving research project.

The architecture, scientific framework, and documentation continue to mature as new findings emerge through experimentation, validation, and forensic engineering analysis.

Some components remain experimental and should be interpreted as ongoing research rather than finalized production systems.

---

## 📈 Future Direction

The next phase of development focuses on transforming EBIS from an advanced runtime engine intelligence framework into a broader scientific reasoning platform.

Future work will continue strengthening:

- Evidence accumulation.
- Trust estimation.
- Uncertainty management.
- Causal discovery.
- Knowledge lifecycle management.
- Experiment planning.
- Real-world validation.
- Scientific documentation and publication.

The long-term objective is to build a runtime system capable of learning responsibly, reasoning transparently, and supporting engineering decisions through continuously validated evidence.

---

## 📜 Intellectual Property Notice

The EBIS project, its architectural concepts, theoretical framework, research methodology, and associated documentation represent ongoing original research.

Unless explicitly stated otherwise, all intellectual property remains with the project author.

Please refer to the repository's existing `LICENSE` file and copyright notice for the governing terms of use.

---

## Closing Statement

<div align="center">

*"Reality remains the ultimate authority."*

</div>

Every model, prediction, hypothesis, and causal belief within EBIS exists to be challenged by evidence.

The long-term mission of the project is not merely to generate predictions, but to build systems capable of continuously improving their understanding of the physical world through observation, experimentation, validation, and transparent scientific reasoning.
