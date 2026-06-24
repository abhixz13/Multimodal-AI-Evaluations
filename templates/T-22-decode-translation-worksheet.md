# DECODE Translation Worksheet

**Repository:** Multimodal-AI-Evaluations  
**File:** `worksheets/decode-translation-worksheet.md`  
**Parent Document:** Multimodal AI Evals Speak (v2.1), Section 6 and Section 20.3  
**Usage:** Use this worksheet during the 90-minute Draft PRD Session. PM and Eval Engineer work through it together. Output feeds directly into the Eval Stub template. Print double-sided, or work in a shared doc.

---

## Session Setup

**Feature being evaluated:** _______________________________________________

**Date:** _______________  **PM:** _______________  **Eval Engineer:** _______________

**Source requirement (paste verbatim):**

```
[Paste the exact text of the feature requirement here before the session begins]
```

**Session agenda reminder:**

| Time | Activity | Output |
|---|---|---|
| 0:00–0:15 | Read requirement aloud; flag compressed phrases using Vocabulary Dictionary | Flagged phrase list |
| 0:15–0:35 | Step D — Decompose into atomic claims | Numbered claim list |
| 0:35–0:55 | Step E — Enumerate modalities and slices | Modality inventory + high-risk slice |
| 0:55–1:15 | Step C (partial) — Assign contracts and name failure modes | Eval Stub Sections 1–3 |
| 1:15–1:30 | Complete Eval Stub Sections 4–6; sign off; assign Eval Owner | Completed Eval Stub |

---

## Phase 1 — Compressed Phrase Scan (0:00–0:15)

Before decomposing, identify every phrase in the requirement that belongs in the Vocabulary Mapping Dictionary. These are phrases that look like requirements but hide multiple eval primitives.

**Common compressed phrases to watch for:**

| If the requirement says... | Ask before proceeding... |
|---|---|
| "should be accurate" | Accurate at what? Against which reference? |
| "should not hallucinate" | What content? In what context? |
| "should understand images" | Understand what — detection, OCR, layout, semantics, intent? |
| "should feel grounded" | Grounded in the input or in world knowledge? |
| "should handle edge cases" | Which edges specifically? |
| "should be consistent" | Within a session? Across users? Across modalities? |
| "should work for documents" | Which formats, sizes, languages, quality levels? |
| "should communicate uncertainty" | How expressed? Calibrated against what? |

**Flagged phrases in this requirement:**

| Phrase | Question to resolve | Resolution |
|---|---|---|
| | | |
| | | |
| | | |

> **Rule:** Do not proceed to Step D until every flagged phrase has a resolution. Unresolved compressions produce under-specified atomic claims.

---

## Step D — Decompose (0:15–0:35)

**Atomicity test:** Each claim should be answerable as "the system did / did not do X" by examining a single output. If a claim requires interpretation or involves the word "and," decompose further.

**Decompose the requirement into atomic capability claims:**

| # | Atomic Claim | Source Phrase | Pass Atomicity Test? |
|---|---|---|---|
| 1 | | | ☐ Yes  ☐ Needs splitting |
| 2 | | | ☐ Yes  ☐ Needs splitting |
| 3 | | | ☐ Yes  ☐ Needs splitting |
| 4 | | | ☐ Yes  ☐ Needs splitting |
| 5 | | | ☐ Yes  ☐ Needs splitting |
| 6 | | | ☐ Yes  ☐ Needs splitting |
| 7 | | | ☐ Yes  ☐ Needs splitting |
| 8 | | | ☐ Yes  ☐ Needs splitting |

**Step D checkpoint (before moving to Step E):**
- [ ] Every claim can be answered "did / did not" from a single output
- [ ] No claim contains "and" connecting two distinct behaviors
- [ ] No claim is still stated in PRD Speak (e.g., "feels accurate" rather than "produces no fabricated claims")
- [ ] PM has confirmed the list reflects feature intent

**PM sign-off on claim list:** _________________ Date: _________

---

## Step E — Enumerate (0:35–0:55)

### Part 1 — Modality Inventory

For each modality, mark whether it appears in the **input** pipeline, the **output** pipeline, or **intermediate processing**. Every checked modality requires at least one eval dimension in the full Eval Spec.

| Modality | Input? | Processing? | Output? | Notes |
|---|---|---|---|---|
| Text (typed, OCR-extracted, transcribed) | ☐ | ☐ | ☐ | |
| Image (photos, diagrams, charts, screenshots, scans) | ☐ | ☐ | ☐ | |
| Audio (voice, recordings, ambient sound) | ☐ | ☐ | ☐ | |
| Video (recordings, screen captures) | ☐ | ☐ | ☐ | |
| Document (PDF, DOCX, PPTX as primary input) | ☐ | ☐ | ☐ | |
| Structured data (JSON, CSV, tables, forms) | ☐ | ☐ | ☐ | |
| Cross-modal synthesis (output references multiple input modalities) | N/A | ☐ | ☐ | |

**Modality coverage check:** For any modality checked above, confirm at least one atomic claim from Step D implicates it. Any modality with no corresponding claim is a specification gap — add a claim or flag for discussion.

### Part 2 — Input Slice Inventory

Define the dimensions along which inputs will vary in production. Use these to build the slice grid in the full Eval Spec.

| Slice Dimension | Values in Scope | High-Risk Value | Defer? |
|---|---|---|---|
| Modality presence | Text only / Image only / Audio only / Text+Image / Text+Audio / Full multimodal | | ☐ |
| Input quality | Pristine / Lightly degraded / Heavily degraded / Partial / Adversarial | | ☐ |
| Content domain | [List the specific domains this feature must serve] | | ☐ |
| User intent | Informational / Transactional / Exploratory / Compliance / Adversarial | | ☐ |
| Length / duration | Short / Medium / Long | | ☐ |
| Language / locale | [If applicable] | | ☐ |
| Sensitive subgroups | Accessibility / Demographic / Regulatory | | ☐ |

**High-risk slice (one sentence — this goes into Eval Stub Section 5):**

> The input condition most likely to cause a production failure that a standard, clean test set would miss:

_______________________________________________________________________________

**Known out-of-scope conditions (these go into Eval Stub Section 6):**

| Condition | Reason |
|---|---|
| | |
| | |

---

## Step C — Classify (0:55–1:15)

### Part 1 — Pipeline Stage Assignment

For each atomic claim from Step D, mark which pipeline stage(s) it implicates. A claim implicating 4+ stages should be returned to Step D for further decomposition.

| Claim # | Ingestion | Detection | Preprocessing | Chunking | Embedding | Retrieval | Generation | Formatting | Delivery |
|---|---|---|---|---|---|---|---|---|---|
| 1 | | | | | | | | | |
| 2 | | | | | | | | | |
| 3 | | | | | | | | | |
| 4 | | | | | | | | | |
| 5 | | | | | | | | | |
| 6 | | | | | | | | | |

### Part 2 — Quality Contract Assignment

For each claim, which quality contract(s) does it trigger? Every claim must land on at least one. If a claim cannot be assigned, it is out of scope, under-decomposed, or aspirational.

| Claim # | Ingestion Fidelity | Faithfulness | Factuality | Cross-Modal Consistency | Output Quality | Cannot assign — reason: |
|---|---|---|---|---|---|---|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |
| 6 | | | | | | |

**Unassigned claims:** Any claim that cannot be assigned to a contract must be resolved before sign-off:
- **Out of scope:** Remove from the claim list and add to Eval Stub Section 6
- **Under-decomposed:** Return to Step D and split further
- **Aspirational:** Document as a desired future capability, not a current eval requirement

### Part 3 — Top 3 Failure Modes

From the claim-contract assignments above, identify the three most consequential failure modes. These seed Eval Stub Section 3 and the full Eval Spec's Failure Mode Register.

**Failure mode atomicity test:** A reviewer given only the system input and the system output can score this failure without asking the team. If "no" or "maybe," rewrite.

| Rank | Failure Mode (concrete, observable) | Contract | Priority | Pass Atomicity Test? |
|---|---|---|---|---|
| 1 | | | P0 / P1 / P2 | ☐ Yes  ☐ Rewrite |
| 2 | | | P0 / P1 / P2 | ☐ Yes  ☐ Rewrite |
| 3 | | | P0 / P1 / P2 | ☐ Yes  ☐ Rewrite |

---

## Session Output Checklist

Before ending the session, confirm the following outputs are complete:

**Eval Stub:**
- [ ] Section 1 (Quality Contracts) — completed from Step C Part 2
- [ ] Section 2 (Modalities) — completed from Step E Part 1
- [ ] Section 3 (Top Failure Modes) — completed from Step C Part 3
- [ ] Section 4 (P0 Gates) — populated if thresholds known; placeholder if not
- [ ] Section 5 (High-Risk Slice) — completed from Step E Part 2
- [ ] Section 6 (Out-of-Scope) — completed from Step E Part 2
- [ ] Section 7 (Sign-Off) — both parties sign

**Assignments:**
- [ ] Eval Owner named and assigned to complete full Eval Spec
- [ ] Target Eval Spec completion date agreed (must precede development sprint start)
- [ ] Eval Stub pasted into PRD Evaluation Requirements section by PM before next session

**DECODE Progress tracker (update in Eval Stub):**
- [x] D — Decompose *(completed this session)*
- [x] E — Enumerate *(completed this session)*
- [x] C — Classify *(partially completed this session)*
- [ ] O — Operationalize *(Eval Owner completes in full Eval Spec)*
- [ ] D — Define *(Eval Owner completes in full Eval Spec)*
- [ ] E — Express *(Eval Owner completes in full Eval Spec)*

---

## Quick Reference: Cross-Modal Failure Modes to Check

When any two or more modalities are checked in Step E, deliberately ask whether any of these cross-modal failures apply:

| Pattern | Question to Ask |
|---|---|
| **Modality override** | Could the system ignore one modality and answer from the other even when the ignored modality is the relevant one? |
| **Hallucinated synthesis** | Could the system fabricate a connection between text and image content that doesn't actually exist? |
| **Modality confidence asymmetry** | Could the system present uncertain content from one modality with the same confidence as certain content from another? |
| **Phantom reference** | Could the system refer to an image, chart, or audio element that is not actually present in the input? |
| **Temporal-spatial confusion** | For video: could the system attribute content to the wrong time or the wrong speaker? |

If any answer is "yes" or "possibly," add a corresponding failure mode in Step C Part 3.

---

*Part of the Multimodal-AI-Evaluations template library.*  
*Parent framework: Multimodal AI Evals Speak v2.1, Sections 6 and 20.3*  
*See also: `templates/eval-stub-template.md` · `templates/prd-eval-requirements-section.md` · `templates/multimodal-eval-spec-template.md`*
