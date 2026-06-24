# Guides

This directory contains the foundational documents of the Multimodal AI Evaluation Framework — a progressive learning and doing system designed to take any practitioner from first principles to a production-grade evaluation program.

---

## Reading Order

The first three guides are designed to be read in sequence. Each builds on the previous one. The fourth guide is a practitioner tool that can be used alongside Guide 2 or 3 — it is the translation layer between product requirements and evaluation specifications.

| Step | Guide | What You Get | Time |
| --- | --- | --- | --- |
| 1 | [101 Guide](Multimodal%20AI%20Product%20Evaluation%20%E2%80%94%20101%20Guide%20-%20Enhanced.pdf) | The mental model — concepts, failure modes, and why multimodal evaluation is different | 2–3 hrs read |
| 2 | [Getting Started Guide](Multimodal%20AI%20Product%20Evaluation%20-%20Getting%20Started.pdf) | The navigation layer — where to start, how to move through the framework, phase-by-phase steps | 1 hr read |
| 3 | [Field Manual](Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf) | The complete operational discipline — 20 chapters, all five contracts, every fix surface, full governance | Reference |
| 4 | [Multimodal AI Evals Speak](multimodal-ai-evals-requirements.md) | The translation layer — converting PRDs, feature requests, and stakeholder requirements into evaluation specifications using the DECODE framework | Practitioner tool |

---

## Guide 1 — 101 Guide

**File:** [`Multimodal AI Product Evaluation — 101 Guide - Enhanced.pdf`](Multimodal%20AI%20Product%20Evaluation%20%E2%80%94%20101%20Guide%20-%20Enhanced.pdf)

**Who it is for:** Anyone new to multimodal AI evaluation. No prior evaluation experience assumed.

**What it covers:**

- Why multimodal AI evaluation is harder than text-only evaluation — and why polished audio bypasses critical judgment in a way that text never does
- The nine-stage pipeline and where failures actually originate (~34% at Stage 1)
- The Five Quality Contracts and why collapsing them into one score hides failures
- The Five Gulfs — five fundamental gaps between what your team intends and what your system does
- The AMI Cycle: Analyze → Measure → Improve
- The hallucination detection toolkit: decompose-then-verify, NLI approach, key benchmarks
- Seven common misconceptions that consistently cause wasted months
- The evaluation tool ecosystem: when to reach for Galileo, Arize, Braintrust, LangSmith, Comet Opik, Patronus, Langfuse
- Where to start — whether you have a live product or are pre-launch

**Key insight from this guide:**
> *A multimodal AI product is a chain. Its strength is determined by its weakest link — and that weakest link is almost always invisible in the final output.*

---

## Guide 2 — Getting Started Guide

**File:** [`Multimodal AI Product Evaluation - Getting Started.pdf`](Multimodal%20AI%20Product%20Evaluation%20-%20Getting%20Started.pdf)

**Who it is for:** Practitioners ready to build or improve an evaluation program. Assumes the 101 Guide has been read, or that you have some evaluation background.

**What it covers:**

- **Who Are You? Start Here** — a navigation table matching your situation to your entry point, with estimated time to first value
- **The Mindset Before Everything Else** — four pre-commitments that determine whether your evaluation program produces signal or noise
- **The 12-Week Phase-by-Phase Plan** with completion criteria for each phase:
  * Phase 1 — Foundation (Weeks 1–2): Judge Panel, Evaluation Brief, Ten-Trace Exercise
  * Phase 2 — Error Analysis (Weeks 2–5): Traces Coding, Tagged Coding, Causal Chains
  * Phase 3 — Measurement Layer (Weeks 5–8): Human Labels, Evaluator Building, CI Gating
- **Adapting for your team size** — solo practitioner through enterprise context
- **Getting organizational buy-in** — three arguments that consistently work, plus the fastest path to a yes

**The four commitments introduced in this guide:**

| Commitment | The Core Discipline |
| --- | --- |
| Per-Modality, Per-Contract Thinking | Never ask "is this output good?" Ask five separate questions — one per contract |
| Pipeline-First Diagnosis | Trace every failure to its origin stage before proposing any intervention |
| Reading Real Traces | Plan for 60–80% of development time to go to error analysis |
| Pre-Committed Thresholds | Set every quality bar before any evaluation runs — not after |

---

## Guide 3 — Practitioner's Field Manual

**File:** [`Multimodal AI Product Evaluation - The Practioner's Field Manual - v2.1.pdf`](Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf)

**Who it is for:** Practitioners building or running a production multimodal AI evaluation program. The complete reference.

**What it covers — 20 chapters across 17 parts:**

| Part | Chapters | Focus |
| --- | --- | --- |
| I — Foundation | Ch 1–2 | Evaluation Mindset · Evaluation Brief |
| II — Mapping | Ch 3 | Surface Mapping · Nine-Stage Pipeline |
| III — Error Analysis | Ch 4 | Error Analysis Protocol · Taxonomy Building · Causal Chains |
| IV — Quality Contracts | Ch 5 | Per-Contract Quality Statements · Reference Thresholds |
| V — Architecture | Ch 6 | Cost–Latency–Quality–Fidelity Trade-offs · Cascade Pattern |
| VI — Instrumentation | Ch 7 | Logging for Evaluability · Four Logging Layers · Drop-off Timestamps |
| VII — Evaluators | Ch 8 | Evaluator-Building Protocol · Cohen's Kappa · Slice Analysis |
| VIII — Production | Ch 9–11 | Production Monitoring · CI Gating · Progressive Rollout Gates |
| IX — Diagnosis | Ch 12 | Driver Analysis · Five Pipeline Hypotheses |
| X — Fixing | Ch 13 | The Thirteen Fix Surfaces · Routing Quick Reference |
| XI — Decisions | Ch 14 | Decision Rubric · Decision Memo · Reversal Conditions |
| XII — Experiments | Ch 15 | Randomisation Unit · Novelty Effect · Pre-Registration |
| XIII — Data | Ch 16 | Dataset Lifecycle · Leakage Vectors · Media Blob Management |
| XIV — Prioritization | Ch 17 | Six-Dimension Priority Formula · Blast Radius Worksheet |
| XV — Governance | Ch 18 | RACI · Prompt Review Board · Cadence Calendar · Debt Register |
| XVI — Tooling | Ch 19 | Build vs. Buy · Scenario-Based Tool Selection |
| XVII — Quarter 1 Plan | Ch 20 | 12-Week Action Plan · Quarter 2 and 3 Outlook |

**Appendices:**

| Appendix | Contents |
| --- | --- |
| A | Template Pack Quick Reference (T-01 through T-18) |
| B | Ten Starter Kit Artifacts with time estimates |
| C | Multimodal Failure Mode Library (13 named patterns) |
| D | Red Flags Checklist (run quarterly — 40+ checks) |
| E | The Five Gulfs and where each is bridged in the manual |
| F | Evaluation Maturity Model (Levels 1–5) |
| G | Benchmark Reference Map (MMDocBench, OCRBench, ChartQA, PodEval, and more) |
| H | Glossary of 30+ key terms in plain English |

---

## Guide 4 — Multimodal AI Evals Speak

**File:** [`multimodal-ai-evals-speak.md`](multimodal-ai-evals-requirements.md)

**Who it is for:** AI Product Managers, AI Solutions Architects, ML Engineers, QA and Eval Engineers, and Team Leads who receive product requirements and need a systematic method for converting them into executable evaluation specifications. Assumes basic familiarity with the Five Quality Contracts (from the 101 Guide) but can be used independently.

**The problem it solves:** Most evaluation guides teach how to *build* evals. This guide teaches how to *derive* what to evaluate in the first place — from the language of PRDs, feature requests, OKRs, and stakeholder statements. It is the upstream discipline that feeds T-01 through T-18.

**What it covers:**

- The Translation Problem — why specification debt causes more evaluation program failures than tool or infrastructure gaps
- The Two Languages — PRD Speak (language of intent) vs. Evals Speak (language of contracts), and the Decision Language that connects them to stakeholders
- Why multimodal systems make requirements translation harder — five compounding reasons including modality coupling, the explosion of failure surface, and the demo trap
- The Translation Stack — eight conceptual layers from stakeholder intent to executed eval specification
- **The DECODE methodology** — six steps for translating any requirement into a formal eval spec:
  - **D**ecompose requirements into atomic capability claims
  - **E**numerate modalities and input slices
  - **C**lassify by pipeline stage, quality contract, and failure mode
  - **O**perationalize each failure mode as a measurement primitive
  - **D**efine thresholds, gates, datasets, and monitoring
  - **E**xpress as a formal eval specification
- The Multimodal Eval Dimensions Reference — a lookup table of 20+ named dimensions across ingestion, retrieval, generation, and delivery layers
- The Five Quality Contracts as Eval Anchors — PRD phrases that trigger each contract, with the note that Safety is a mandatory governance overlay across all five
- Six Stakeholder Requirement Archetypes — Executive Vision, Feature Requirement, Safety/Compliance, Performance, User Experience, and Multimodal Consistency — with translation paths for each
- Vocabulary Mapping Dictionary — 15 compressed stakeholder phrases decoded into their eval primitives
- The Multimodal Eval Specification Template — a 15-section formal spec document
- Five worked examples across document Q&A, voice-to-structured-data, multimodal customer support, lease review, and screen-aware voice assistant
- Modality-specific eval considerations for image, audio, video, document, and cross-modal interactions
- The Eval Coverage Matrix — a per-contract × per-modality audit grid
- Eight anti-patterns, each with a named tell, a failure explanation, and a correction
- Monday Morning Actions by practitioner situation
- Five closing principles
- **Appendix: Populating AI Product PRDs with Eval Specifications** — including the Eval Stub, PRD Eval Requirements Section, and Concurrent Authoring Protocol

**Companion templates for this guide:**

| Template | Purpose |
| --- | --- |
| [T-19 Eval Stub Template](../templates/T-19-eval-stub-template.md) | 7-section PM-authored artifact that lives in the PRD; seeds the full Eval Spec |
| [T-20 PRD Eval Requirements Section](../templates/T-20-prd-eval-requirements-section.md) | Container section embedded in every AI feature PRD |
| [T-21 Multimodal Eval Specification Template](../templates/T-21-multimodal-eval-spec-template.md) | Full 15-section eval contract authored by the eval engineer |
| [T-22 DECODE Translation Worksheet](../templates/T-22-decode-translation-worksheet.md) | Guided 90-minute session worksheet for PM + eval engineer |

**Key insight from this guide:**
> *Every stakeholder sentence is a compressed claim. The translator's job is to refuse that compression — politely and repeatedly — until every quality dimension is named, every modality is inventoried, and every failure mode is concrete enough for a reviewer to score from input and output alone.*

---

## Quick Navigation by Situation

| You are... | Go here |
| --- | --- |
| Encountering multimodal AI evaluation for the first time | [101 Guide](Multimodal%20AI%20Product%20Evaluation%20%E2%80%94%20101%20Guide%20-%20Enhanced.pdf) — Part 1 |
| Confused about why one accuracy score is not enough | [101 Guide](Multimodal%20AI%20Product%20Evaluation%20%E2%80%94%20101%20Guide%20-%20Enhanced.pdf) — Part 5: The Four Contracts |
| Trying to understand where failures actually come from | [101 Guide](Multimodal%20AI%20Product%20Evaluation%20%E2%80%94%20101%20Guide%20-%20Enhanced.pdf) — Part 3: The Pipeline |
| Ready to build an evaluation program | [Getting Started Guide](Multimodal%20AI%20Product%20Evaluation%20-%20Getting%20Started.pdf) — Phase 1 |
| Mid-program and need to build evaluators | [Getting Started Guide](Multimodal%20AI%20Product%20Evaluation%20-%20Getting%20Started.pdf) — Phase 3 |
| Running a production alert investigation | [Field Manual](Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf) — Chapter 12: Driver Analysis |
| Making a ship/hold/rollback decision | [Field Manual](Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf) — Chapter 14: Decision-Making |
| Planning a quarterly governance review | [Field Manual](Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf) — Chapter 18: Governance |
| Looking for the right tool for your situation | [Field Manual](Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf) — Chapter 19: Build vs. Buy |
| Starting from zero with 12 weeks ahead | [Field Manual](Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf) — Chapter 20: The 12-Week Plan |
| Translating a PRD or feature request into an eval spec | [Multimodal AI Evals Speak](multimodal-ai-evals-speak.md) — Section 6: DECODE Methodology |
| Running a PRD authoring session with a PM and eval engineer | [T-22 DECODE Translation Worksheet](../templates/T-22-decode-translation-worksheet.md) — print or open before the session |
| Embedding eval requirements into a PRD right now (solo PM) | [T-19 Eval Stub Template](../templates/T-19-eval-stub-template.md) — Sections 1–3 minimum for Draft stage |
| Writing a full evaluation specification for a feature | [T-21 Multimodal Eval Specification Template](../templates/T-21-multimodal-eval-spec-template.md) — start from the completed Eval Stub |
| Decoding a stakeholder phrase into evaluation primitives | [Multimodal AI Evals Speak](multimodal-ai-evals-speak.md) — Section 10: Vocabulary Mapping Dictionary |
| Auditing whether an eval spec covers all modalities and contracts | [Multimodal AI Evals Speak](multimodal-ai-evals-speak.md) — Section 14: Eval Coverage Matrix |

---

## How the Guides Connect to the Templates

Every chapter in the Field Manual that has an associated template includes a reference callout. Guide 4 (Multimodal AI Evals Speak) has four dedicated companion templates. All templates live in [`/templates`](../templates).

### Field Manual Templates (T-01 – T-18)

| Guide section | Template |
| --- | --- |
| Getting Started Phase 1 — Evaluation Brief | [T-01](../templates/T-01-evaluation-brief.md) |
| Field Manual Ch 5 — Quality Statement | [T-02](../templates/T-02-quality-statement.md) |
| Getting Started Phase 1 — Architecture | [T-03](../templates/T-03-architecture-worksheet.md) |
| Getting Started Phase 1 — Logging | [T-04](../templates/T-04-logging-schema.md) |
| Getting Started Phase 3 — Evaluator Prompts | [T-05](../templates/T-05-ingestion-fidelity-evaluator.md) through [T-09](../templates/T-09-cross-modal-consistency-evaluator.md) |
| Field Manual Ch 9 — Monitoring | [T-10](../templates/T-10-gate-thresholds-alert-rules.md) |
| Getting Started Phase 3 — CI Gating | [T-11](../templates/T-11-regression-suite-coverage-audit.md) · [T-12](../templates/T-12-ci-gate-policy.md) |
| Field Manual Ch 12 — Driver Analysis | [T-13](../templates/T-13-driver-analysis-template.md) |
| Field Manual Ch 14 — Decisions | [T-14](../templates/T-14-decision-memo.md) |
| Field Manual Ch 15 — Experiments | [T-15](../templates/T-15-experiment-pre-registration.md) |
| Field Manual Ch 16 — Datasets | [T-16](../templates/T-16-dataset-record.md) |
| Field Manual Ch 17 — Prioritization | [T-17](../templates/T-17-backlog-prioritization-table.md) |
| Field Manual Ch 18 — Governance | [T-18](../templates/T-18-evaluation-debt-register.md) |

### Multimodal AI Evals Speak Templates (T-19 – T-22)

| Guide section | Template | Who authors it | When |
| --- | --- | --- | --- |
| Section 20.1 — The Eval Stub | [T-19](../templates/T-19-eval-stub-template.md) | PM | Same session as feature requirement |
| Section 20.2 — PRD Eval Requirements Section | [T-20](../templates/T-20-prd-eval-requirements-section.md) | PM | Embedded in every AI feature PRD |
| Section 11 — Eval Specification Template | [T-21](../templates/T-21-multimodal-eval-spec-template.md) | Eval Engineer | After PRD Approved; before development starts |
| Section 20.3 — Concurrent Authoring Protocol | [T-22](../templates/T-22-decode-translation-worksheet.md) | PM + Eval Engineer | 90-minute Draft PRD session |

---

*Read the 101 Guide to understand the map. Read the Getting Started Guide to plan your route. Use the Field Manual to walk it. Use Multimodal AI Evals Speak to translate what you're building into what you're evaluating.*
