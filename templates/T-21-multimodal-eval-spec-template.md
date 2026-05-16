# Multimodal Eval Specification Template

**Repository:** `hsrivatsa/Multimodal-AI-Evaluations`  
**File:** `templates/multimodal-eval-spec-template.md`  
**Parent Document:** Multimodal AI Evals Speak (v2.1), Section 11  
**Usage:** Use this template to produce a formal Eval Specification for any AI feature. Authored by the Eval Owner, building from the completed Eval Stub in the PRD. Must be approved before development begins.

---

## Authoring Instructions

**Input:** A completed Eval Stub from the PRD (Sections 1–7 populated and signed off).  
**Process:** Complete DECODE Steps O and D using this template, then finalize Step E by reviewing and approving the completed document.  
**Owner:** Eval Engineer (with Product Owner review at Sections 6, 7, and 12).  
**Timeline:** 1–2 weeks from Eval Stub sign-off to Eval Spec approval.

**Section mapping from Eval Stub to this spec:**
- Eval Stub Section 1 (Contracts) → Spec Sections 4 and 5
- Eval Stub Section 2 (Modalities) → Spec Section 4
- Eval Stub Section 3 (Failure Modes) → Spec Section 6 (expand to full register)
- Eval Stub Section 4 (P0 Gates) → Spec Section 11 (expand with full threshold definitions)
- Eval Stub Section 5 (High-Risk Slice) → Spec Section 9 (first entry in slice grid)
- Eval Stub Section 6 (Out-of-Scope) → Spec Section 2

---

# Multimodal Eval Specification: [Feature Name]

**Spec ID:** EVAL-[YYYY]-[NNN]  
**Version:** 1.0  
**Status:** Draft | In Review | Approved | Active | Deprecated  
**PRD Reference:** [PRD-ID]  
**Eval Stub Reference:** STUB-[YYYY]-[NNN]  
**Product Owner:** [Name]  
**Eval Owner:** [Name]  
**Reviewers:** [Names]  
**Approved By:** [Name]  
**Approval Date:** [Date]  
**Last Updated:** [Date]

---

## 1. Intent Statement

*One paragraph in PRD Speak, quoted or paraphrased from the feature requirement in the PRD. This is the only place PRD Speak is permitted in this document.*

[Paste or paraphrase the feature requirement here]

---

## 2. Scope and Non-Scope

### In Scope
[What this evaluation explicitly covers — input types, user populations, use cases, modalities, languages]

### Out of Scope
*Transfer directly from Eval Stub Section 6. Silence is not exclusion.*

| Out-of-Scope Condition | Reason | Disposition |
|---|---|---|
| [From Eval Stub] | | [Deferred to vN / Will not evaluate / Separate spec] |

---

## 3. Atomic Capability Claims

*Transfer and expand from the decomposition work done in DECODE Step D during the PRD session. Each claim must be answerable as "did / did not" from a single output. Confirm list with Product Owner.*

| # | Atomic Claim | Source Phrase in Requirement | PRD Owner Confirmed |
|---|---|---|---|
| 1 | | | ☐ |
| 2 | | | ☐ |
| 3 | | | ☐ |

---

## 4. Pipeline Stage Implication Matrix

*For each atomic claim, mark which of the nine pipeline stages it implicates. A claim implicating 4+ stages may be under-decomposed — return to Section 3.*

| Claim # | Ingestion (1) | Detection (2) | Preprocessing (3) | Chunking (4) | Embedding (5) | Retrieval (6) | Generation (7) | Formatting (8) | Delivery (9) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | | | | | | | | | |
| 2 | | | | | | | | | |
| 3 | | | | | | | | | |

---

## 5. Quality Contract Assignments

*Every claim must land on at least one contract. A claim that cannot be assigned is out of scope, under-decomposed, or aspirational — all three are useful findings.*

| Claim # | Ingestion Fidelity | Faithfulness | Factuality | Cross-Modal Consistency | Output Quality |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

**Safety note:** Safety adversarial coverage is required on this specification regardless of which contracts are checked above.

---

## 6. Failure Mode Register

*Expand the three seed failure modes from Eval Stub Section 3. Add additional failure modes from the full DECODE Step C analysis. Each failure mode must pass the atomicity test: a reviewer given only input and output can score it without team consultation.*

| # | Failure Mode (concrete, observable) | Quality Contract | Pipeline Stage | Priority | Example |
|---|---|---|---|---|---|
| 1 | | | | P0 / P1 / P2 | |
| 2 | | | | P0 / P1 / P2 | |
| 3 | | | | P0 / P1 / P2 | |
| 4 | | | | P0 / P1 / P2 | |
| 5 | | | | P0 / P1 / P2 | |

*Add rows as needed. Aim for completeness on P0 failure modes; P1 and P2 can be added iteratively.*

---

## 7. Measurement Primitives

*For each failure mode in Section 6, define the five measurement primitives. Never anonymize the judge. Never leave threshold blank for P0 failure modes.*

| Failure Mode # | Metric Name | Definition | Measurement Method | Judge (named, with rubric version) | Dataset | Launch Threshold | Regression Threshold | Failure Consequence | Priority |
|---|---|---|---|---|---|---|---|---|---|
| 1 | | | Automated / LLM-Judge / Human / Hybrid | | | | | | P0 |
| 2 | | | | | | | | | |
| 3 | | | | | | | | | |

**Operationalization template (use for each row):**

```
Metric Name: [Name]
Definition: [Precise definition — what exactly is being measured]
Measurement Method: [Automated / LLM-as-Judge / Human Annotator / Hybrid]
Judge: [Full judge specification — model, version, rubric version, temperature, run count, voting rule]
Scoring Range: [0–1 / 0–100 / Boolean / Categorical]
Dataset: [Source, size, slices covered, refresh policy]
Ground Truth Required: [Yes / No — if yes, what form and source]
Launch Threshold: [Minimum score to ship — justify the number]
Regression Threshold: [Score below which production alert fires]
Failure Consequence: [What breaks for user or business if this fails]
Priority: [P0 — blocking / P1 — high / P2 — tracked]
```

---

## 8. Judge Panel Composition

*Name every judge used in this eval. No anonymous scores. Rubrics must be linked documents, not inline summaries.*

| Judge ID | Model or Role | Rubric Reference | Failure Modes Covered | Agreement Target | Notes |
|---|---|---|---|---|---|
| J1 | [e.g., Claude Sonnet 4 with rubric v1.0] | [link] | [FM #s] | ≥ 0.80 judge consistency | |
| J2 | [e.g., Human annotator panel — 3 reviewers] | [link] | [FM #s] | Cohen's κ ≥ 0.70 | |
| J3 | [e.g., Deterministic extraction check] | [link] | [FM #s] | N/A | |

**Judge panel principles:**
- Do not use an LLM judge for failure modes that LLMs share with the system under test (e.g., phantom visual references, modality hallucinations)
- Human judges must cover all P0 failure modes involving visual or audio grounding claims, even if only as a spot-check sample
- At least one non-LLM judge must appear in every panel

---

## 9. Input Slice Grid

*The first row must come from Eval Stub Section 5 (High-Risk Slice). All other slices follow from DECODE Step E. Mark each cell: ✅ Covered, ⚠️ Sampled (10–20%), ❌ Deferred, N/A.*

| Slice Dimension | Slice Values | Coverage Plan | Deferred? | Reason for Deferral |
|---|---|---|---|---|
| **Modality presence** | Text only / Image only / Audio only / Text+Image / Text+Audio / Full multimodal | | | |
| **Input quality** | Pristine / Lightly degraded / Heavily degraded / Partial / Adversarial | | | |
| **Content domain** | [List domains specific to this feature] | | | |
| **User intent** | Informational / Transactional / Exploratory / Compliance / Adversarial | | | |
| **Length / duration** | Short / Medium / Long | | | |
| **Language / locale** | [If applicable] | | | |
| **Sensitive subgroups** | Accessibility / Demographic / Regulatory | | | |
| **[High-risk slice from Eval Stub]** | [From Section 5] | ✅ Must be covered | No | Identified as highest-risk at PRD stage |

**Coverage rule:** Any slice marked ❌ Deferred for a modality the system actively uses must be documented as a Known Limitation in Section 12.

---

## 10. Dataset Manifest

| Dataset ID | Purpose | Source | Size | Slices Covered | Refresh Policy | Contamination Policy | Annotation Method |
|---|---|---|---|---|---|---|---|
| DS-01 | Core representative cases | [Source] | [N] | [Slices] | [Cadence] | [Policy] | [Method] |
| DS-02 | Edge cases | | | | | | |
| DS-03 | Adversarial cases (safety) | | | | | | |
| DS-04 | Regression cases (historical failures) | | | | | | |
| DS-05 | High-risk slice cases | | | | | | |

**Ground truth specification:**

| Failure Mode # | Ground Truth Type | Source | Annotation Method | Annotator Qualification | Inter-Annotator Agreement Target |
|---|---|---|---|---|---|
| | Reference answer / Binary label / Score / Schema | | | | Cohen's κ ≥ [X] |

---

## 11. Gate Register

```
LAUNCH GATE: [Feature Name]
Spec ID: EVAL-[YYYY]-[NNN]
All P0 thresholds must be met before this feature ships to production.
Any single P0 failure results in a HOLD recommendation.

REQUIRED — P0 (blocking; failure = HOLD, escalate immediately):
  [ ] [Failure Mode #1]: [metric] ≥ [threshold] — Judge: [J#] — Dataset: [DS-#]
  [ ] [Failure Mode #2]: [metric] ≥ [threshold] — Judge: [J#] — Dataset: [DS-#]
  [ ] Safety: Zero violations on adversarial test set — Judge: [J#] — Dataset: [DS-#]

RECOMMENDED — P1 (high priority; failure = limited launch with scope restriction):
  [ ] [Failure Mode #]: [metric] ≥ [threshold] — Judge: [J#] — Dataset: [DS-#]
  [ ] [Failure Mode #]: [metric] ≥ [threshold] — Judge: [J#] — Dataset: [DS-#]

TRACKED — P2 (observed, not gated; used for trend awareness):
  [ ] [Failure Mode #]: [metric] — trend watch — Dashboard: [link]

LAUNCH RECOMMENDATION LOGIC:
  All P0 and P1 thresholds met     → Full launch
  All P0 met, one or more P1 gaps  → Limited launch with scope restriction + P1 roadmap item
  Any P0 threshold missed          → HOLD — do not ship — fix and re-evaluate
  Any safety failure               → HOLD — escalate immediately to [owner]

GATE OWNERSHIP:
  Gate owner: [Name]
  Review cadence: [Quarterly / Per-release / Real-time]
  Deprecation policy: [When gates are retired or replaced]
```

---

## 12. Known Limitations

*This section is not optional. Every eval suite has detection gaps. Documenting them honestly is what distinguishes a spec that governs from a spec that provides false assurance.*

| Limitation | What the Eval Cannot Detect | Impact if Exploited | Mitigation Plan |
|---|---|---|---|
| [Limitation] | [Specific gap in coverage] | [Risk to user or business] | [What partially compensates] |
| All deferred slices from Section 9 | Failures specific to those input conditions | Unknown until those slices are evaluated | [Target date for coverage] |

---

## 13. Production Monitoring Specification

*An eval suite with no production monitoring is a launch-gate-only instrument. It catches failures before launch but cannot detect drift or regression after.*

| Dimension | Monitoring Method | Alert Threshold | Alert Owner | Response SLA | Escalation Path |
|---|---|---|---|---|---|
| [Dimension] | [LLM-Judge pipeline / Deterministic check / Human sample] | [Threshold] | [Owner] | [SLA] | [Escalation] |
| Safety | Adversarial probe pipeline — daily | Any failure | [Owner] | Immediate | [Escalation] |

**Monitoring cadence:** [Real-time / Hourly / Daily / Per-release]  
**Dashboard location:** [Link]  
**Incident response runbook:** [Link]

---

## 14. Stakeholder Readout Template

*Populate this at launch review time using actual eval results. Lead with the verdict. Every stakeholder row is one sentence.*

**Verdict:** [LAUNCH READY / LIMITED LAUNCH — scope restriction: ___ / HOLD — reason: ___]

| Stakeholder | One-Sentence Quality Summary |
|---|---|
| CEO / CPO | |
| CFO | |
| Legal / CLO | |
| CTO | |
| Head of Sales | |
| Head of Support | |

**Where quality is strong:** [2–3 sentences on passing P0 dimensions — specific, not generic]

**Where quality breaks:** [2–3 sentences on failing or at-risk dimensions — specific, not hedged]

**Recommended decision:** [Action with supporting evidence]

**Decision required from stakeholder:** [What you need — scope authorization, launch approval, budget, timeline]

---

## 15. Change Log

| Version | Date | Author | Summary of Change |
|---|---|---|---|
| 1.0 | [Date] | [Author] | Initial specification |

---

*Part of the `hsrivatsa/Multimodal-AI-Evaluations` template library.*  
*Parent framework: Multimodal AI Evals Speak v2.1, Section 11*  
*See also: `templates/eval-stub-template.md` · `templates/prd-eval-requirements-section.md` · `worksheets/decode-translation-worksheet.md`*
