# Eval Stub Template

**Repository:** `abhixz13/Multimodal-AI-Evaluations`  
**File:** `templates/eval-stub-template.md`  
**Parent Document:** Multimodal AI Evals Speak (v2.1), Section 20.1  
**Usage:** Copy this template into the Evaluation Requirements section of every AI feature PRD. Complete during the Draft PRD session alongside the feature requirement. See the Concurrent Authoring Protocol (Section 20.3) for session structure and time budget.

---

## When to Use This Template

- Every AI feature PRD that involves an AI system processing or producing content
- Must be completed before the feature moves from Draft PRD to PRD Review
- Minimum viable completion (for Draft stage): Sections 1, 2, and 3
- Full completion required (for PRD Approved stage): All sections, both sign-offs

## Who Completes It

- **Sections 1–3:** PM and Eval Engineer jointly in the Draft PRD session (60–90 min)
- **Section 4:** PM and Eval Engineer at PRD Review (30 min); placeholder thresholds acceptable at Draft stage
- **Sections 5–6:** PM, with Eval Engineer review
- **Section 7:** Both owners at PRD Review or Approval

---

## Eval Stub: [Feature Name]

**Stub ID:** STUB-[YYYY]-[NNN]  
**PRD Reference:** [PRD-ID]  
**Full Eval Spec Reference:** [EVAL-ID — or "TBD: to be authored by Eval Owner before development start"]  
**Eval Owner:** [Name — or "TBD: assign before PRD Review"]  
**DECODE Progress:** [ ] D  [ ] E  [ ] C  [ ] O  [ ] D  [ ] E  
**Stub Status:** Draft | Complete | Eval Spec Initiated | Eval Spec Approved

> **DECODE Progress key:** Check each letter as the corresponding step is completed. D (Decompose) and E (Enumerate) and C (Classify) are completed in the PRD session. O (Operationalize), D (Define), and E (Express) are completed by the Eval Owner in the full Eval Spec.

---

### 1. Quality Contracts Triggered

Mark all that apply. **At least one required** before this stub is considered complete at Draft stage.

- [ ] **Ingestion Fidelity** — system must correctly parse or extract source content (text, images, audio, video, documents)
- [ ] **Faithfulness** — outputs must be grounded in provided source material and not fabricate claims
- [ ] **Factuality** — outputs must be correct with respect to verifiable ground truth
- [ ] **Cross-Modal Consistency** — outputs must be coherent and non-contradictory across different input modalities
- [ ] **Output Quality** — outputs must meet format, tone, completeness, and audience-appropriateness standards
- [ ] **Safety** — outputs must comply with policy, compliance, or harm-avoidance requirements *(governance overlay — check this on every feature regardless of which quality contracts above are triggered)*

**Primary contract:** [Name the single most critical contract for this feature — the one whose failure would most directly harm users or the business]

> **Quick trigger guide:**
> - Any mention of file uploads, input formats, OCR, or transcription → Ingestion Fidelity
> - Any mention of "based on our data," "don't make things up," or document-grounded answers → Faithfulness
> - Any mention of accuracy, correctness, or domain-specific facts → Factuality
> - Any mention of voice + text, image + text, or consistency across channels → Cross-Modal Consistency
> - Any mention of tone, brand voice, format, or response length → Output Quality

---

### 2. Modalities Involved

Mark all modalities the system processes as **input** or produces as **output**. For each checked modality, the full Eval Spec must include at least one eval dimension.

**Input modalities:**
- [ ] Text (typed queries, uploaded text files)
- [ ] Image (photographs, diagrams, charts, screenshots, scanned documents)
- [ ] Audio (voice commands, uploaded audio recordings)
- [ ] Video (uploaded recordings, screen captures)
- [ ] Document (PDF, DOCX, PPTX — as primary input type)
- [ ] Structured Data (JSON payloads, CSV files, database exports, form submissions)

**Output modalities:**
- [ ] Text (natural language answers, summaries, narratives)
- [ ] Structured output (JSON, tables, filled forms)
- [ ] Cross-modal output (text output that references or synthesizes multiple input modalities)

> **Coverage rule:** Every checked modality above requires at least one eval dimension in the full Eval Spec. Any modality left unchecked that the system actually processes is a specification gap.

---

### 3. Top Failure Modes

List the three most likely or most consequential ways quality can break for this feature. These become the seed entries in the full Eval Spec's Failure Mode Register (DECODE Step C).

| # | Failure Mode | Contract | Priority |
|---|---|---|---|
| 1 | [Describe concretely — what specific wrong thing does the system produce?] | [Contract name] | P0 / P1 / P2 |
| 2 | [Describe concretely] | | P0 / P1 / P2 |
| 3 | [Describe concretely] | | P0 / P1 / P2 |

**Atomicity test for each failure mode:** A reviewer given only the system input and the system output can determine whether this failure occurred — without asking the original team. If the answer is "no" or "maybe," rewrite the failure mode until it passes.

**Examples of concrete failure modes:**
- ✅ "System generates an answer citing a clause number not present in the uploaded lease PDF"
- ✅ "System transcribes 'valve pressure: 40 PSI' as 'valve pressure: 14 PSI' in a noisy environment"
- ✅ "System describes chart content as 'revenue increased Q3' when the chart shows Q3 decline"
- ❌ "System hallucinates" — too abstract; which content? in which context? scored how?
- ❌ "System gives wrong answer" — not a failure mode; it is a symptom

---

### 4. P0 Launch Gate Summary

List only the thresholds whose failure would block launch. These are ship gates — hard, pre-agreed, non-negotiable. Leave blank at Draft stage if thresholds are not yet known; the Eval Owner will populate them in the full Eval Spec.

| Dimension | Metric | Minimum Threshold |
|---|---|---|
| [Dimension name] | [Named metric] | [≥ or ≤ value] |
| [Dimension name] | [Named metric] | [≥ or ≤ value] |
| [Dimension name] | [Named metric] | [≥ or ≤ value] |
| Safety | Zero violations on adversarial test set | 100% *(always P0 — never leave blank)* |

> **Threshold guidance:** If you cannot justify a specific number, write a placeholder with a note ("≥ 0.90 — to be calibrated on first eval run"). A placeholder threshold that both owners acknowledge is better than a blank. Do not use round numbers without justification.

---

### 5. High-Risk Slice

Describe in **one sentence** the input condition most likely to cause a production failure that a standard, clean test set would miss. This is the slice that must receive explicit coverage in the full Eval Spec — not a deferred one.

> **Examples:**
> - "Multi-column PDFs where primary data is encoded in charts rather than text tables."
> - "Voice inputs from field technicians recorded in ambient industrial noise above 70dB."
> - "Customer support photos taken on low-end phones in poor indoor lighting."
> - "Lease documents that are photocopied scans of physical paper, not native digital PDFs."

[Your high-risk slice:]

---

### 6. Known Out-of-Scope Conditions

List any input types, use cases, user populations, or languages this feature explicitly does **NOT** cover for this release. Silence is not exclusion — anything not listed here will be assumed in scope by the Eval Owner when building the full eval.

| Out-of-Scope Condition | Reason / Disposition |
|---|---|
| [Condition] | [Why excluded; when it will be addressed if known] |
| [Condition] | |

---

### 7. Eval Stub Sign-Off

Both signatures required before the PRD advances to Approved status.

| Role | Name | Date | Notes |
|---|---|---|---|
| Product Owner | | | |
| Eval Owner | | | |

**What sign-off means:**
- **Product Owner confirms:** The atomic claims, failure modes, and out-of-scope conditions correctly reflect feature intent.
- **Eval Owner confirms:** The stub contains sufficient information to begin DECODE Steps O and D in the full Eval Spec without a follow-up translation session.

---

*Part of the `abhixz13/Multimodal-AI-Evaluations` template library.*  
*Parent framework: Multimodal AI Evals Speak v2.1, Section 20.1*  
*See also: `templates/prd-eval-requirements-section.md` · `templates/multimodal-eval-spec-template.md` · `worksheets/decode-translation-worksheet.md`*
