# PRD Eval Requirements Section Template

**Repository:** `abhixz13/Multimodal-AI-Evaluations`  
**File:** `templates/prd-eval-requirements-section.md`  
**Parent Document:** Multimodal AI Evals Speak (v2.1), Section 20.2  
**Usage:** Add this section to every AI feature PRD. It is the container for the Eval Stub and the governance commitments that make evaluation a first-class requirement — not an afterthought.

---

## How to Use This Template

1. Copy the section below (starting at the `---` divider) into your PRD after the functional requirements section.
2. Complete the Eval Stub using `templates/eval-stub-template.md` during the Draft PRD session.
3. Paste the completed stub into Section A below.
4. Populate Sections B and C at PRD Review or Approval.
5. Do not mark the PRD as Approved until all checkboxes in Section C are checked and Section D is signed off.

---

---

## Evaluation Requirements

> **Purpose of this section:** This section records the evaluation commitments for this AI feature. It is co-authored by the Product Owner and Eval Owner during or immediately after the feature requirement authoring session. It must reach "Complete" status before the feature moves to development.
>
> **Status:** ☐ Not Started  ☐ Eval Stub Draft  ☐ Eval Stub Complete  ☐ Eval Spec Initiated  ☐ Eval Spec Approved

---

### A. Eval Stub

*Paste the completed Eval Stub here. Use `templates/eval-stub-template.md`. Minimum viable state at Draft PRD stage: Sections 1, 2, and 3 complete. All sections required at Approved stage.*

[PASTE COMPLETED EVAL STUB HERE]

---

### B. Eval Specification Reference

The full Eval Specification is the detailed, executable version of the Eval Stub. It is authored by the Eval Owner using `templates/multimodal-eval-spec-template.md` and stored in the `abhixz13/Multimodal-AI-Evaluations` repository.

| Field | Value |
|---|---|
| **Full Eval Spec ID** | EVAL-[YYYY]-[NNN] — or "TBD" at Draft stage |
| **Eval Spec location** | [Link to file in Multimodal-AI-Evaluations repo] |
| **Eval Spec status** | Not yet initiated / In progress / In review / Approved / Active |
| **Target completion date** | [Date — must precede development sprint start] |
| **Eval Owner** | [Name] |

> **Requirement:** The Eval Spec must reach Approved status before the first development sprint begins. A PRD that enters development with an unapproved or non-existent Eval Spec is a governance failure.

---

### C. Evaluation Commitments

The following commitments are made at PRD Approval time. Both the Product Owner and Eval Owner are responsible for ensuring these commitments are upheld throughout development.

**Authoring commitments:**
- [ ] Eval Stub is complete (all 7 sections) and signed off by both Product Owner and Eval Owner
- [ ] Full Eval Spec will be completed and approved before development begins
- [ ] Eval Spec was authored concurrently with, not after, this PRD

**Governance commitments:**
- [ ] Launch gate thresholds are pre-agreed and documented in the Eval Spec before development begins — not negotiated at launch review time
- [ ] Safety is included as a P0 launch gate with a zero-tolerance adversarial test set, regardless of feature type
- [ ] Production monitoring specification is included in the Eval Spec — not deferred to post-launch

**Change management commitments:**
- [ ] Any change to feature scope after PRD Approval requires a corresponding PR to the Eval Spec before development on that scope begins
- [ ] Eval Spec thresholds are reviewed at quarterly cadence after launch — not left at initial values indefinitely

---

### D. Launch Gate Summary (Executive View)

*For executive and leadership visibility at launch review. Copy the P0 gate summary from Eval Stub Section 4. This feature will not ship unless all P0 thresholds in the full Eval Spec are met.*

| Dimension | Metric | Minimum Threshold | Status at Launch Review |
|---|---|---|---|
| [From Eval Stub Section 4] | | | ☐ Pass  ☐ Fail  ☐ Pending |
| [From Eval Stub Section 4] | | | ☐ Pass  ☐ Fail  ☐ Pending |
| Safety | Zero violations on adversarial test set | 100% | ☐ Pass  ☐ Fail  ☐ Pending |

**Launch recommendation:** [To be populated at launch review — LAUNCH READY / LIMITED LAUNCH (scope: ___) / HOLD (reason: ___)]

---

*Part of the `abhixz13/Multimodal-AI-Evaluations` template library.*  
*Parent framework: Multimodal AI Evals Speak v2.1, Section 20.2*  
*See also: `templates/eval-stub-template.md` · `templates/multimodal-eval-spec-template.md` · `worksheets/decode-translation-worksheet.md`*
