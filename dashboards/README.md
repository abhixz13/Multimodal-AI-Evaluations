# Multimodal AI Evaluations — Dashboards & Reports Design System

**Visual design specification and screen artifacts for Multimodal AI Evaluations**
---

## Purpose & Scope

This directory contains the complete design specification and visual artifacts for a practitioner-grade evaluation platform that operationalizes the Multimodal AI Product Evaluation framework as a living, interactive environment rather than a static document.

The platform covers the full evaluation lifecycle — from initial surface mapping and failure taxonomy through contract scoring, causal chain analysis, backlog prioritization, and published evaluation reports — for any multimodal AI product: document-to-audio systems, text-to-video generators, generative image tools, async video summarizers, and multimodal agents.

**Design scope:** Desktop-first (1440px), dark mode, practitioner audience. Not a consumer product — data density is intentional.

**Full specification:** [`DESIGN_SPEC.md`](./DESIGN_SPEC.md)
**Data model:** [`datamodels.md`](./datamodels.md)

---

## Design Philosophy

Five principles that govern every layout and component decision.

| # | Principle | Rule |
|---|---|---|
| P1 | **Upstream first** | Failures originate upstream and appear downstream. Stage 1 is always leftmost. Safety overrides always precede contract cards. |
| P2 | **Evidence is not optional** | Every numerical score carries a provenance badge — [SIM] / [EST] / [REV] / [INF] / [LIVE]. A score without a badge is a validation error. |
| P3 | **Safety is architecturally above everything** | The Safety Override Strip is a structural section that precedes all contract cards. It cannot be collapsed, dismissed, or scrolled past. |
| P4 | **Data density is a feature** | Tables are the correct UI for this domain. Resist all impulses to simplify into summary cards. |
| P5 | **The report is the product** | Every dashboard screen is a data entry point for the evaluation report. The rendered report is the canonical publishable output. |

---

## Design System

The platform uses a unified design system defined in full in [`DESIGN_SPEC.md`](./DESIGN_SPEC.md).

**Contract color tokens** — five colors permanently associated with the Five Quality Contracts (C1 amber-orange, C2 violet, C3 red, C4 blue, C5 green) plus deep red for C-SAFETY — function as a persistent visual vocabulary across all screens.

**Evidence badge system** — five pill badge types ([SIM] gray, [EST] amber, [REV] green, [INF] blue, [LIVE] purple) appear on every numerical score throughout the platform, making the epistemological status of each measurement immediately legible.

**Card elevation variants** — seven named card states (`card-default`, `card-pass`, `card-fail`, `card-override`, `card-unknown`, `card-hover`, `card-safety`) ensure that the C-SAFETY contract card is always visually distinct from the five quality contract cards, regardless of pass/fail status.

**Typography:** Inter (all text) + JetBrains Mono (scores, IDs, trace identifiers)
**Base:** Background `#0F1117` · Surface-1 `#1A1D27` · Surface-2 `#252836`
**Pipeline stages:** Nine distinct colors from S1 `#DC2626` (highest upstream risk) to S9 `#6B7280`

---

## Screens

The platform comprises 17 screens across four sections: Evaluation, Analysis, Products, and Governance.

---

### Screen 1 — Mission Control
10-second status check displaying safety overrides, all five contract health scores with sub-contract granularity, pipeline failure origin distribution, consumption signals, and overall evaluation verdict.
<img width="708" height="844" alt="screen-01-mission-control" src="https://github.com/user-attachments/assets/4d550fb1-5d4c-435e-8b06-f443bf16cb40" />


<!-- ![Mission Control](./images/screen-01-mission-control.png) -->

[📄 View PDF](./pdf/screen-01-mission-control.pdf)

---

### Screen 2 — Evaluation Brief (T-01)
Gated threshold-setting form that locks all evaluation runs until quality contract thresholds, judge panel, consumption targets, and riskiest assumption are documented before any scores are seen.
<img width="702" height="838" alt="screen-02-evaluation-brief" src="https://github.com/user-attachments/assets/5890a197-d16f-4cb7-8fcb-ebfb51355ac9" />


<!-- ![Evaluation Brief](./images/screen-02-evaluation-brief.png) -->

[📄 View PDF](./pdf/screen-02-evaluation-brief.pdf)

---

### Screen 3 — Judge Panel
Multi-rater protocol coordinator tracking blind labeling progress, Cohen's kappa per judge pair, disagreement queue (surfacing specification questions, not labeling noise), and tiebreaker escalation SLA.
<img width="1526" height="1624" alt="image" src="https://github.com/user-attachments/assets/fd9a4d1b-665b-443f-ba26-4e5a75fa3b2e" />


<!-- ![Judge Panel](./images/screen-03-judge-panel.png) -->

[📄 View PDF](./pdf/screen-03-judge-panel.pdf)

---

### Screen 4 — Five Contracts Dashboard
Per-contract deep-dive showing disaggregated sub-contract scores with evidence badges, 30-day trend sparklines, canary corpus live delta, evaluator kappa health, and a full failing-cases table.
<img width="1414" height="1836" alt="image" src="https://github.com/user-attachments/assets/354274c5-6d82-4b46-a971-b2929a1365eb" />


<!-- ![Five Contracts Dashboard](./images/screen-04-five-contracts.png) -->

[📄 View PDF](./pdf/screen-04-five-contracts.pdf)

---

### Screen 5 — Pipeline Inspector
Nine-stage pipeline map with product-specific stage labels, failure percentage and status per stage, upstream alert logic (fires when S1+S2 > 35%), and an expandable stage detail panel showing failure tags, blast radii, and causal chain roots.
<img width="1447" height="1797" alt="image" src="https://github.com/user-attachments/assets/d64f277e-b178-4bf3-9d5b-e79ff0dd1e41" />


<!-- ![Pipeline Inspector](./images/screen-05-pipeline-inspector.png) -->

[📄 View PDF](./pdf/screen-05-pipeline-inspector.pdf)

---

### Screen 6 — Trace Explorer
Individual trace view with stage-labeled span waterfall, contract verdicts with judge quotes and causal chain attribution, and a parallel audio-native evaluation track (waveform player with failure timestamp markers and six-dimension evaluation table) for products with audio output.
<img width="1396" height="1409" alt="image" src="https://github.com/user-attachments/assets/d329f9ff-c1b7-4b8b-bf1f-32a4d29d28c3" />

<!-- ![Trace Explorer](./images/screen-06-trace-explorer.png) -->

[📄 View PDF](./pdf/screen-06-trace-explorer.pdf)

---

### Screen 7 — Surface Mapper
Ten-trace open-coding workspace for the first step of the Analyze phase — capturing free-text failure observations across initial traces and surfacing emerging patterns before the formal taxonomy is built.
<img width="1471" height="1812" alt="image" src="https://github.com/user-attachments/assets/340f8a5e-3958-413d-9fc4-ae2ea3a32ce1" />


<!-- ![Surface Mapper](./images/screen-07-surface-mapper.png) -->

[📄 View PDF](./pdf/screen-07-surface-mapper.pdf)

---

### Screen 8 — Taxonomy Builder
Product-specific failure tag namespace manager organized by pipeline junction, showing blast radius, causal chain linkage, and case counts per tag — the source of truth for all trace tagging, failure mode library entries, and causal chain definitions.
<img width="1449" height="1651" alt="image" src="https://github.com/user-attachments/assets/668c16f6-8cbb-4576-bee8-62cd91be0140" />

<img width="1432" height="1628" alt="image" src="https://github.com/user-attachments/assets/3dbea832-28c3-43fc-a22d-00c55922583f" />

<!-- ![Taxonomy Builder](./images/screen-08-taxonomy-builder.png) -->

[📄 View PDF](./pdf/screen-08-taxonomy-builder.pdf)

---

### Screen 9 — Driver Analysis Workspace (T-13)
Structured five-step root-cause analysis tool (Observe → Hypothesize → Fix Surfaces → Recommend → Summarize) that auto-populates from run data, calculates priority formula scores for proposed fixes, enforces evidence badge requirements before recommendations are finalized, and generates a T-14 Decision Memo as its exit action.


<!-- ![Driver Analysis Workspace](./images/screen-09-driver-analysis.png) -->

[📄 View PDF](./pdf/screen-09-driver-analysis.pdf)

---

### Screen 10 — Failure Mode Library
Queryable library of all named failure patterns for a product, with stage-origin bar chart, blast radius leaderboard, and per-pattern cards showing occurrence counts with evidence badges, fix surface attribution, causal chain links, and backlog rank.

![Failure Mode Library](./images/screen-10-failure-mode-library.png)

[📄 View PDF](./pdf/screen-10-failure-mode-library.pdf)

---

### Screen 11 — Causal Chains
Named causal chain tracker showing root-to-downstream failure stories, expected lift after fix, and a required Blast Radius Explanation block — the narrative that makes legible why an upstream fix outranks a faster downstream fix in the priority formula.

![Causal Chains](./images/screen-11-causal-chains.png)

[📄 View PDF](./pdf/screen-11-causal-chains.pdf)

---

### Screen 12 — Backlog Prioritization (T-17)
Priority-formula-ranked fix backlog with a structurally separate safety override zone (rendered above all formula items, with no formula score) ensuring zero-tolerance items are never deprioritized against implementation effort.

![Backlog Prioritization](./images/screen-12-backlog.png)

[📄 View PDF](./pdf/screen-12-backlog.pdf)

---

### Screen 13 — Product Evaluation Report
Fourteen-section evaluation report assembled from live platform data — contract scores, backlog, driver analysis output, decision trigger table, and auto-generated evidence provenance index — rendered as a shareable document with one-click PDF export.

![Product Evaluation Report](./images/screen-13-report.png)

[📄 View PDF](./pdf/screen-13-report.pdf)

---

### Screen 14 — Maturity Model
Five-level evaluation maturity tracker (Ad Hoc → Instrumented → Systematic → Closed Loop → Self-Improving) with per-level requirement checklists and an advancement plan to the next level.

![Maturity Model](./images/screen-14-maturity-model.png)

[📄 View PDF](./pdf/screen-14-maturity-model.pdf)

---

### Screen 15 — Five Drift Monitor
Governance view tracking all five drift types — Input, Ingestion (with live canary corpus delta), Model, Judge (with evaluator kappa health table), and Consumption (with a 21-day minimum window enforcement that blocks premature conclusions from novelty-period data).

![Five Drift Monitor](./images/screen-15-drift-monitor.png)

[📄 View PDF](./pdf/screen-15-drift-monitor.pdf)

---

### Screen 16 — Lifecycle Coverage Audit (T-11)
Test suite distribution audit comparing actual case type percentages against framework-required targets for the current lifecycle stage, with media blob registry and coverage gap report generation.

![Lifecycle Coverage Audit](./images/screen-16-coverage-audit.png)

[📄 View PDF](./pdf/screen-16-coverage-audit.pdf)

---

### Screen 17 — New Product Onboarding
Three-step product setup wizard (Identity → Pipeline Stage Labels → Evaluation Brief) that pre-populates default sub-contracts, judge roles, failure taxonomy, and audio-native eval track settings based on product category selection.

![New Product Onboarding](./images/screen-17-onboarding.png)

[📄 View PDF](./pdf/screen-17-onboarding.pdf)

---

## Using These Screens for Frontend Development

These design artifacts are intended for direct handoff to a frontend implementation. The recommended approach:

**1. Establish the design token file first.**
The complete token set (contract colors, evidence badge colors, stage colors, card elevation variants, base palette, typography, spacing) is defined in [`DESIGN_SPEC.md`](./DESIGN_SPEC.md). Implement this as a CSS/Tailwind config or design system foundation before touching any screen.

**2. Build the EvidenceBadge component before any screen.**
The `EvidenceBadge` ([SIM] / [EST] / [REV] / [INF] / [LIVE]) is the most novel primitive in the system. It appears on every numerical score throughout the platform. Implement it with its five color variants, hover tooltip, and error state (missing badge = parent card red border) as a prerequisite to all other work.

**3. Follow the data model.**
The core entities — `EvalProduct`, `EvalRun`, `ContractScore`, `Trace`, `Span`, `FailureMode`, `CausalChain`, `BacklogItem`, `DriverAnalysis`, `Judge`, `DriftEntry` — are fully typed in [`datamodels.md`](./datamodels.md). The frontend reads from these; the report assembles from them.

**4. Recommended screen build order.**
Build screens in dependency order: Mission Control → Five Contracts → Pipeline Inspector → Trace Explorer → Backlog → Driver Analysis → Causal Chains → Failure Mode Library → Product Evaluation Report → remaining functional screens.

**5. Recommended stack.**
React · Tailwind CSS · shadcn/ui (component primitives) · Recharts or D3 (sparklines, pipeline bar charts, causal chain diagrams) · JetBrains Mono (score and ID typography). Backend: Phoenix (Arize) or Opik (Comet) for trace storage and eval runner infrastructure — replace only the frontend layer.

**6. Architecture note.**
The platform is designed as a custom frontend over an open-source eval backend. The trace storage, SDK integrations, and eval runner infrastructure from Arize Phoenix or Comet Opik are retained as-is. The frontend is the proprietary layer — organized around the Five Quality Contracts, Nine-Stage Pipeline, and AMI Cycle rather than the generic abstractions of the underlying platform.

---

## Directory Structure

```
dashboards/
├── README.md                  ← this file
├── DESIGN_SPEC.md             ← full 17-screen design specification
├── datamodels.md              ← TypeScript entity definitions
├── images/
│   ├── screen-01-mission-control.png
│   ├── screen-02-evaluation-brief.png
│   └── ... (17 screens)
└── pdf/
    ├── screen-01-mission-control.pdf
    ├── screen-02-evaluation-brief.pdf
    └── ... (17 screens)
```

---



