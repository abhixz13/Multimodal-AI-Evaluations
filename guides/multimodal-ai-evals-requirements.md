# Multimodal AI Evals Speak
## Translating AI Product Requirements into Multimodal AI Evaluation Specifications

> A practitioner's field guide for converting PRDs, feature requests, stakeholder asks, and "make it good" requirements into rigorous, executable multimodal evaluation specifications.

---

## Table of Contents

1. [Purpose and Scope](#1-purpose-and-scope)
2. [The Translation Problem](#2-the-translation-problem)
3. [The Two Languages: PRD Speak vs. Evals Speak](#3-the-two-languages-prd-speak-vs-evals-speak)
4. [Why Multimodal Makes Translation Harder](#4-why-multimodal-makes-translation-harder)
5. [The Translation Stack: From Requirement to Eval Spec](#5-the-translation-stack-from-requirement-to-eval-spec)
6. [The DECODE Translation Methodology](#6-the-decode-translation-methodology)
7. [Multimodal Eval Dimensions Reference](#7-multimodal-eval-dimensions-reference)
8. [The Five Quality Contracts as Eval Anchors](#8-the-five-quality-contracts-as-eval-anchors)
9. [Stakeholder Requirement Archetypes and Eval Translations](#9-stakeholder-requirement-archetypes-and-eval-translations)
10. [Vocabulary Mapping Dictionary](#10-vocabulary-mapping-dictionary)
11. [The Multimodal Eval Specification Template](#11-the-multimodal-eval-specification-template)
12. [Worked Examples](#12-worked-examples)
13. [Modality-Specific Eval Considerations](#13-modality-specific-eval-considerations)
14. [Eval Coverage Matrix](#14-eval-coverage-matrix)
15. [Eval Specification Review Checklist](#15-eval-specification-review-checklist)
16. [Anti-Patterns and Failure Modes](#16-anti-patterns-and-failure-modes)
17. [Monday Morning Actions](#17-monday-morning-actions)
18. [Closing Principles](#18-closing-principles)
19. [Glossary](#19-glossary)
20. [Appendix: Populating AI Product PRDs with Eval Specifications](#20-appendix-populating-ai-product-prds-with-eval-specifications)

---

## 1. Purpose and Scope

### What This Document Is

Every multimodal AI product begins life as a sentence in someone else's language. A PM writes *"the assistant should accurately understand what the user shows it."* A designer writes *"responses should feel grounded in the screen."* A founder writes *"don't hallucinate."* An enterprise customer writes *"it must work for compliance use cases."*

None of these sentences are evaluation requirements. They are wishes, intuitions, and proxies for wishes. They describe a destination, not a measurement. And the gap between them and a real evaluation suite is wide enough that most teams either skip the work entirely — shipping on vibes — or build evals that measure the wrong thing precisely.

This document is the manual for the translation discipline that closes that gap, specialized for multimodal AI systems where the input space includes images, audio, video, documents, screen captures, and arbitrary cross-modal combinations.

It answers the question that most evaluation guides skip: *how do you get from what a stakeholder or product team says they want to a formal, executable, measurable evaluation plan?*

It is designed for:

- **AI Product Managers** developing evaluation plans for multimodal AI features
- **AI Solutions Architects** defining quality contracts for multimodal pipelines
- **ML Engineers and Prompt Engineers** who need evaluation-ready acceptance criteria
- **QA and Eval Engineers** who receive vague product requirements and need a systematic method to derive test specifications
- **Team Leads and Engineering Managers** who need to scope and resource evaluation work

### What This Document Is Not

This document does not teach how to build evaluation infrastructure — datasets, scoring pipelines, LLM-as-Judge configurations. Those topics are covered in the companion documents in this repository. This document operates one layer upstream: the discipline of *requirements translation*.

### Scope of Multimodal Coverage

This framework covers evaluation requirements for systems that process or produce content across any combination of:

- **Text** — natural language input, output, summaries, answers, narratives
- **Image** — photographs, diagrams, charts, UI screenshots, scanned documents
- **Audio** — speech, voice commands, ambient sound, music, audio narration
- **Video** — recorded video, screen recordings, video with embedded audio tracks
- **Document** — PDFs, structured documents, slides, mixed-content files
- **Structured Data** — tables, JSON, database exports embedded within multimodal inputs

---

## 2. The Translation Problem

### Why Translation Fails

Multimodal AI evaluation programs fail not because teams cannot build eval infrastructure, but because they begin with the wrong inputs. The root failure is **specification debt**: evaluation criteria derived after the fact from complaints, regressions, and launch failures rather than from product requirements established before development begins.

The four most common failure modes:

**Failure Mode 1 — Metric Without Meaning.**
A team ships with "0.87 faithfulness" as the quality signal. No stakeholder agreed to 0.87 as a threshold. No one defined which failure cases faithfulness was measuring. The number exists, but it governs nothing.

**Failure Mode 2 — Modality Blindspot.**
A team evaluates the text output of a multimodal system comprehensively but never evaluates whether the visual content referenced in that output was accurately described, correctly identified, or appropriately grounded. The system hallucinates about what is in an image; no eval catches it.

**Failure Mode 3 — Requirement Drift.**
Product requirements change during development. Eval specifications, written once and never revisited, measure the original product, not the shipped product. The launch gate passes a system that no longer matches the requirement.

**Failure Mode 4 — Stakeholder Disconnection.**
Eval results are presented in technical language. Stakeholders cannot interpret them. Launch decisions are made on instinct. The eval investment produces no governance value.

### The Translation Discipline

The discipline of translating product requirements into eval specifications is **Multimodal Evals Speak**. It is the structured process of:

1. Reading a product requirement, feature request, or stakeholder statement
2. Identifying the underlying quality claims embedded in that requirement
3. Decomposing those claims into testable, measurable dimensions
4. Assigning each dimension to the appropriate modality, pipeline stage, and quality contract
5. Enumerating concrete, observable failure modes for each dimension
6. Specifying the measurement method, judge, dataset, threshold, and gate level
7. Producing a formal Eval Specification that an engineering team can execute and a stakeholder can read

Translation is not transcription. The output is not the input restated. The output is a structured artifact, in a different language, with explicit assumptions, scoped failure modes, named judges, and defensible thresholds. This process closes the gap between what a product says it does and what evaluation evidence can prove.

---

## 3. The Two Languages: PRD Speak vs. Evals Speak

Treat these as genuinely different languages, not different dialects. Words that look identical mean different things in each. Confusing them is the most common failure mode in multimodal eval programs.

**PRD Speak** is the language of intent. It is forward-looking, holistic, and aspirational. It talks about what the user should feel, what the product should be capable of, and what success looks like at a market or business level. It is appropriate for alignment, prioritization, and roadmap conversations. It is *useless* as a measurement instrument because it bundles dozens of latent quality dimensions into single phrases.

**Evals Speak** is the language of contracts. It is backward-looking, decompositional, and operational. It talks about what specifically broke, where in the pipeline it broke, against which slice of inputs, scored by which judge, against which threshold, with what variance. It is appropriate for shipping decisions, regression detection, and capability claims. It is *useless* for inspiring a team because it has no story arc.

Both languages are necessary. The translation discipline lives in the seam between them.

| Dimension | PRD Speak | Evals Speak |
|---|---|---|
| Time orientation | Future state | Observed behavior |
| Granularity | Holistic capability | Pipeline-stage failure mode |
| Truth conditions | "Users will love this" | "Score ≥ 0.85 on slice X" |
| Unit of analysis | Feature | Trace / span / output |
| Owner | PM, Design, Founder | Eval engineer, Applied AI |
| Failure response | Iterate the brief | Diagnose the gap |
| Output artifact | PRD, brief, roadmap | Spec, suite, dashboard |

A third register — **Decision Language** — synthesizes eval evidence back into stakeholder-ready verdicts, trade-off summaries, and action recommendations. It is the language of launch reviews, risk committees, and board updates: verdict-first, quantified-risk framing, action-oriented.

> "Core catalog queries are launch-ready. Structured extraction breaks on handwritten invoice fields. Recommend phased launch with document type restriction."

**The AI PM's job is to translate fluently between all three registers — in both directions.**

---

## 4. Why Multimodal Makes Translation Harder

Single-modality eval translation is already hard. Multimodal translation is harder for five compounding reasons.

**First — Modality coupling.** When a user shows the system a screenshot and asks a question, "correct" requires the system to ingest the image faithfully, perceive its content accurately, ground its response in what was perceived, and produce an answer consistent with both the image and the world. A failure can originate at any stage, and the symptom — a wrong answer — looks identical regardless of origin. PRD speak collapses all of this into "the answer was wrong." Evals speak must keep them separate.

**Second — Explosion of failure surface.** A pure text system has a roughly linear input space. A multimodal system has a combinatorial one: image quality × image content type × audio quality × audio content × text framing × user intent × cross-modal alignment. A PRD that says "works for documents" is silently making claims about dozens of latent slices.

**Third — Judge availability.** Many multimodal quality dimensions cannot be cheaply judged by an LLM and cannot be cheaply judged by humans either. Audio transcription faithfulness, image-text grounding, video temporal consistency — each requires specialized models, structured human protocols, or both. The translation must specify *which judge* scores each contract and budget honestly for the expensive ones.

**Fourth — Reference data scarcity.** Text evals often have ground truth. Multimodal evals frequently do not. A document Q&A system may have no labeled "correct extraction" for the long tail of customer documents. The translation must specify whether the eval is reference-based, reference-free, or hybrid — and what that implies about signal strength.

**Fifth — The demo trap.** Multimodal systems demo extraordinarily well on cherry-picked inputs and fail in mundane ways on the real input distribution. PRD speak almost always describes the cherry-picked input ("imagine a user shows the assistant their tax form"). Evals speak must aggressively sample from the actual or expected input distribution — including degraded, noisy, partial, and adversarial inputs.

The translation discipline exists to neutralize all five.

---

## 5. The Translation Stack: From Requirement to Eval Spec

Treat translation as a layered transformation. Each layer takes a less precise input and produces a more precise output. Each layer has explicit deliverables. No layer is skipped.

```
Layer 0:  STAKEHOLDER INTENT
          "Users should be able to trust the assistant's reading of their documents."
                ↓
Layer 1:  DECOMPOSED CAPABILITY CLAIMS
          ["ingests scanned PDFs", "extracts table structure", "answers grounded questions", ...]
                ↓
Layer 2:  PIPELINE-STAGE ASSIGNMENT
          Each claim mapped to one or more of the Nine Multimodal Pipeline Stages
                ↓
Layer 3:  QUALITY CONTRACT ASSIGNMENT
          Each claim mapped to one or more of the Five Quality Contracts
                ↓
Layer 4:  FAILURE MODE ENUMERATION
          For each contract: concrete, observable failure modes with examples
                ↓
Layer 5:  MEASUREMENT PRIMITIVES
          For each failure mode: metric, judge, dataset, threshold, variance budget
                ↓
Layer 6:  SLICED EVAL SUITE
          Failure modes × input slices = the actual eval grid
                ↓
Layer 7:  GATES AND SLOs
          Which thresholds gate ship? Which gate experiments? Which are tracked?
                ↓
Layer 8:  EVAL SPECIFICATION ARTIFACT
          A single versioned document that captures all of the above and can be executed
```

Each downward arrow is where translation work happens. Each layer below is the *answer* to a question asked at the layer above.

> **Note on the relationship between the Translation Stack and DECODE:** The eight layers above are a conceptual model showing every transformation stage from stakeholder intent to executed eval. The six-step **DECODE methodology** in Section 6 is the practitioner's implementation of this stack — Steps D, E, and C each consolidate multiple conceptual layers, while Steps O, D, and E map cleanly to Layers 5, 7, and 8 respectively. Read the Translation Stack to understand the full transformation; use DECODE to execute it.

---

## 6. The DECODE Translation Methodology

The structured translation process follows exactly **six steps** under the **DECODE** framework — one step per letter, no exceptions:

| Letter | Step | Core Question |
|---|---|---|
| **D** | Decompose | What are the atomic claims in this requirement? |
| **E** | Enumerate | Which modalities and input slices are involved? |
| **C** | Classify | Which pipeline stages, quality contracts, and failure modes apply? |
| **O** | Operationalize | How will each failure mode be measured and judged? |
| **D** | Define | What thresholds, gates, and datasets govern this eval? |
| **E** | Express | What is the formal, versioned eval specification artifact? |

Walk through every step for every new feature, capability, or product surface. Skipping steps does not save time; it defers the work to a worse moment.

---

### Step D — Decompose the Requirement into Atomic Capability Claims

Take the original requirement and shatter it into the smallest atomic capability claims you can defend. The test for atomicity: each claim should be answerable as "the system did / did not do X" by examining a single output. If a claim still requires interpretation, decompose further.

**Example requirement:**
> "Product managers should be able to upload a competitive intelligence report (PDF) and ask natural language questions about it. Answers should be accurate and grounded in the document."

**Decomposed atomic capability claims:**

| # | Atomic Claim | Source Phrase |
|---|---|---|
| 1 | The system correctly extracts all pages, including tables and embedded charts | "upload...PDF" |
| 2 | Answers contain only claims supported by the document | "grounded in the document" |
| 3 | Answers are factually correct with respect to document content | "accurate" |
| 4 | The system retrieves the correct source passage before generating an answer | implied by RAG architecture |
| 5 | Answers do not fabricate sources or misattribute claims | implied by grounding |
| 6 | When a question cannot be answered from the document, the system says so | implied by "grounded" |

A requirement like *"the assistant should help users review contracts"* might similarly decompose into seven or more atomic claims. The decomposition conversation with the PRD owner will surface 30% of assumptions the team did not know were there. That is half the value of this step.

**Deliverable:** A numbered list of atomic capability claims confirmed by the PRD owner.

---

### Step E — Enumerate Modalities and Input Slices

Identify every modality present in the input pipeline, the output pipeline, and intermediate processing stages. Then enumerate the dimensions along which inputs vary — this becomes the slice grid that determines what the eval suite actually covers.

**Part 1 — Modality Inventory:**
- [ ] Text (typed, OCR-extracted, transcribed)
- [ ] Image (photos, diagrams, charts embedded in documents)
- [ ] Audio (voice input, audio tracks in uploaded media)
- [ ] Video (uploaded recordings, screen captures)
- [ ] Structured data (tables, forms, JSON payloads)
- [ ] Document (PDF, DOCX, PPTX as primary input type)
- [ ] Cross-modal (outputs that reference or synthesize across modalities)

For each modality checked: a separate eval dimension is required. A claim that implicates four or more pipeline stages is usually still under-decomposed — return it to Step D.

**Part 2 — Input Slice Grid:**

The eval suite is failure modes *crossed with* input slices. The slicing dimensions are where multimodal eval design earns its keep and where the demo trap is neutralized.

Define slices along at least:

- **Modality presence** — text only, image only, audio only, text+image, text+audio, full multimodal
- **Input quality** — pristine, lightly degraded, heavily degraded, partial, adversarial
- **Content domain** — the actual user-segment domains the feature must serve
- **User intent** — informational, transactional, exploratory, compliance, adversarial
- **Length / duration** — short, medium, long inputs and outputs
- **Language / locale** — if applicable
- **Sensitive subgroups** — accessibility, demographic, regulatory

The slice grid is intentionally larger than the eval budget. The translation deliberately decides which cells to populate, which to sample, and which to defer — and *documents the deferrals*. A deferred slice is a known unknown. An unnoticed slice is an unknown unknown, and unknown unknowns are how multimodal systems embarrass themselves in production.

**Deliverable:** Completed modality inventory with at least one planned eval dimension per active modality, plus a populated slice grid showing planned coverage, sampling strategy, and explicit deferrals.

---

### Step C — Classify by Pipeline Stage, Quality Contract, and Failure Mode

This is the classification step. Every atomic claim gets assigned to where it lives in the pipeline, which quality contract it belongs to, and what concrete failure modes it can produce. All three sub-tasks belong here because they are the same act of structured analysis — moving from a claim to its evaluable form.

**Part 1 — Pipeline Stage Assignment:**

Assign each claim to its location(s) in the nine-stage multimodal pipeline. Naming the stage is what makes root-cause diagnosis possible when a failure occurs.

| Stage | Pipeline Component | Eval Focus |
|---|---|---|
| 1 | Input Ingestion | Parsing fidelity, format coverage, encoding handling |
| 2 | Modality Detection | Correct identification of content type per segment |
| 3 | Preprocessing | Normalization quality, OCR accuracy, transcription accuracy |
| 4 | Chunking & Segmentation | Chunk boundary quality, semantic coherence of splits |
| 5 | Embedding & Indexing | Retrieval recall, embedding coverage of key content |
| 6 | Retrieval | Precision@k, source relevance, cross-modal retrieval accuracy |
| 7 | Generation | Faithfulness, factuality, instruction-following, tone, length |
| 8 | Output Formatting | Format compliance, citation accuracy, structured field completeness |
| 9 | Delivery & Observability | Latency, cost, trace completeness, anomaly detection |

**Part 2 — Quality Contract Assignment:**

Each atomic claim must land on at least one of the Five Quality Contracts (detailed in Section 8). If a claim cannot be assigned, it is either out of scope, under-decomposed, or wishful thinking — all three are useful findings.

**Part 3 — Failure Mode Enumeration:**

For each (claim, contract) pair, enumerate the specific, observable ways it can fail. Be concrete — "hallucinates" is a category, not a failure mode. Concrete failure modes:

- Fabricates a clause number not present in the source document
- Reports a monetary amount with the wrong currency unit
- Describes an on-screen element that does not exist in the image
- Answers confidently about an illegible region without flagging the illegibility
- Drops a segment with no speech but high visual content from a video summary

**Test for a well-specified failure mode:** A competent human reviewer, given only the input and the output, can determine whether the failure occurred without consulting the original team. If they cannot, the failure mode is too abstract.

Deliberately enumerate failure modes that cross modalities. Cross-modal failure modes are the most-missed and the most-costly. The worked examples in Section 12 demonstrate this in detail.

**Deliverable:** A claim-to-stage matrix, a claim-to-contract matrix, and a failure mode register organized by contract with brief examples.

---

### Step O — Operationalize Each Failure Mode as a Measurement Primitive

For each failure mode from Step C, specify five things:

1. **Metric** — What numerical or categorical score will be produced? (Accuracy, F1, Likert, pass/fail, structured rubric score, ranked preference, etc.)
2. **Judge** — Who or what produces the score? (Human judge with rubric, LLM-as-Judge with rubric, deterministic check, specialized model, hybrid panel.)
3. **Dataset** — Against what inputs is the metric computed? Name its source, size, slices, and refresh policy.
4. **Threshold** — What value counts as passing for this failure mode, for this release?
5. **Variance budget** — How much score noise is tolerable and how is it estimated? (Inter-rater agreement for humans, judge stability for LLMs, bootstrapped CIs for deterministic checks.)

**Operationalization template:**

```
Metric Name: [Name]
Definition: [Precise definition of what is being measured]
Measurement Method: [Automated / LLM-as-Judge / Human Annotator / Hybrid]
Judge: [Named judge with rubric version — never anonymous]
Scoring Range: [0–1 / 0–100 / Boolean / Categorical]
Dataset: [Source, size, slices, refresh policy]
Ground Truth Required: [Yes / No — what form]
Launch Threshold: [Minimum score required to ship]
Regression Threshold: [Score below which production alert fires]
Failure Consequence: [What breaks for user or business]
Priority: [P0 — blocking / P1 — high / P2 — tracked]
```

**Critical judge principle:** Name the judge. Never anonymize it. "LLM-as-judge" is not a judge specification. "Claude Sonnet 4 with rubric v2.3, temperature 0, run three times with majority vote" is. Use the Judge Panel architecture — different failure modes need different judges. Do not use an LLM judge for a failure mode that LLMs systematically share with the system under test. Phantom visual references and modality hallucinations are exactly such cases. Humans or specialized detection models must be on the panel.

**Deliverable:** A measurement primitive table per failure mode.

---

### Step D — Define Thresholds, Gates, Datasets, and Monitoring

With failure modes operationalized, formally define the governance layer: what thresholds trigger what actions, who owns what, and how the system is monitored in production.

**Part 1 — Gate Classification:**

Not every threshold is a gate. Distinguish three levels:

- **Ship gates** — Hard thresholds; failure blocks release. Should be few and clearly tied to user-visible harm or capability claims the company is making publicly.
- **Experiment gates** — Thresholds that block merging a change into the main candidate. Higher noise tolerance; designed to catch regressions during iteration.
- **Tracked metrics** — Observed but not gated. Used for trend awareness, debugging, and surfacing emerging issues. Many failure modes start here and graduate to experiment gates as the system matures.

Tie each gate to an owner, a review cadence, and a deprecation policy. Gates that nobody owns become gates that nobody enforces — which is worse than no gate, because they create false comfort.

**Part 2 — Dataset and Ground Truth Specification:**

For each metric defined in Step O, confirm the dataset source, size, refresh policy, and contamination policy. Identify any dimensions requiring new annotation and estimate the annotation timeline.

**Part 3 — Production Monitoring Specification:**

Define the alert thresholds, monitoring cadence, alert owners, and response SLAs that govern the system after launch. An eval suite with no production monitoring is a launch-gate-only instrument — it cannot detect drift.

**Deliverable:** A gate register (failure mode → metric → threshold → gate level → owner → review cadence), a dataset manifest, and a production monitoring specification.

---

### Step E — Express as a Formal Eval Specification

Compile all outputs of the preceding five steps into the standardized Eval Spec format in Section 11. The specification is the durable artifact between product and evaluation. Both sides sign off. Both sides own it. Changes are PR'd, reviewed, and tracked.

This is not a summary document. It is the authoritative record of what the eval program covers, why, and how. A spec that describes a product surface that no longer exists is worse than no spec, because it creates false assurance.

**Deliverable:** A complete, versioned multimodal eval specification document.

---

## 7. Multimodal Eval Dimensions Reference

Use this as a lookup when decomposing requirements into measurement primitives.

### 7.1 Ingestion-Layer Dimensions

| Dimension | Definition | Applies To | Typical Metric |
|---|---|---|---|
| **Parse Fidelity** | Does the system correctly extract content from the source format? | All modalities | Character-level accuracy, field extraction F1 |
| **OCR Accuracy** | For image-embedded text: how accurately is it transcribed? | Image, Document | Character error rate (CER), word error rate (WER) |
| **Audio Transcription Accuracy** | For speech input: how accurately is it transcribed? | Audio, Video | Word error rate (WER), speaker diarization accuracy |
| **Layout Preservation** | Are tables, lists, headers, and visual structure retained? | Document, Image | Layout structure F1 |
| **Multimodal Segment Detection** | Are different modality regions correctly identified and separated? | Document, Video | Segment classification accuracy |
| **Format Coverage** | What percentage of expected input formats are correctly ingested? | All | Coverage rate across format inventory |

### 7.2 Retrieval-Layer Dimensions

| Dimension | Definition | Applies To | Typical Metric |
|---|---|---|---|
| **Retrieval Recall** | Does the retrieval stage surface all relevant source passages? | All RAG-based | Recall@k |
| **Retrieval Precision** | Are retrieved passages relevant, or is noise introduced? | All RAG-based | Precision@k, MRR |
| **Cross-Modal Retrieval** | Can the system retrieve image content from a text query, and vice versa? | Image+Text, Document | Cross-modal recall@k |
| **Source Attribution Accuracy** | Does the system correctly identify which source document a passage came from? | Multi-document | Attribution accuracy |
| **Temporal Retrieval Accuracy** | For time-sensitive content: does the system retrieve the most current source? | Document, Video | Recency-weighted recall |

### 7.3 Generation-Layer Dimensions

| Dimension | Definition | Applies To | Typical Metric |
|---|---|---|---|
| **Faithfulness** | Does the output contain only claims supported by the retrieved context? | All | Faithfulness score (NLI-based or LLM-Judge) |
| **Factuality** | Are all factual claims true with respect to a ground-truth source? | All | Factuality score, claim-level precision |
| **Hallucination Rate** | What proportion of outputs contain at least one unsupported claim? | All | Hallucination rate (per-output and per-claim) |
| **Instruction Following** | Does the output comply with format, length, tone, and task instructions? | All | Instruction adherence score |
| **Visual Grounding** | When describing an image: are claims grounded in visible content? | Image, Video, Document | Visual grounding score |
| **Audio Content Accuracy** | When answering about audio: are claims grounded in the audio track? | Audio, Video | Audio grounding score |
| **Cross-Modal Consistency** | Does the output treat the same entity consistently across modalities? | All multimodal | Cross-modal consistency score |
| **Abstention Accuracy** | When the system should say "I don't know," does it? | All | Abstention precision and recall |
| **Safety Compliance** | Does the output avoid harmful, biased, or policy-violating content? | All | Safety pass rate, category-level failure rate |

### 7.4 Output and Delivery Dimensions

| Dimension | Definition | Applies To | Typical Metric |
|---|---|---|---|
| **Output Quality** | Overall quality of the final response as assessed by a human or judge panel | All | MOS, ROUGE/BLEU, LLM-Judge |
| **Format Compliance** | Does the structured output match the required schema? | Structured output | Schema validation pass rate |
| **Citation Quality** | Are citations accurate, complete, and correctly formatted? | Document-grounded | Citation accuracy, coverage |
| **Latency** | Time from input submission to output delivery | All | p50, p95, p99 latency |
| **Cost per Successful Task** | Total inference cost for tasks that meet quality threshold | All | Cost/task (passing) |
| **Escalation Accuracy** | When the system should escalate to human review, does it? | Agent systems | Escalation precision, recall, F1 |

---

## 8. The Five Quality Contracts as Eval Anchors

The Five Quality Contracts provide the organizing architecture for multimodal eval specifications. Every product requirement, regardless of how it is stated, maps to one or more of these contracts. Every atomic claim must land on at least one. If a claim cannot be assigned, it is under-decomposed or out of scope.

### Contract 1 — Ingestion Fidelity

**The promise:** The system correctly ingests and represents source content — text, images, audio, video, and documents — without loss, distortion, or misattribution.

**What breaks when this fails:** Every downstream evaluation is invalid. A system that misreads an invoice cannot faithfully summarize it. Ingestion Fidelity is the foundation; all other contracts depend on it.

**PRD phrases that trigger this contract:**
> "The system should handle PDFs, Word documents, and scanned images."  
> "Users can upload audio recordings and receive transcripts."  
> "The system ingests our product catalog in CSV and JSON formats."

**Eval specification trigger:** Any mention of input types, upload capabilities, file format support, parsing, or OCR.

---

### Contract 2 — Faithfulness

**The promise:** The system's outputs contain only claims supported by the source material it was given. It does not add, invent, or embellish.

**What breaks when this fails:** User trust. In regulated industries, faithfulness failures are compliance failures.

**PRD phrases that trigger this contract:**
> "The AI should answer based on our documentation, not general knowledge."  
> "Summaries must stay within the content of the uploaded report."  
> "The assistant must not make claims not supported by the policy document."

**Eval specification trigger:** Any mention of retrieval-augmented generation, document-grounded answers, "only use our data," or summarization.

---

### Contract 3 — Factuality

**The promise:** All factual claims in the system's outputs are true with respect to verifiable ground truth.

**What breaks when this fails:** Downstream decisions made on false information. In medical, legal, financial, or technical contexts, factuality failures create direct user harm and organizational liability.

**PRD phrases that trigger this contract:**
> "The system should provide accurate product specifications."  
> "Answers about regulatory requirements must be correct."  
> "The AI should not give outdated pricing information."

**Eval specification trigger:** Any mention of accuracy, correctness, data freshness, regulatory content, or domain-specific factual claims.

---

### Contract 4 — Cross-Modal Consistency

**The promise:** The system produces coherent, non-contradictory outputs across modalities. A claim about an image is consistent with the text describing that image. An audio-based answer is consistent with the text-based answer to the same question.

**What breaks when this fails:** Incoherent user experience. Multimodal systems that fail this contract create a particularly dangerous form of confusion because the inconsistency is invisible to any single-modality evaluation.

**PRD phrases that trigger this contract:**
> "Users should get the same answer whether they ask by voice or by text."  
> "Chart descriptions should match the data visible in the chart."  
> "The video summary should be consistent with the transcript summary."

**Eval specification trigger:** Any mention of multiple input channels, multimodal inputs, or consistency of experience across interaction modes.

---

### Contract 5 — Output Quality

**The promise:** The final output — regardless of modality — meets the standards of completeness, clarity, relevance, tone, and format that users and the business require.

**What breaks when this fails:** User adoption, satisfaction, and task completion. A system can pass all four upstream contracts and still produce outputs that are too long, too terse, poorly formatted, tonally inappropriate, or incomprehensible to its audience.

**PRD phrases that trigger this contract:**
> "Responses should be clear and concise."  
> "The system should match our brand voice."  
> "Summaries should be 3–5 bullet points, not paragraphs."  
> "Audio output should be natural-sounding, not robotic."

**Eval specification trigger:** Any mention of response quality, user experience, tone, format, length, readability, or audience-appropriateness.

---

> **A note on Safety:** Safety is a mandatory governance overlay applied across all five contracts — not a sixth standalone contract. Every multimodal eval specification must include an adversarial safety test set regardless of which quality contracts are triggered. In the Eval Stub and gate register, Safety always appears as a P0 item with a zero-tolerance threshold. It cannot be demoted to P1 or deferred.

---

## 9. Stakeholder Requirement Archetypes and Eval Translations

Different stakeholders produce different patterns of requirement language. Each archetype requires a different translation approach.

### Archetype 1 — The Executive Vision Statement

**How it appears:**
> "We want the AI to be the best assistant in our industry."
> "The system should just work."
> "Users should trust it."

**Translation approach:** Break into constituent quality claims. "Trust" maps to Faithfulness + Factuality + Abstention Accuracy. "Just work" maps to task success rate + escalation accuracy + latency. "Best in industry" requires a benchmark comparison — what does the competitive bar actually measure?

**Eval spec outputs:** Define "task success" for primary use cases; establish a competitive baseline; define the trust signal as a composite score with component weights; set thresholds that represent "best in industry" in operationalizable terms.

---

### Archetype 2 — The Feature Requirement

**How it appears:**
> "Users should be able to upload an image of a receipt and extract line items, totals, and vendor name."
> "The system should accept voice commands and return structured data."

**Translation approach:** Map to pipeline stages. Receipt image → Ingestion Fidelity (image parse, OCR) → Generation: structured extraction → Output: schema validation. Each stage has an eval dimension.

**Eval spec outputs:** OCR accuracy on receipt image test set (100+ cases, varied quality); field-level F1 for line items, totals, vendor name; schema compliance; edge case coverage (handwritten, low-resolution, multi-currency, partial receipts).

---

### Archetype 3 — The Safety and Compliance Requirement

**How it appears:**
> "The AI must never provide advice that violates our compliance policy."
> "The system cannot recommend competitor products."
> "Outputs must be HIPAA-safe."

**Translation approach:** Safety requirements are not confirmed by measuring passing cases — they are confirmed by measuring failure rates on adversarial test sets designed to elicit violations.

**Eval spec outputs:** Adversarial test set (200+ cases probing the policy boundary); safety pass rate target of 100%; failure taxonomy; escalation trigger definition; production monitoring specification.

---

### Archetype 4 — The Performance Requirement

**How it appears:**
> "The system should respond in under 3 seconds."
> "Cost should not exceed $0.02 per query."

**Translation approach:** Decouple latency/cost from quality. These generate separate eval tracks — and the intersection (quality at the required performance level) is where the real risk lives.

**Eval spec outputs:** Latency eval (p50, p95, p99); quality-at-latency eval (faithfulness and factuality scores at the cost-optimized model tier); cost-quality trade-off matrix; regression definition.

---

### Archetype 5 — The User Experience Requirement

**How it appears:**
> "The AI should feel natural to talk to."
> "Responses should match our brand voice."
> "The system should know when to be concise and when to go deep."

**Translation approach:** These translate to Output Quality dimensions with human-in-the-loop evaluation. Naturalness, brand voice, and response calibration cannot be fully automated — they require human judges with clear rubrics.

**Eval spec outputs:** Human eval rubric (define "natural," "brand-consistent," and "appropriately calibrated" as scorable dimensions); judge panel specification; MOS threshold.

---

### Archetype 6 — The Multimodal Consistency Requirement

**How it appears:**
> "Whether a user asks by voice or text, they should get the same quality of answer."
> "The system should accurately describe what is in an uploaded image."
> "Video summaries should match what is actually said and shown."

**Translation approach:** Build parallel test sets for each modality pair.

**Eval spec outputs:** Cross-modal parallel test set; consistency score; modality-specific quality scores (to ensure neither modality is systematically weaker); visual and audio grounding evals.

---

## 10. Vocabulary Mapping Dictionary

A working translator's dictionary. Left column is what stakeholders say. Right column is what to ask, and what it likely maps to in Evals Speak.

| Stakeholder Phrase | What to Ask | Evals Speak Destination |
|---|---|---|
| "Should be accurate" | Accurate at what? Compared to what reference? | Faithfulness + Factuality, metric depends on source of truth availability |
| "Should not hallucinate" | Hallucinate what kind of content? In what context? | Faithfulness (grounding); specific failure modes for fabricated entities, numbers, citations |
| "Should understand images" | Understand what — detection, OCR, layout, semantics, intent? | Ingestion Fidelity (capture) + Perception failure modes per content type |
| "Should feel grounded" | Grounded against the input or the world? | Faithfulness (input-grounded) vs. Factuality (world-grounded) — these are different contracts |
| "Should work for enterprise" | Which document types, sizes, formats, languages? | Massive slice grid expansion required |
| "Should be safe" | Safe against which threat models? | Separate safety spec; maps to Output Quality + safety contract |
| "Should be fast" | At what percentile, with what input distribution? | Performance SLO, separate from quality eval but tracked alongside it |
| "Should be consistent" | Within a session? Across users? Across modalities? | Cross-Modal Consistency vs. session consistency vs. inter-user consistency |
| "Should handle edge cases" | Which edges? Adversarial, degraded, partial, out-of-domain? | Explicit slice grid cells — do not let "edge cases" remain a category |
| "Should communicate uncertainty" | Uncertain about what? Expressed how? Calibrated against what? | Output Quality (form) + Faithfulness (substance) + calibration metric |
| "Should be helpful" | Helpful for which user goal? Measured how? | Output Quality with task-completion rubric |
| "Should work for documents" | Which formats, lengths, qualities, languages, sensitivities? | Massive slice expansion required before this is testable |
| "Should preserve meaning" | Across which transformations? Summarization, translation, modality conversion? | Faithfulness + Cross-Modal Consistency depending on transformation |
| "Should be production-ready" | At what scale, latency, cost, error budget? | Out of scope for quality eval; system SLOs |
| "Should not be sycophantic" | In which kinds of disagreements? | Output Quality rubric with explicit disagreement scenarios |

The pattern is consistent: every stakeholder phrase compresses two or three distinct evaluation primitives into a single word. The translator's job is to refuse that compression — politely and repeatedly.

---

## 11. The Multimodal Eval Specification Template

Use this template literally. Field discipline is part of the work. The most under-used section is **Section 12 — Known Limitations.** Every eval suite has them. Listing them explicitly is what separates a spec that is honest from a spec that is theater.

> **Standalone file:** The full standalone version of this template is [`T-21-multimodal-eval-spec-template.md`](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/templates/T-21-multimodal-eval-spec-template.md). Use that file when creating a new Eval Spec for a feature.

```markdown
# Multimodal Eval Specification: [Feature Name]

**Spec ID:** EVAL-[YYYY]-[NNN]
**Version:** [semver]
**Status:** Draft | In Review | Approved | Active | Deprecated
**Product Requirement Reference:** [PRD-ID or Feature Request ID]
**Product Owner:** [Name]
**Eval Owner:** [Name]
**Reviewers:** [Names]
**Approved By:** [Name]
**Approval Date:** [Date]
**Last Updated:** [Date]

---

## 1. Intent Statement
One paragraph in PRD speak, quoted or paraphrased from the source requirement.
This is the only place PRD speak is allowed in this document.

## 2. Scope and Non-Scope
What this evaluation covers.
What it explicitly does not cover — out-of-scope items must be enumerated.
Silence is not exclusion.

## 3. Atomic Capability Claims
Numbered list of decomposed claims confirmed by PRD owner.

## 4. Pipeline Stage Implication Matrix
| Claim # | Stage 1 | Stage 2 | Stage 3 | Stage 4 | Stage 5 | Stage 6 | Stage 7 | Stage 8 | Stage 9 |
|---|---|---|---|---|---|---|---|---|---|
| 1 | | | | | | | | | |

## 5. Quality Contract Assignments
| Claim # | Ingestion Fidelity | Faithfulness | Factuality | Cross-Modal Consistency | Output Quality |
|---|---|---|---|---|---|
| 1 | | | | | |

## 6. Failure Mode Register
For each (claim, contract) pair: concrete, observable failure modes with brief examples.
A competent reviewer must be able to score them without consulting the original team.

## 7. Measurement Primitives
For each failure mode:

| Failure Mode | Metric | Judge (named) | Dataset | Launch Threshold | Regression Threshold | Failure Consequence | Priority |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

## 8. Judge Panel Composition
Named judges, their rubrics (linked), their domains of authority, and their variance budgets.

| Judge | Model/Role | Rubric Reference | Domains | Agreement Target |
|---|---|---|---|---|
| | | | | |

## 9. Input Slice Grid
| Slice Dimension | Slices | Coverage Plan | Deferred? | Reason for Deferral |
|---|---|---|---|---|
| Modality presence | | | | |
| Input quality | | | | |
| Content domain | | | | |
| User intent | | | | |
| Length/duration | | | | |
| Language/locale | | | | |
| Sensitive subgroups | | | | |

## 10. Dataset Manifest
| Dataset | Source | Size | Slices Covered | Refresh Policy | Contamination Policy |
|---|---|---|---|---|---|
| | | | | | |

## 11. Gate Register

```
LAUNCH GATE: [Feature Name]

REQUIRED (P0 — blocking; failure = HOLD):
  [ ] [Failure Mode]: [metric] ≥ [threshold]
  [ ] [Safety]: Zero failures on adversarial test set

RECOMMENDED (P1 — high priority; failure = limited scope):
  [ ] [Failure Mode]: [metric] ≥ [threshold]

TRACKED (P2 — observed, not gated):
  [ ] [Failure Mode]: [metric] — trend watch

LAUNCH RECOMMENDATION LOGIC:
  All P0 and P1 gates met     → Full launch
  All P0 met, P1 gap(s)       → Limited launch with scope restriction and P1 roadmap
  Any P0 missed               → HOLD — do not ship
  Any safety failure          → HOLD — escalate immediately
```

## 12. Known Limitations
Honest list of what this eval cannot detect or where it is weak.
This section is not optional.

## 13. Production Monitoring Specification
| Dimension | Monitoring Method | Alert Threshold | Alert Owner | Response SLA |
|---|---|---|---|---|
| | | | | |

Monitoring Cadence: [Real-time / Hourly / Daily / Per-release]

## 14. Stakeholder Readout Template

**Verdict:** [LAUNCH READY / LIMITED LAUNCH — restriction / HOLD — reason]

| Stakeholder | One-Sentence Summary |
|---|---|
| CEO / CPO | |
| CFO | |
| Legal / CLO | |
| CTO | |
| Head of Sales | |
| Head of Support | |

**Where Quality is Strong:** [2–3 sentences]
**Where Quality Breaks:** [2–3 sentences — specific, not hedged]
**Recommended Decision:** [Specific, with evidence]
**Required Decision from Stakeholder:** [What you need]

## 15. Change Log
| Version | Date | Author | Summary |
|---|---|---|---|
| 1.0 | | | Initial specification |
```

---

## 12. Worked Examples

### Example 1 — Document Q&A (PDF Competitive Intelligence)

**Original Requirement:**
> "Product managers should be able to upload a competitive intelligence report (PDF) and ask natural language questions about it. Answers should be accurate and grounded in the document."

**Decomposed Quality Claims:**

| # | Quality Claim | Source Phrase | Modality | Stage | Contract |
|---|---|---|---|---|---|
| 1 | PDF content fully and accurately extracted | "upload...PDF" | Document | Stage 1 | Ingestion Fidelity |
| 2 | Charts and tables correctly represented | implied by "PDF" with likely mixed content | Image + Document | Stage 3 | Ingestion Fidelity |
| 3 | Answers contain only claims in the document | "grounded in the document" | Text | Stage 7 | Faithfulness |
| 4 | Answers factually correct w.r.t. document | "accurate" | Text | Stage 7 | Factuality |
| 5 | Correct source passage retrieved before answering | implied by RAG | Text | Stage 6 | Retrieval |
| 6 | Abstains correctly when question is out of scope | implied by "grounded" | Text | Stage 7 | Faithfulness |

**Metrics:**

| Dimension | Metric | Target | Priority |
|---|---|---|---|
| PDF parse coverage | % pages correctly extracted, including tables and charts | ≥ 98% | P0 |
| Table extraction accuracy | Field-level F1 on table benchmark | ≥ 0.90 | P0 |
| Chart description accuracy | LLM-Judge visual grounding score | ≥ 0.85 | P1 |
| Retrieval recall | Recall@5 on annotated Q&A pairs | ≥ 0.92 | P0 |
| Faithfulness | NLI-based faithfulness score | ≥ 0.92 | P0 |
| Factuality | Claim-level precision vs. document ground truth | ≥ 0.90 | P0 |
| Abstention accuracy | F1 on "I don't know" cases | ≥ 0.85 | P1 |

**Stakeholder Readout:**
> "The document Q&A system is launch-ready for text-heavy competitive reports. Quality breaks on reports where insight is encoded primarily in charts — trends are described accurately, but specific data point values are missed. Recommend launching with a usage note: 'For quantitative chart analysis, cross-check with the source.' P1 improvement targeted for Q3."

---

### Example 2 — Voice-to-Structured-Data (Field Technician Forms)

**Original Requirement:**
> "Field technicians should be able to dictate inspection notes by voice. The system should transcribe the voice input and populate a structured inspection form."

**Quality Claims:**

| # | Quality Claim | Modality | Stage | Contract |
|---|---|---|---|---|
| 1 | Speech accurately transcribed | Audio | Stage 3 | Ingestion Fidelity |
| 2 | Transcription works in noisy field environments | Audio | Stage 3 | Ingestion Fidelity |
| 3 | Technical domain vocabulary correctly recognized | Audio | Stage 3 | Ingestion Fidelity |
| 4 | Structured fields correctly populated from speech | Text (derived) | Stage 8 | Output Quality |
| 5 | No field hallucinated or invented | Text | Stage 7 | Faithfulness |
| 6 | Form schema always correctly matched | Structured | Stage 8 | Output Quality |

**Critical Cross-Modal Note:** The eval must test the *full chain*, not transcription quality and extraction quality independently. The compounding error rate (transcription error → extraction error) is the real risk.

**Metrics:**

| Dimension | Metric | Target | Priority |
|---|---|---|---|
| Transcription WER (clean audio) | Word error rate | ≤ 5% | P0 |
| Transcription WER (noisy, SNR < 15dB) | Word error rate | ≤ 15% | P0 |
| Domain vocabulary recognition | Named entity recall on inspection terms | ≥ 0.95 | P0 |
| Field extraction accuracy | Field-level F1 vs. ground truth forms | ≥ 0.92 | P0 |
| End-to-end form accuracy | % forms with zero extraction errors | ≥ 80% | P1 |
| Schema compliance | % outputs matching form schema | 100% | P0 |
| Hallucinated field rate | % fields with content not in speech | ≤ 0.5% | P0 |

**Stakeholder Readout:**
> "Voice-to-form is launch-ready for standard field conditions. Quality degrades in high-noise environments — compressor rooms, outdoor wind above 20 mph. Recommend launching to office and light-field technicians first. Heavy industrial rollout follows noise-robustness improvements. Estimated affected user percentage at current quality: 15%."

---

### Example 3 — Multimodal Customer Support Agent (Photo + Voice/Text)

**Original Requirement:**
> "Customers should be able to send a photo of a damaged product, describe the issue by text or voice, and receive an accurate resolution recommendation — replacement, repair guidance, or escalation to a human agent."

**Quality Claims:**

| # | Quality Claim | Modality | Stage | Contract |
|---|---|---|---|---|
| 1 | Photo analyzed correctly for damage | Image | Stage 7 | Ingestion Fidelity + Factuality |
| 2 | Damage type correctly classified | Image | Stage 7 | Factuality |
| 3 | Voice description accurately transcribed | Audio | Stage 3 | Ingestion Fidelity |
| 4 | Image and text/voice descriptions synthesized consistently | Image + Text/Audio | Stage 7 | Cross-Modal Consistency |
| 5 | Replacement / repair / escalation decision is correct | Text | Stage 8 | Factuality + Output Quality |
| 6 | Escalation fires correctly when required | Agent | Stage 8–9 | Safety / Output Quality |
| 7 | System does not recommend actions not in policy | Text | Stage 7 | Faithfulness |

**This is a safety-critical eval.** Incorrect escalation failure (not escalating when required) is a P0 failure with direct customer harm potential.

**Metrics:**

| Dimension | Metric | Target | Priority |
|---|---|---|---|
| Damage classification accuracy | Accuracy on damage taxonomy test set | ≥ 0.88 | P0 |
| Cross-modal consistency | Semantic agreement between image-only and text+image paths | ≥ 0.85 | P0 |
| Resolution decision accuracy | % correct vs. expert ground truth | ≥ 0.90 | P0 |
| Escalation recall | % of cases requiring escalation that are escalated | ≥ 0.98 | P0 (safety) |
| Escalation precision | % of escalations genuinely requiring human review | ≥ 0.85 | P1 |
| Policy faithfulness | Zero violations on adversarial set | 100% pass rate | P0 |
| Voice transcription WER | Word error rate on customer voice messages | ≤ 8% | P1 |

**Hardest eval challenge:** Building the ground-truth annotated test set for "correct" resolution decisions requires domain expert annotation and cannot be automated. Budget 3–4 weeks for 300+ cases.

---

### Example 4 — Lease Review Assistant (Document + Cross-Modal)

**Original Requirement:**
> "The assistant should help users review their leases."

**Why this example matters:** The compression is severe. "Help users review" is doing the work of at least six distinct atomic claims, several of which cross contract boundaries.

**Decomposed:**

1. Ingests lease documents in PDF, DOCX, and photo-of-paper formats.
2. Preserves structural elements: parties, dates, monetary terms, clauses.
3. Identifies non-standard or risky clauses against a reference of standard practice.
4. Answers questions using only the lease document.
5. Distinguishes "the lease says X" from "this is standard practice" — a Faithfulness vs. Factuality distinction.
6. Refuses or flags requests requiring licensure.

**Selected cross-modal failure modes for claim 4:**
- Fabricates a clause number not present in the lease
- Reports a monetary amount with wrong currency unit or magnitude
- Answers confidently about a section that was illegible in the scan, without flagging illegibility
- Conflates a clause from an earlier visible draft with the executed version

**Note on judge selection for illegibility-without-flag:** This failure mode requires human judges on a slice of deliberately degraded scans. An LLM judge shares the same blind spot — it may also fail to flag what it cannot read.

---

### Example 5 — Screen-Aware Voice Assistant (Cross-Modal)

**Original Requirement:**
> "Make the voice assistant feel more attentive when sharing my screen."

**Why this example matters:** "Feel more attentive" compresses at least four distinct testable claims, and the failure modes are almost entirely cross-modal.

**Decomposed:**
1. Audio inputs transcribed faithfully under typical home and office conditions.
2. Screen content at utterance time is ingested and represented to the model.
3. Responses reference specific on-screen elements when the user's request is screen-relevant.
4. Responses do not reference on-screen elements when the user's request is screen-irrelevant.
5. Volume of screen commentary calibrated to user intent.

**Cross-modal failure modes (where this feature actually lives):**
- *Modality confusion:* Describes audio content as if it were on screen, or vice versa.
- *Stale frame grounding:* References a UI element visible 30 seconds ago, no longer on screen.
- *Phantom reference:* References an on-screen element that does not exist (visual hallucination).
- *Modality dropout:* Silently ignores the screen when it was clearly relevant.
- *Modality over-pull:* Spontaneously narrates the screen when user asked an unrelated question.

**Critical judge panel note:** Do not use an LLM judge for phantom references. LLMs share the same visual hallucination failure mode as the system under test. Human judges, or specialized detection models, must cover this slice.

---

## 13. Modality-Specific Eval Considerations

### 13.1 Image Modality

**Unique risks:**
- Systems hallucinate image content confidently — claiming to see things not present
- Chart and diagram understanding is qualitatively different from photograph understanding
- Low-resolution, poorly-lit, or partially obscured images degrade rapidly
- OCR on image-embedded text introduces compounding errors

**Eval requirements:**
- Always include a visual grounding check — verify claims against visible content using a specialist vision model or human judge
- Build separate test sets for: photographs, diagrams, charts, screenshots, scanned documents, low-quality images
- Test degraded inputs explicitly: rotation, poor lighting, partial occlusion, compression artifacts
- For chart understanding: test trend recognition (qualitative) and value extraction (quantitative) separately — they fail differently

---

### 13.2 Audio Modality

**Unique risks:**
- Transcription errors cascade — one misheard word can change a claim's meaning
- Speaker diarization failures produce incoherent outputs for multi-speaker inputs
- Acoustic environment variation creates systematic coverage gaps
- Emotion and prosody are lost in transcription and affect meaning

**Eval requirements:**
- Always test WER across acoustic conditions, not only clean speech
- Build a domain vocabulary test set: technical terms, proper nouns, product names
- For multi-speaker audio: include diarization accuracy as a separate eval dimension
- For voice agents: test transcription quality and downstream task accuracy independently

---

### 13.3 Video Modality

**Unique risks:**
- Video combines image and audio failure modes — errors can originate from either track
- Temporal coherence is an additional eval dimension not present in static modalities
- Scene transitions and speaker changes create segmentation challenges
- Long-form video (>10 minutes) introduces context window and memory challenges

**Eval requirements:**
- Always evaluate visual and audio tracks independently before evaluating joint output
- Add temporal accuracy as an eval dimension: does the system correctly identify *when* an event occurs?
- For video summarization: test that summaries cover the full duration, not only the opening minutes
- Build test sets including: single speaker/no cuts, multi-speaker dialogue, fast-cut editing, and poor audio

---

### 13.4 Document Modality (PDF / DOCX / PPTX)

**Unique risks:**
- Document layout encodes meaning — parsers that strip layout destroy information
- Multi-column layouts, headers/footers, and watermarks create parsing artifacts
- Embedded images within documents require image eval in addition to document eval
- Scanned PDFs require OCR and introduce compounding error rates

**Eval requirements:**
- Test parsing fidelity separately from comprehension quality
- Include: text-native PDFs, scanned PDFs, mixed-content, multi-column, merged-cell tables, footnotes
- For PPTX: test speaker note extraction and visible slide content extraction separately
- Measure table extraction separately from body text — they require different parsing approaches

---

### 13.5 Cross-Modal Interactions

The highest-risk eval gap in multimodal systems is the interaction between modalities — where a failure is invisible when each modality is evaluated independently.

**Cross-modal failure patterns to test:**

| Pattern | Description | Test Approach |
|---|---|---|
| **Modality override** | System ignores image and answers from text context only | Present conflicting image and text; verify system uses correct modality |
| **Hallucinated synthesis** | System fabricates connections between text and image that don't exist | Present unrelated image and text; verify system doesn't invent relevance |
| **Modality confidence asymmetry** | System presents confident claims from a poor-quality input alongside accurate claims from a good-quality input | Present one high-quality and one low-quality input; test uncertainty expression |
| **Reference failure** | System correctly identifies image content but fails to connect it to the text query | Verify generation stage correctly uses retrieved image context |
| **Temporal-spatial confusion** | System attributes content to wrong time or speaker in video | Annotated temporal ground truth test set |

---

## 14. Eval Coverage Matrix

Use this matrix to audit whether a proposed eval specification covers all required dimensions. Mark each cell: ✅ Covered, ⚠️ Partial, ❌ Missing, N/A Not applicable.

| Quality Contract | Text | Image | Audio | Video | Document | Structured | Cross-Modal |
|---|---|---|---|---|---|---|---|
| **Ingestion Fidelity** | | | | | | | |
| **Faithfulness** | | | | | | | |
| **Factuality** | | | | | | | |
| **Cross-Modal Consistency** | N/A | N/A | N/A | N/A | N/A | N/A | |
| **Output Quality** | | | | | | | |
| **Safety** | | | | | | | |
| **Retrieval Quality** | | | | | | | |
| **Latency / Cost** | | | | | | | |
| **Escalation / Abstention** | | | | | | | |

**Coverage rule:** Any cell marked ❌ for a modality that the feature actively uses is a specification gap that must be resolved before the eval spec is approved.

---

## 15. Eval Specification Review Checklist

### Completeness
- [ ] All quality claims from the product requirement are identified and mapped
- [ ] All modalities present in the system are covered by at least one eval dimension
- [ ] All five Quality Contracts are assessed (with clear N/A where not applicable)
- [ ] Launch gate thresholds are defined for all P0 dimensions
- [ ] Test set composition is defined with target sizes and sources
- [ ] Ground truth specification is defined for every dimension requiring it
- [ ] Section 12 (Known Limitations) is populated honestly

### Rigor
- [ ] Thresholds are justified, not arbitrarily round numbers
- [ ] Edge cases and adversarial cases are included in test set specification
- [ ] Safety dimensions carry a zero-tolerance adversarial test set requirement
- [ ] Cross-modal consistency is evaluated if more than one modality is involved
- [ ] Compounding error rates are assessed for multi-stage pipelines
- [ ] No LLM judge is used for failure modes that LLMs share with the system under test

### Executability
- [ ] Every metric has a defined measurement method
- [ ] LLM-as-Judge configurations have a named model, rubric reference, and run configuration
- [ ] Human annotation requirements have a judge count, qualification, and agreement target
- [ ] Test set sources are available or have a procurement plan
- [ ] Annotation timeline is estimated

### Governance
- [ ] Spec is linked to its originating product requirement
- [ ] Approval chain is defined with named owners
- [ ] Gate register has a named owner and review cadence per gate
- [ ] Production monitoring specification is included
- [ ] Stakeholder readout template is populated with role-specific language
- [ ] Change log is present

---

## 16. Anti-Patterns and Failure Modes

Each anti-pattern has a **tell** — the signal that it is present before it causes damage.

**Anti-Pattern 1 — The Metric Without a Threshold.**
*Tell:* The eval report says "we measured faithfulness" without a number or gate.
*Why it fails:* A metric without a threshold governs nothing. No launch gate, no regression alert, no stakeholder conversation is possible.
*Correction:* Every metric must have a defined threshold. If you cannot justify a specific value, document the reasoning for your placeholder and commit to calibrating on the first evaluation run.

---

**Anti-Pattern 2 — The Single-Modality Eval.**
*Tell:* A multimodal system is evaluated only on its text output. Score card shows only generation-layer metrics.
*Why it fails:* The system may produce high-quality text while hallucinating or misrepresenting the image, audio, or document content that informed that output.
*Correction:* Use the Eval Coverage Matrix. Every modality the system ingests or produces requires at least one eval dimension.

---

**Anti-Pattern 3 — The Benchmark Substitution.**
*Tell:* "We evaluated on MMMU and MMBT and scored above average."
*Why it fails:* Public benchmarks test general capabilities, not your product's specific behavior on your users' specific content. An 83rd percentile score on a benchmark is meaningless if your users primarily upload low-quality scanned invoices.
*Correction:* Build or augment your test set with cases from your actual use case domain. Benchmark scores are a starting point, not a launch gate.

---

**Anti-Pattern 4 — Eval Spec Written After Development.**
*Tell:* The product ships, something breaks, then the team writes an eval to diagnose the failure.
*Why it fails:* Post-hoc evals measure the system that was built, not the system that was required. They cannot prevent launch failures; they can only explain them.
*Correction:* Eval specs are written concurrently with or before the PRD. The eval spec *is* the acceptance criterion.

---

**Anti-Pattern 5 — Safety as a P2.**
*Tell:* Safety dimensions are marked "recommended" rather than "required" in the launch gate.
*Why it fails:* Any safety failure that reaches production is a P0 incident regardless of the spec classification. Marking safety as optional creates organizational cover for shipping unsafe systems.
*Correction:* Safety dimensions are always P0. Zero-tolerance thresholds. Required adversarial test coverage. No exceptions.

---

**Anti-Pattern 6 — The LLM-Judge Monoculture.**
*Tell:* Every score in the suite is produced by an LLM judge, often the same model family as the system under test. Judge scores correlate suspiciously with no human anchor.
*Why it fails:* LLM judges share failure modes with LLM systems. Phantom visual references, modality hallucinations, and confidently-wrong responses are exactly the failures that LLM judges miss because they are susceptible to the same.
*Correction:* Rotate human spot-checks quarterly on the failure modes most prone to shared blind spots. Even a 5% human sample changes what you can see.

---

**Anti-Pattern 7 — The Slice-Free Aggregate.**
*Tell:* Reported scores are aggregated across all inputs. The team reports "we improved" but a specific subgroup degraded.
*Why it fails:* Aggregate scores hide the slices where the system actually fails. A multimodal system that performs well on clean inputs and fails on degraded ones looks fine in aggregate.
*Correction:* Slice-level reporting is the default. Aggregate is the summary of the eval, not the eval.

---

**Anti-Pattern 8 — The Disconnected Stakeholder Readout.**
*Tell:* Eval results are delivered as a raw metrics table. Stakeholders make launch decisions on instinct.
*Why it fails:* The eval investment produces no governance value. Evidence that cannot be read does not improve decisions.
*Correction:* Every eval spec includes a stakeholder readout template. Every launch review leads with a verdict in plain language before presenting any metric.

---

## 17. Monday Morning Actions

The point of this document is not for it to be read. The point is for it to change what you do on Monday morning. Pick the actions that apply to where you are.

**If you are starting a new multimodal feature this week:** Begin with DECODE Step D (Decompose) before any code is written. Spend two hours decomposing the feature request with the PM in the room. Produce a numbered list of atomic claims. Get a signature. The act of getting a signature surfaces assumptions the team didn't know were there.

**If you are inheriting an existing multimodal system without a spec:** Reconstruct the spec backward. Read the code. Read the PRDs. List every capability the system appears to be claiming. Run the six-step DECODE methodology against the reverse-engineered claim set. The gaps you find are not failures — they are the work.

**If you have a spec but no slice grid:** Build the grid this week. Even a hand-drawn whiteboard version. The act of placing your current eval coverage onto a deliberate grid will reveal the unevaluated slices carrying production risk.

**If you have a spec but no Section 12 (Known Limitations):** Add it today. Spend one hour writing it honestly. Share it with the PM. The relationship is now stronger, because both sides know what is and is not being measured.

**If your judges are all LLM-as-Judge:** Rotate one human review cycle this quarter on the failure modes most prone to shared blind spots: phantom references, modality confusion, and confidently-wrong responses. Even a 5% human sample changes what you can see.

**If your team conflates "the model hallucinated" with "the input was bad":** Add Ingestion Fidelity instrumentation before any further Faithfulness work. You cannot diagnose Faithfulness without it.

**If your eval thresholds have not been reviewed since launch:** Review them this sprint. Systems mature, user populations shift, and a threshold set when 10,000 users were active may be wrong at 1,000,000. Every gate has a quarterly review cadence — if yours doesn't, add it.

**If you have never written an eval spec:** Use the template in Section 11 for your next feature. Do not improve it on the first pass. Use it literally. Refine on the second pass.

---

## 18. Closing Principles

Five principles that summarize the discipline.

**Refuse the compression.** Every stakeholder sentence is a compressed claim. Politely, repeatedly, refuse to evaluate compressed claims. Decompose first. The decomposition conversation is where the most important assumptions surface.

**Name the judge.** No anonymous scores. Every metric has a named judge, a versioned rubric, and a variance budget. If a score has no judge, it has no meaning. If the judge is an LLM, name it completely: model, version, temperature, run count, voting rule.

**Slice before you aggregate.** The slice grid is the eval. The aggregate is the summary of the eval. Reporting only the summary is reporting only the part that hides the failures. A multimodal system that passes in aggregate and fails in one slice has failed.

**Write down what you are not measuring.** Section 12 is not optional. The integrity of an eval suite is determined as much by its honest limitations as by its measured passes. Known unknowns are manageable. Unknown unknowns are how multimodal AI products embarrass themselves in production.

**Make the spec the contract.** The eval specification is the durable artifact between product and evaluation. Version it. Sign it. Review it when the product changes. Treat it like code. A spec that describes a product surface that no longer exists is worse than no spec, because it creates false assurance.

A multimodal AI product is a stack of claims rendered in a stack of modalities. An evaluation is a stack of measurements rendered against a stack of contracts. Translation is the discipline that keeps the two stacks honest with each other. Without it, the gap between what we said we built and what we actually built widens with every release. With it, the gap becomes measurable, named, and — eventually — closable.

**If you are using a PRD process without embedded eval requirements:** Take Section 20 of this document to your next sprint planning session. Add the Eval Stub template to your PRD standard template this week. The first PRD that ships with a completed Eval Stub will set the organizational standard.

---

## 19. Glossary

| Term | Definition |
|---|---|
| **Atomic Capability Claim** | The smallest unit of system behavior that can be answered "did / did not" by examining a single output without interpretation |
| **Eval Spec** | A formal versioned document defining what will be measured, how, and what threshold must be met for a feature to ship or continue in production |
| **Quality Claim** | An implicit or explicit assertion about system quality embedded in a product requirement or stakeholder statement |
| **Launch Gate** | A pre-agreed set of eval thresholds that must all be met before a feature ships. Failure on any single P0 gate blocks release |
| **Regression Threshold** | The score below which a production monitoring alert fires, indicating quality has degraded from baseline |
| **Specification Debt** | Evaluation criteria derived after the fact from complaints, regressions, and launch failures rather than from pre-development requirements |
| **Ingestion Fidelity** | The degree to which source content is correctly extracted and represented before any generation occurs |
| **Faithfulness** | The degree to which a generated output contains only claims supported by the provided context |
| **Factuality** | The degree to which a generated output's claims are true with respect to verifiable ground truth |
| **Cross-Modal Consistency** | The degree to which a system produces coherent, non-contradictory outputs across different modalities |
| **Output Quality** | The overall quality of the final output as experienced by the end user — completeness, clarity, tone, and format |
| **LLM-as-Judge** | An evaluation method in which a language model scores another AI system's output against a defined rubric |
| **Judge Panel** | A structured configuration of multiple evaluators (human and/or AI) whose scores are aggregated into a final quality signal |
| **Visual Grounding** | The degree to which claims about an image are supported by content visibly present in that image |
| **Audio Grounding** | The degree to which claims about an audio or video track are supported by content audibly present in that track |
| **WER (Word Error Rate)** | The proportion of words in a transcription that differ from the reference transcript; the primary metric for speech recognition quality |
| **Cohen's Kappa** | A statistical measure of inter-annotator agreement that accounts for chance agreement; ≥ 0.70 is the target for high-quality annotation |
| **Compounding Error Rate** | The error accumulation effect in multi-stage pipelines where upstream failures amplify downstream failures |
| **EDD (Evaluation-Driven Development)** | A development methodology in which eval specifications are written before features ship and failure cases drive the product roadmap |
| **DECODE** | The six-step methodology for translating product requirements into eval specifications: **D**ecompose requirements into atomic claims → **E**numerate modalities and input slices → **C**lassify by pipeline stage, quality contract, and failure mode → **O**perationalize each failure mode as a measurement primitive → **D**efine thresholds, gates, datasets, and monitoring → **E**xpress as a formal eval specification |
| **Five Quality Contracts** | The five foundational quality commitments of a multimodal AI system: Ingestion Fidelity, Faithfulness, Factuality, Cross-Modal Consistency, Output Quality |
| **Nine-Stage Multimodal Pipeline** | The standard pipeline architecture: Input Ingestion → Modality Detection → Preprocessing → Chunking → Embedding → Retrieval → Generation → Output Formatting → Delivery |
| **MOS (Mean Opinion Score)** | A human-judged quality score averaged across a panel of evaluators; the standard for perceptual quality in audio and output quality evaluations |
| **Modality Blindspot** | A modality that a multimodal system actively processes for which no eval dimension has been defined |
| **Phantom Reference** | A cross-modal failure mode in which a system references an element in an image, audio, or video that is not present |
| **Modality Override** | A cross-modal failure mode in which a system ignores one input modality and answers from another, even when the ignored modality was the relevant one |
| **Ship Gate** | A hard threshold whose failure blocks release; tied to user-visible harm or public capability claims |
| **Experiment Gate** | A threshold that blocks merging a change into the main candidate; higher noise tolerance than a ship gate |
| **Eval Stub** | A lightweight structured artifact (7 sections, authored by PM) that lives inside a PRD and captures the minimum evaluation commitments — quality contracts triggered, modalities involved, top failure modes, P0 launch gate summary, high-risk slice, and out-of-scope conditions — before a feature moves to development. Graduates to a full Eval Spec during development. |
| **PRD Eval Requirements Section** | The container section added to every AI feature PRD that holds the Eval Stub, the Eval Spec reference link, the evaluation commitments checklist, and the P0 gate summary for executive visibility. |
| **Concurrent Authoring Protocol** | The workflow in which the Eval Stub is drafted in the same session as the PRD feature requirement, ensuring evaluation commitments are made before development assumptions harden. |

---

## 20. Appendix: Populating AI Product PRDs with Eval Specifications

This appendix closes the gap between the DECODE translation framework and the practical workflow of authoring AI product requirements. It introduces three artifacts that do not exist anywhere else in this document: the **Eval Stub**, the **PRD Eval Requirements Section Template**, and the **Concurrent Authoring Protocol**. Together they make it possible to embed evaluation requirements into a PRD during the authoring session — not after.

---

### 20.1 The Eval Stub

The Eval Stub is a named, defined artifact that lives *inside* the PRD. It is not a summary of the full Eval Spec. It is a structured commitment: the minimum set of evaluation decisions that must be made before a feature requirement is considered complete.

A full Eval Spec (Section 11) is 15–20 pages and is authored by an eval engineer. An Eval Stub is 10–12 fields and is authored by the PM — ideally in the same session as the feature requirement itself.

The relationship between the two artifacts:

```
PRD Feature Requirement
        │
        ├── contains → EVAL STUB (authored by PM, lives in PRD)
        │                    │
        │                    └── references → FULL EVAL SPEC (authored by eval engineer, lives in repo)
        │
        └── links to → Product Design, Technical Architecture, etc.
```

**The Eval Stub is the handoff document.** When an eval engineer receives a PRD containing a completed Eval Stub, they can begin Step O (Operationalize) of DECODE immediately — without a translation session, without a requirements meeting.

---

#### Completed Eval Stub Example — Document Q&A (PDF Competitive Intelligence)

This example corresponds to the worked example in Section 12.1 and shows what a fully completed stub looks like at the end of the Draft PRD session.

```markdown
## Eval Stub: Document Q&A — Competitive Intelligence PDF Feature

**Stub ID:** STUB-2025-001
**PRD Reference:** PRD-2025-042
**Full Eval Spec Reference:** EVAL-2025-007
**Eval Owner:** [Eval Engineer Name]
**DECODE Progress:** [x] D  [x] E  [x] C  [ ] O  [ ] D  [ ] E
**Stub Status:** Complete

---

### 1. Quality Contracts Triggered

- [x] Ingestion Fidelity — system must correctly parse or extract source content
- [x] Faithfulness — outputs must be grounded in provided source material
- [x] Factuality — outputs must be correct with respect to ground truth
- [ ] Cross-Modal Consistency — not triggered; single input modality path
- [ ] Output Quality — deferred to P1; format requirements minimal for v1
- [x] Safety — governance overlay; adversarial test set required (always P0)

**Primary contract:** Faithfulness

---

### 2. Modalities Involved

- [x] Text (query input and answer output)
- [x] Image (charts and tables embedded in PDFs)
- [ ] Audio
- [ ] Video
- [x] Document (PDF primary input type)
- [ ] Structured Data
- [x] Cross-modal output (text answers reference and describe visual chart content)

---

### 3. Top Failure Modes

| # | Failure Mode (concrete, observable) | Contract | Priority |
|---|---|---|---|
| 1 | System generates an answer citing a claim not present in the uploaded PDF | Faithfulness | P0 |
| 2 | System misreads a table or chart and reports an incorrect data value (e.g., wrong revenue figure, wrong percentage) | Ingestion Fidelity | P0 |
| 3 | System answers confidently about an illegible scan region without flagging uncertainty | Faithfulness | P1 |

**Failure mode test:** A reviewer given only the PDF and the system answer can determine whether each failure occurred without consulting the team. ✓ All three pass.

---

### 4. P0 Launch Gate Summary

| Dimension | Metric | Minimum Threshold |
|---|---|---|
| PDF parse coverage | % pages fully extracted including embedded tables and charts | ≥ 98% |
| Faithfulness | NLI-based faithfulness score on document Q&A benchmark | ≥ 0.92 |
| Retrieval recall | Recall@5 on 200-case annotated Q&A test set | ≥ 0.92 |
| Factuality | Claim-level precision vs. document ground truth | ≥ 0.90 |
| Safety | Zero violations on adversarial test set | 100% |

---

### 5. High-Risk Slice

"Multi-column PDF reports where primary insight is encoded in charts with no accompanying data table — the system must extract quantitative claims from visual content rather than text."

---

### 6. Known Out-of-Scope Conditions

| Out-of-Scope Condition | Reason |
|---|---|
| Handwritten documents | OCR pipeline not trained on handwriting; deferred to Q3 |
| Documents over 100 pages | Context window constraints; chunking strategy under evaluation |
| Non-English documents | Localization not in scope for v1 |

---

### 7. Eval Stub Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| Product Owner | | | |
| Eval Owner | | | |
```

**What to observe in this example:** The DECODE Progress tracker shows D, E, and C are complete — reflecting work done in the 90-minute Draft PRD session. Steps O, D, and E remain unchecked, to be completed by the eval engineer in the full Eval Spec. The Safety checkbox is checked and marked as a governance overlay even though Safety is not the primary contract. The high-risk slice is a single concrete sentence, not a category.

---

#### Eval Stub Template

```markdown
## Eval Stub: [Feature Name]

**Stub ID:** STUB-[YYYY]-[NNN]
**PRD Reference:** [PRD-ID]
**Full Eval Spec Reference:** [EVAL-ID — or "TBD: to be created by eval engineer"]
**Eval Owner:** [Name or "TBD"]
**DECODE Progress:** [ ] D  [ ] E  [ ] C  [ ] O  [ ] D  [ ] E
**Stub Status:** Draft | Complete | Eval Spec Initiated | Eval Spec Approved

---

### 1. Quality Contracts Triggered
Mark all that apply. At least one required before this stub is considered complete.

- [ ] Ingestion Fidelity — system must correctly parse or extract source content
- [ ] Faithfulness — outputs must be grounded in provided source material
- [ ] Factuality — outputs must be correct with respect to ground truth
- [ ] Cross-Modal Consistency — outputs must be coherent across modalities
- [ ] Output Quality — outputs must meet format, tone, and completeness standards
- [x] Safety — outputs must comply with policy, compliance, or harm-avoidance requirements *(governance overlay — required on all features regardless of which quality contracts are triggered)*

**Primary contract:** [Name the single most important one]

---

### 2. Modalities Involved
Mark all that the system processes as input or produces as output.

- [ ] Text
- [ ] Image
- [ ] Audio
- [ ] Video
- [ ] Document (PDF / DOCX / PPTX)
- [ ] Structured Data (JSON / CSV / Tables)
- [ ] Cross-modal output (output that synthesizes or references multiple input modalities)

---

### 3. Top Failure Modes
List the three most likely or most consequential ways quality can break for this feature.
These become the first entries in the full Eval Spec's Failure Mode Register (Step C).

| # | Failure Mode (concrete, observable) | Contract | Priority |
|---|---|---|---|
| 1 | | | P0 / P1 / P2 |
| 2 | | | P0 / P1 / P2 |
| 3 | | | P0 / P1 / P2 |

**Failure mode test:** A reviewer, given only the input and the output, can determine whether this failure occurred without asking the original team. If not, rewrite it.

---

### 4. P0 Launch Gate Summary
List only the thresholds whose failure would block launch. Leave blank if thresholds are not yet known — the eval engineer will define them in the full spec.

| Dimension | Metric | Minimum Threshold |
|---|---|---|
| | | |
| | | |
| Safety | Zero violations on adversarial test set | 100% (always P0) |

---

### 5. High-Risk Slice
Describe in one sentence the input condition most likely to cause failure that the standard test set would miss.

> Example: "Low-quality scanned PDFs with merged table cells and no embedded text layer."
> Example: "Voice inputs recorded in ambient noise above 70dB (SNR < 15dB)."

[Your high-risk slice here]

---

### 6. Known Out-of-Scope Conditions
List any input types, use cases, or user populations this feature explicitly does NOT cover.
Silence is not exclusion — unstated scope will be assumed to be in scope by the eval engineer.

| Out-of-Scope Condition | Reason |
|---|---|
| | |

---

### 7. Eval Stub Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| Product Owner | | | |
| Eval Owner | | | |
```

---

### 20.2 The PRD Eval Requirements Section Template

This is the section that gets added to every AI feature PRD. It is not the Eval Stub — it is the *container* that holds the Eval Stub plus the pointers and commitments that governance requires.

Copy this section template into your PRD standard template. Every AI feature PRD should contain it. The standalone file is [`T-20-prd-eval-requirements-section.md`](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/templates/T-20-prd-eval-requirements-section.md).

```markdown
---

## Evaluation Requirements

> This section records the evaluation commitments for this feature. It is co-authored
> by the PM and eval engineer during or immediately after the feature requirement
> authoring session. It must be completed before the feature moves to development.

### Eval Stub
[Paste completed Eval Stub here — see Eval Stub Template]

### Eval Specification Reference
- **Full Eval Spec:** [Link to EVAL-[YYYY]-[NNN] in the Multimodal-AI-Evaluations repo]
- **Eval Spec Status:** [Not yet initiated / In progress / Approved / Active]
- **Target Eval Spec Completion Date:** [Date — must precede development start]

### Evaluation Commitments

The following commitments are made at PRD approval time:

- [ ] Eval Stub is complete and signed off by both Product Owner and Eval Owner
- [ ] Full Eval Spec will be completed before development begins
- [ ] Launch gate thresholds are pre-agreed and documented in the Eval Spec
- [ ] Production monitoring specification is included in the Eval Spec
- [ ] Any changes to feature scope require a corresponding PR to the Eval Spec

### Launch Gate Summary
This feature will not ship unless all P0 thresholds in the Eval Spec are met.
[Copy P0 gate summary from Eval Stub Section 4 here for executive visibility]

---
```

---

### 20.3 The Concurrent Authoring Protocol

The DECODE methodology is most effective when run *during* PRD authoring — not after. This protocol defines the workflow, the session structure, the participant roles, and the artifact state at each stage of the PRD lifecycle.

> **Printable session guide:** A standalone worksheet for running the 90-minute Draft PRD session is available at [`T-22-decode-translation-worksheet.md`](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/templates/T-22-decode-translation-worksheet.md). Print it or open it in a shared doc before the session begins.

#### The Core Principle

**The Eval Stub is written in the same session as the feature requirement.** Not the next day. Not by a different team. In the same room, at the same time, by the same participants. Every hour of delay between writing the requirement and writing the Eval Stub is an hour in which assumptions harden, scope drifts, and the translation becomes harder.

---

#### Protocol: Who Does What and When

| PRD Stage | DECODE Steps to Complete | Minimum Eval Artifact | Who | Time Budget |
|---|---|---|---|---|
| **Feature Ideation** | D only (Decompose) | Numbered list of atomic claims | PM | 30 min |
| **Draft PRD** | D + E (+ start C) | Eval Stub Sections 1–3 complete | PM + Eval Engineer | 60–90 min |
| **PRD Review** | C complete | Eval Stub Sections 4–6 complete, stub signed off | PM + Eval Engineer | 30 min |
| **PRD Approved** | O + D | Full Eval Spec in progress, Step O and D complete | Eval Engineer (PM review) | 1–2 weeks |
| **Development Start** | All six steps complete | Full Eval Spec approved | Eval Engineer | Before first sprint |
| **Launch Review** | — | Eval results mapped to Stakeholder Readout Template | PM | At launch gate |

---

#### The Session Structure: Draft PRD Session (60–90 min)

This is the highest-leverage session in the entire eval program. Run it as follows.

**Participants:** PM (required) + Eval Engineer (required). Designer and TL optional but valuable.

**Inputs:** Draft feature requirement text. Vocabulary Mapping Dictionary (Section 10 of this document).

**Agenda:**

```
0:00–0:15   PM reads requirement aloud. Eval engineer listens for compression.
            Both parties flag any phrase from the Vocabulary Mapping Dictionary.
            Do not proceed until every compressed phrase has been questioned.

0:15–0:35   DECODE Step D — Decompose.
            PM and eval engineer jointly enumerate atomic claims.
            PM confirms the list reflects intent. Eval engineer pushes for atomicity.
            Target: 5–10 atomic claims for a standard feature requirement.

0:35–0:55   DECODE Step E — Enumerate modalities and slices.
            Complete the Modality Inventory checklist together.
            Name the high-risk slice (Eval Stub Section 5).
            Name the out-of-scope conditions (Eval Stub Section 6).

0:55–1:15   DECODE Step C (partial) — Assign quality contracts and name failure modes.
            For each atomic claim: which contract does it trigger?
            For the top 3 claims: what are the concrete, observable failure modes?
            Complete Eval Stub Sections 1–3.

1:15–1:30   Complete Eval Stub Sections 4–6 (gate summary, high-risk slice, out-of-scope).
            Both parties sign off. Eval engineer takes ownership of full Eval Spec.
            PM embeds completed stub in PRD Evaluation Requirements section.
```

**Output:** Completed Eval Stub embedded in PRD. Eval engineer assigned. Full Eval Spec initiation scheduled.

---

#### How the Eval Stub Graduates to a Full Eval Spec

The Eval Stub is a commitment, not a specification. It names the contracts, the failure modes, and the risk — but does not yet define the metrics, judges, datasets, thresholds, or slice grid in full. The eval engineer graduates it to a full spec by completing Steps O and D of DECODE, then expressing the result in the Section 11 template.

The graduation path:

```
EVAL STUB (authored in PRD session)
    │
    │  Eval engineer completes:
    │  Step O — Operationalize each failure mode as a measurement primitive
    │  Step D — Define thresholds, gates, datasets, and monitoring
    │
    ▼
FULL EVAL SPEC (EVAL-[YYYY]-[NNN] in repo)
    │
    │  Both owners review and approve
    │
    ▼
ACTIVE EVAL PROGRAM (gates enforced, monitoring live)
```

The stub's Failure Mode Register becomes the full spec's Section 6. The stub's P0 gate summary becomes the full spec's Section 11 Gate Register — expanded with complete threshold definitions, dataset references, and owners. Nothing in the stub is discarded; it is expanded.

---

#### Minimum Viable Eval Stub at Each Stage

If time is constrained, these are the absolute minimum fields that must be complete before the PRD advances:

| PRD Stage | Minimum Complete Fields |
|---|---|
| Draft | Sections 1 (contracts) + 2 (modalities) + failure mode table (at least 2 rows) |
| Review | All stub sections complete, Section 4 (P0 gates) populated even if placeholder thresholds |
| Approved | Stub signed off by both owners, Eval Spec initiation date confirmed |

A PRD with an incomplete Eval Stub in the "Approved" state is a governance failure. The stub is a required artifact, not an optional one.

---

#### The Solo PM Workflow (No Eval Engineer Available)

When an eval engineer is not available for the drafting session, the PM completes the Eval Stub solo using the following abbreviated protocol:

1. Open the Vocabulary Mapping Dictionary (Section 10). Highlight every phrase in the feature requirement that appears in the left column. Write down the "What to Ask" question for each.
2. Run DECODE Step D solo: list every atomic capability claim. Aim for at least five.
3. Complete Eval Stub Sections 1 and 2 (contracts and modalities) — these do not require eval expertise.
4. Write at least two concrete failure modes in Section 3. Use the atomicity test: can a reviewer score it from input + output alone?
5. Leave Section 4 (P0 gates) blank with the note "Thresholds to be defined by eval engineer."
6. Complete Sections 5 and 6 (high-risk slice and out-of-scope) based on your knowledge of the user population.
7. Flag the stub as "Eval Engineer Review Required" and assign it before the PRD review meeting.

A PM-only stub is not a complete stub. It is a starting point that reduces the eval engineer's session time from 90 minutes to 30.

---

**Companion guides:**
[Practitioner's Field Manual](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/guides/Multimodal%20AI%20Product%20Evaluation%20-%20The%20Practioner's%20Field%20Manual%20-%20v2.1.pdf) · [101 Guide](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/guides/Multimodal%20AI%20Product%20Evaluation%20%E2%80%94%20101%20Guide%20-%20Enhanced.pdf) · [Getting Started Guide](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/guides/Multimodal%20AI%20Product%20Evaluation%20-%20Getting%20Started.pdf)

**Companion templates for this document:**
[T-19 Eval Stub Template](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/templates/T-19-eval-stub-template.md) · [T-20 PRD Eval Requirements Section](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/templates/T-20-prd-eval-requirements-section.md) · [T-21 Multimodal Eval Specification Template](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/templates/T-21-multimodal-eval-spec-template.md) · [T-22 DECODE Translation Worksheet](https://github.com/hsrivatsa/Multimodal-AI-Evaluations/blob/main/templates/T-22-decode-translation-worksheet.md)

*Version 2.1. Contributions and corrections welcome via PR.*

