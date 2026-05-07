# Multimodal AI Evaluations: Everything You Need to Know

 << Sharp opinions, not universal truths. Use your judgment. >>
> ## What this FAQ is

A practical FAQ for teams building, evaluating, and operating multimodal AI products.

## What this FAQ is not

It is not a benchmark survey, model leaderboard, academic literature review, or legal/compliance manual.

> ## Start Here

If you are new to multimodal evals, read Q1–Q14 first.

If you are building an eval system this week, read Q6, Q22, Q24, Q25, Q32, Q35, Q50.

If your product is already in production, read Q21, Q42, Q50–Q56.

If you work in a regulated domain, read Q44–Q48.

---

## Table of Contents

**[Foundations](#foundations)**

- [Q1. What are multimodal AI evaluations, and what makes them their own discipline?](#q1-what-are-multimodal-ai-evaluations-and-what-makes-them-their-own-discipline)
- [Q2. How are multimodal AI evaluations different from text-only LLM evals?](#q2-how-are-multimodal-ai-evaluations-different-from-text-only-llm-evals)
- [Q3. Why is "it looks good" or "it sounds good" not enough?](#q3-why-is-it-looks-good-or-it-sounds-good-not-enough)
- [Q4. What does "evaluation" actually mean in a production multimodal AI product?](#q4-what-does-evaluation-actually-mean-in-a-production-multimodal-ai-product)
- [Q5. What is a multimodal trace?](#q5-what-is-a-multimodal-trace)
- [Q6. What is the minimum viable evaluation setup?](#q6-what-is-the-minimum-viable-evaluation-setup)
- [Q7. Who should own multimodal AI evaluations?](#q7-who-should-own-multimodal-ai-evaluations)
- [Q8. How much effort should a team spend reading traces versus building automated evaluators?](#q8-how-much-effort-should-a-team-spend-reading-traces-versus-building-automated-evaluators)

**[The Five Quality Contracts](#the-five-quality-contracts)**

- [Q9. What are the Five Quality Contracts?](#q9-what-are-the-five-quality-contracts)
- [Q10. What is Ingestion Fidelity, and why does it matter before the model is involved?](#q10-what-is-ingestion-fidelity-and-why-does-it-matter-before-the-model-is-involved)
- [Q11. What is the difference between Faithfulness and Factuality, and can they fail independently?](#q11-what-is-the-difference-between-faithfulness-and-factuality-and-can-they-fail-independently)
- [Q12. What is Cross-Modal Consistency, and why does it not exist in text-only evals?](#q12-what-is-cross-modal-consistency-and-why-does-it-not-exist-in-text-only-evals)
- [Q13. What does Output Quality mean for audio, visual, video, and interactive outputs?](#q13-what-does-output-quality-mean-for-audio-visual-video-and-interactive-outputs)
- [Q14. Why is one overall "accuracy" score dangerous?](#q14-why-is-one-overall-accuracy-score-dangerous)

**[Failure Modes](#failure-modes)**

- [Q15. What are the most common failure modes in multimodal AI products?](#q15-what-are-the-most-common-failure-modes-in-multimodal-ai-products)
- [Q16. Why do so many multimodal failures originate in ingestion?](#q16-why-do-so-many-multimodal-failures-originate-in-ingestion)
- [Q17. What are common PDF, OCR, and document-parsing failures?](#q17-what-are-common-pdf-ocr-and-document-parsing-failures)
- [Q18. What are common ASR and audio-ingestion failures?](#q18-what-are-common-asr-and-audio-ingestion-failures)
- [Q19. What are common visual understanding failures?](#q19-what-are-common-visual-understanding-failures)
- [Q20. What are cross-modal contradiction failures, and why do they slip past per-modality evaluators?](#q20-what-are-cross-modal-contradiction-failures-and-why-do-they-slip-past-per-modality-evaluators)
- [Q21. What is the "quiet failure signature"?](#q21-what-is-the-quiet-failure-signature)

**[Evaluation Design](#evaluation-design)**

- [Q22. How do I write an Evaluation Brief?](#q22-how-do-i-write-an-evaluation-brief)
- [Q23. How do I define "good" before seeing evaluation scores?](#q23-how-do-i-define-good-before-seeing-evaluation-scores)
- [Q24. Should multimodal evals use binary pass/fail or 1–5 Likert scores?](#q24-should-multimodal-evals-use-binary-passfail-or-15-likert-scores)
- [Q25. When should I use human judges versus LLM-as-a-Judge?](#q25-when-should-i-use-human-judges-versus-llm-as-a-judge)
- [Q26. What is a Dictator Panel?](#q26-what-is-a-dictator-panel)
- [Q27. Should I use a single multimodal judge or specialized per-modality judges?](#q27-should-i-use-a-single-multimodal-judge-or-specialized-per-modality-judges)
- [Q28. Where do off-the-shelf multimodal models break down as judges?](#q28-where-do-off-the-shelf-multimodal-models-break-down-as-judges)
- [Q29. How do I calibrate an LLM judge using Cohen's kappa?](#q29-how-do-i-calibrate-an-llm-judge-using-cohens-kappa)
- [Q30. How should I design synthetic test cases for multimodal AI products?](#q30-how-should-i-design-synthetic-test-cases-for-multimodal-ai-products)
- [Q31. How do I evaluate subjective qualities like pacing or engaging narration?](#q31-how-do-i-evaluate-subjective-qualities-like-pacing-or-engaging-narration)

**[Pipeline Diagnosis](#pipeline-diagnosis)**

- [Q32. What is the nine-stage multimodal AI pipeline?](#q32-what-is-the-nine-stage-multimodal-ai-pipeline)
- [Q33. Why evaluate the pipeline instead of only the final output?](#q33-why-evaluate-the-pipeline-instead-of-only-the-final-output)
- [Q34. How do I trace a failure back to its origin stage using causal chain analysis?](#q34-how-do-i-trace-a-failure-back-to-its-origin-stage-using-causal-chain-analysis)
- [Q35. What should I log at each pipeline stage?](#q35-what-should-i-log-at-each-pipeline-stage)
- [Q36. How do I tell if a downstream failure is actually an ingestion failure in disguise?](#q36-how-do-i-tell-if-a-downstream-failure-is-actually-an-ingestion-failure-in-disguise)
- [Q37. What are the thirteen fix surfaces in the Optimization Action Library?](#q37-what-are-the-thirteen-fix-surfaces-in-the-optimization-action-library)
- [Q38. How do I run driver analysis when a failure could be caused by any of three modalities?](#q38-how-do-i-run-driver-analysis-when-a-failure-could-be-caused-by-any-of-three-modalities)

**[Tooling & Infrastructure](#tooling--infrastructure)**

- [Q39. Can I run multimodal evaluation no-code in Braintrust?](#q39-can-i-run-multimodal-evaluation-no-code-in-braintrust)
- [Q40. What custom annotation tooling do I need for audio review?](#q40-what-custom-annotation-tooling-do-i-need-for-audio-review)
- [Q41. What custom annotation tooling do I need for video review?](#q41-what-custom-annotation-tooling-do-i-need-for-video-review)
- [Q42. How do I handle production trace storage for audio and video?](#q42-how-do-i-handle-production-trace-storage-for-audio-and-video)
- [Q43. What gaps in eval tooling should I expect to fill myself in 2026?](#q43-what-gaps-in-eval-tooling-should-i-expect-to-fill-myself-in-2026)

**[Domain Playbooks](#domain-playbooks)**

- [Q44. How should multimodal evaluations differ in healthcare?](#q44-how-should-multimodal-evaluations-differ-in-healthcare)
- [Q45. How should multimodal evaluations differ in legal and compliance-heavy domains?](#q45-how-should-multimodal-evaluations-differ-in-legal-and-compliance-heavy-domains)
- [Q46. How should multimodal evaluations differ for education and tutoring?](#q46-how-should-multimodal-evaluations-differ-for-education-and-tutoring)
- [Q47. How should multimodal evaluations differ for insurance, finance, and claims?](#q47-how-should-multimodal-evaluations-differ-for-insurance-finance-and-claims)
- [Q48. How should multimodal evaluations differ for robotics and physical AI?](#q48-how-should-multimodal-evaluations-differ-for-robotics-and-physical-ai)
- [Q49. How should multimodal evaluations differ for creative tools?](#q49-how-should-multimodal-evaluations-differ-for-creative-tools)

**[Production, Monitoring & Governance](#production-monitoring--governance)**

- [Q50. How do I move from offline evals to production monitoring?](#q50-how-do-i-move-from-offline-evals-to-production-monitoring)
- [Q51. What metrics should be monitored continuously?](#q51-what-metrics-should-be-monitored-continuously)
- [Q52. How do I detect evaluator drift or judge drift?](#q52-how-do-i-detect-evaluator-drift-or-judge-drift)
- [Q53. How should multimodal evals integrate into CI/CD and release gates?](#q53-how-should-multimodal-evals-integrate-into-cicd-and-release-gates)
- [Q54. What does a progressive rollout gate waterfall look like?](#q54-what-does-a-progressive-rollout-gate-waterfall-look-like)
- [Q55. What should trigger a ship, hold, rollback, or ramp decision?](#q55-what-should-trigger-a-ship-hold-rollback-or-ramp-decision)
- [Q56. How do I create an ongoing governance cadence?](#q56-how-do-i-create-an-ongoing-governance-cadence)

---

## Foundations

### Q1. What are multimodal AI evaluations, and what makes them their own discipline?

Multimodal AI evaluations are the systematic practice of measuring, diagnosing, and improving the quality of AI products whose inputs, outputs, or both span more than one modality — text, audio, image, video, structured data, sensor signals.

They are their own discipline because the failure surface is fundamentally larger than text-to-text systems, and because the standard LLM-eval toolkit silently breaks at the seams between modalities. Three claims justify the field. **One** — ingestion is a first-class evaluation surface; the moment audio becomes a transcript or a PDF becomes parsed text, errors enter that no downstream model fix will recover. **Two** — output correctness in one modality says nothing about cross-modal coherence; narration that is factually accurate while the slides on screen show something different is a failure neither an audio judge nor a visual judge will catch alone. **Three** — quality is irreducibly plural; Ingestion Fidelity, Faithfulness, Factuality, Cross-Modal Consistency, and Output Quality are five different contracts, and collapsing them into one accuracy number is the most common mistake teams make.

A useful test: if your product can fail in a way where every individual modality looks correct but the user experience is wrong, you are in multimodal eval territory. If not, the standard LLM-eval playbook is enough.

### Q2. How are multimodal AI evaluations different from text-only LLM evals?

Five differences that matter operationally.

**Ingestion is its own evaluation stage.** The model never sees the raw audio or video — it sees an ASR transcript, an OCR extraction, a frame sample. Errors at ingestion are usually invisible at the output and impossible to fix downstream.

**Quality fragments into five contracts.** Text evals ask "is the answer correct?" Multimodal evals must ask, separately: did we ingest faithfully, did the model stay faithful to what it ingested, are the facts true, do the modalities agree, is the rendered output good in its native form.

**Judges are harder.** LLM-as-judge for text is well-understood. Audio, visual, and cross-modal judges are immature. Off-the-shelf multimodal models hallucinate confidently when asked to evaluate non-text content.

**Annotation is more expensive and slower.** Reviewing 100 text traces takes an afternoon. Reviewing 100 traces of three-minute audio briefings takes days, and reviewer attention degrades faster.

**The fix-surface is wider.** A text-LLM failure usually maps to prompt, context, or model. A multimodal failure can map to thirteen distinct fix surfaces — preprocessing, ingestion model, chunking, retrieval, prompt, generation model, post-processing, rendering — and you need diagnostics that point you to the right one.

The honest summary: multimodal evals are LLM evals plus ingestion evaluation plus cross-modal evaluation plus a much harder judging problem. Each addition is non-trivial.

### Q3. Why is "it looks good" or "it sounds good" not enough?

Because surface impressions are the failure mode that ships bad products. Multimodal output is engineered to feel polished — generated audio has clean prosody, video has stable framing, images have plausible lighting — and humans read polish as correctness. This is the trap.

A NotebookLM-style podcast can sound like a professional NPR segment while quietly inverting a key claim from the source. A generated explainer can have beautiful motion design while the narration mentions a chart that isn't on screen. A medical image summary can read fluently while missing the actual finding. In each case, "it sounds good" is true at the surface and false at the contract layer.

The discipline is to evaluate against contracts, not impressions. A well-designed evaluation forces a reviewer to answer specific questions — is every claim in this audio supported by the source, does the visual at 0:42 match the narration, is the rendered chart accurate — before they are allowed to render any opinion on overall quality.

If your team's evaluation ends with a thumbs-up after watching the output once, you do not have evaluation. You have demoing.

### Q4. What does "evaluation" actually mean in a production multimodal AI product?

Evaluation is the closed loop that connects observed behavior to a ship/hold/rollback decision. Anything that does not feed a decision is decoration.

Concretely, production evaluation has four parts. 
**One** — a sampled stream of real production traces flowing into a review queue. 
**Two** — automated evaluators (code assertions, model-graded judges) plus human spot checks scoring each trace against the Five Quality Contracts. 
**Three** — dashboards aggregating scores by cohort, feature, and time, with confidence intervals, so drift is visible. \
**Four** — a governance cadence where a designated owner reviews dashboards, looks at sampled traces, and decides whether to ship, hold, roll back, or ramp.

What evaluation is not: a one-time pre-launch benchmark, a vendor leaderboard score, a generic helpfulness rating, or a vibe check by founders. The test is whether the system would catch a silent overnight regression. If the answer is no, you do not have an evaluation system.

The most important property of a production evaluation system is that it exists continuously, not episodically. Multimodal models, ingestion pipelines, and renderers all drift. Evaluation that runs only when someone remembers will not catch the drift that matters.

### Q5. What is a multimodal trace?

A multimodal trace is the complete, time-ordered record of every artifact, transformation, and decision involved in producing a single output — including all the non-text artifacts the system created or consumed.

For a text LLM, a trace is messages, tool calls, and retrieved documents. For a multimodal product, the trace must additionally include the raw input artifact (audio file, video, image, PDF), the ingested representation (transcript, OCR text, frame samples, embeddings), every intermediate generated artifact (the script, the SSML, the chart specification), and the final output in its native form.

This matters because most multimodal failures cannot be diagnosed from text logs alone. If a generated podcast contains a wrong claim, you need to play the audio at the moment of the claim, see the source paragraph it was supposedly grounded in, and inspect the script that produced the audio — all in one view.

A trace that lets you only see the final output is not a trace. It is a result. Traces exist to make diagnosis tractable.

### Q6. What is the minimum viable evaluation setup?

Five things, none optional, all small.

**One** — a labeled dataset of fifty representative inputs, hand-picked to cover the input variety your product actually sees. Not synthetic, not expanded.

**Two** — a single domain-expert reviewer designated as the benevolent dictator on quality. One person who decides what passes and what fails.

**Three** — a custom annotation interface, even if it's a notebook with a play button and a text box. The reviewer must be able to play audio, scrub video, view images at full resolution, and read the source artifact alongside the output.

**Four** — a binary failure taxonomy with at most ten categories, derived from open-coding the first 30–50 traces. Pass or fail per category. Not a 1–5 scale.

**Five** — a weekly cadence where the reviewer scores 20–30 fresh traces, the failures are tagged by category, and the counts are tracked over time.

What you do not need yet: an LLM-as-judge, a vendor platform, CI integration, or a dashboard. Those are v2 problems and should be built only after the manual loop has revealed which failure categories are stable enough to automate. The most expensive mistake is automating evaluation before you understand your failure modes.

### Q7. Who should own multimodal AI evaluations?

A product manager with domain expertise, supported by an ML engineer and a designer, accountable to a quality governance forum. Not QA. Not risk. Not the model team alone.

Evaluation is fundamentally "what does good mean for the user?" That is product judgment. An ML engineer optimizing without a clear definition of good will optimize for what is measurable, not for what matters. A QA team running scripted tests will catch regressions but miss the qualitative failures that define multimodal quality.

The ML engineer's role is to build instrumentation, evaluator infrastructure, and dashboards — to translate the PM's quality definitions into operational checks. They are the implementer, not the decider.

The designer's role is critical and usually under-allocated: cross-modal consistency, pacing, visual hierarchy, audio prosody — these are design questions, and designers have intuition for them that PMs and engineers lack.

Risk and compliance own a different lane: regulatory and safety contracts, red-teaming. Evaluation owned by risk becomes audit, not improvement.

If a single person has to be named, it is the PM. If a single forum has to be named, it is a weekly quality review with PM, ML, design, and a domain expert.

### Q8. How much effort should a team spend reading traces versus building automated evaluators?

Sixty to eighty percent on reading traces in the first six months. Twenty to forty percent on building evaluators. After that, the ratio inverts only if the team has discovered stable, repeatable failure modes worth automating.

Error analysis is where insight comes from — Hamel and Shreya hammer this in the LLM evals canon, and the case is stronger in multimodal because failure modes are less well-charted and the cost of an evaluator pointed at the wrong thing is higher.

Teams that get this wrong invert the ratio early. They spend three months building a multimodal LLM-as-judge harness, hit eighty-five percent self-agreement, declare the eval system done, and then ship a regression the harness was never designed to catch. The harness was answering a question the team had not yet learned was the wrong question.

A useful heuristic: do not build an automated evaluator for a failure mode until you have seen it at least ten times in manual review and can describe it in a single sentence two reviewers agree on. Below that threshold, automation encodes your misunderstanding. Above that threshold, automation pays for itself.

In multimodal products, the failures that matter most are usually the ones hardest to articulate at first. Reading traces is how the articulation happens.

---

## The Five Quality Contracts

### Q9. What are the Five Quality Contracts?

The Five Quality Contracts are the irreducible dimensions a multimodal AI product must satisfy. Each names a different promise, each can fail independently, and collapsing any two loses signal that matters.

**Ingestion Fidelity** — the input was captured and represented internally without distortion or loss that would change downstream meaning.

**Faithfulness** — the output is grounded in and consistent with what the system actually ingested. It does not invent, contradict, or extend beyond its evidence.

**Factuality** — the claims in the output are true about the world, judged against external ground truth, not just against the ingested input.

**Cross-Modal Consistency** — when the output spans multiple modalities, the modalities agree with each other in content, timing, and emphasis.

**Output Quality** — the rendered artifact is good in its native modality. Audio is intelligible and well-paced, video is stable, images are sharp, slides are readable.

The discipline is to score each contract independently and to refuse to publish a single composite quality number until you have looked at the five sub-scores and understood which is dominating. Most production failures are dominated by one or two contracts at a time, and the dominance shifts as the product matures.

### Q10. What is Ingestion Fidelity, and why does it matter before the model is involved?

Ingestion Fidelity is the contract that the system's internal representation of an input is a faithful representation of the input as the user supplied it. It matters before the model because errors injected at ingestion are usually impossible to recover from downstream — the model treats the corrupted representation as ground truth.

Concrete examples. ASR transcribes "the patient denies chest pain" as "the patient describes chest pain" — clinical meaning inverted by one word. A PDF parser extracts a table column-major instead of row-major, so every row is misaligned. OCR drops a negative sign. A frame sampler grabs frames at 1 fps from video where the critical action happens between samples.

In each case, every downstream component performs correctly given the input it received. The model summarizes faithfully, the judge approves, the output looks clean. It is wrong because the input was wrong before the model saw it.

Ingestion is the highest-leverage evaluation surface because ingestion errors are systematic and fixable. A 2% ASR error rate on medical terms is a tractable engineering problem with a clear remediation path. A model that occasionally invents claims is much harder to bound.

If you cannot answer "what is our ingestion fidelity rate on the ten most common input types?" with a number, you do not have ingestion evaluation.

### Q11. What is the difference between Faithfulness and Factuality, and can they fail independently?

Faithfulness asks whether the output is consistent with what the system ingested. Factuality asks whether the output is true about the world. They fail differently and require different evaluators — and yes, they can fail independently in either direction.

**Faithful but factually wrong**: an audio briefing summarizes an email that misstated a meeting time. The briefing repeats the misstated time. Faithful to the source, wrong about the world. Only an external check against the user's calendar will catch it.

**Factually correct but unfaithful**: a NotebookLM summary of a research paper says the study found "no significant effect" — correct in the world, but the paper itself stated the finding ambiguously and never used those words. The model fact-checked its source against its own training data and silently overrode it. This is more dangerous than it sounds; it destroys the trust contract of a source-grounded product.

The two contracts have different remedies. Faithfulness failures are model failures — fixed at the model layer with better grounding, prompting, retrieval, or post-hoc verification. Factuality failures, when the model was faithful, are source failures — fixed at the data layer with better source curation, freshness policies, authority ranking.

In closed-world products like NotebookLM, Faithfulness dominates. In open-world products like a news assistant, both are in play and both must be evaluated. The most common mistake is shipping one evaluator that does neither well.

### Q12. What is Cross-Modal Consistency, and why does it not exist in text-only evals?

Cross-Modal Consistency is the contract that, when an output spans multiple modalities, the modalities agree with each other in content, timing, and emphasis. It does not exist in text-only evals because text-only outputs have only one modality to check against itself.

The contract has three sub-components. 
**Content alignment** — what the audio says matches what the visual shows. 
**Temporal alignment** — when something is referenced in one modality, it appears in the other within the right tolerance window. The narrator says "as you can see here," and the thing they can see is on screen, not three seconds late. 
**Emphasis alignment** — what is highlighted in one modality is what is being emphasized in the other.

This contract is invisible if you evaluate modalities separately. An audio judge listening only to the audio will pass narration that says "the curve peaks in 2023" because the audio is correct in isolation. A visual judge looking only at the chart will pass it because the chart is correct in isolation. Only a cross-modal judge — looking at both, with timecodes — can catch that the chart shown at the moment of "the curve peaks in 2023" is a different chart with no peak in 2023.

Cross-modal consistency is the single failure mode where current eval tooling is weakest. There is no off-the-shelf evaluator that handles all three sub-components reliably. Most teams either accept high false-negative rates from automated cross-modal judges or rely on human review.

### Q13. What does Output Quality mean for audio, visual, video, and interactive outputs?

Output Quality is the contract that the rendered artifact is good in its native modality. It is deliberately decoupled from Faithfulness, Factuality, and Cross-Modal Consistency so that polish does not mask substance.

For **audio** — intelligibility (can every word be understood at normal volume on a phone speaker), pacing, prosody (intonation that conveys meaning, not flat TTS), absence of artifacts (no glitches or unnatural concatenation), voice consistency.

For **visual** — composition, legibility (text readable at intended display size), color and contrast (accessible, not hostile to colorblind users), absence of generation artifacts (no extra fingers, warped text, broken anatomy), brand consistency.

For **video** — all of the above plus temporal stability (no flicker, no identity shift between frames), motion quality, edit pacing.

For **interactive** — responsiveness within the latency budget, affordance clarity, graceful error states, accessibility (keyboard navigation, screen reader support).

The discipline is to evaluate Output Quality on its own rubric, with reviewers trained to hear or see the failure modes specific to the modality. A reviewer who cannot tell good prosody from bad is not qualified to evaluate audio Output Quality, no matter how senior they are.

### Q14. Why is one overall "accuracy" score dangerous?

Because it averages signal that should not be averaged, hides the dominant failure mode, and gives stakeholders false confidence at the moment when nuance matters most.

A product with 90% accuracy could mean 95% on four contracts and 70% on Cross-Modal Consistency — a serious problem hidden in the aggregate. Or it could mean 70% Ingestion Fidelity with everything downstream looking great because the model is doing its best with corrupted input — a different serious problem, also hidden. Same headline number, totally different remediation paths.

Aggregate metrics distort decision-making. A team that improves Output Quality from 90% to 95% while Faithfulness slips from 90% to 88% will see their composite go up and ship the change. A team watching the contracts separately will see the trade-off and ask whether it is acceptable.

The deeper issue is that the contracts are not commensurate. A 1% drop in Factuality on a medical product is not comparable to a 1% drop in Output Quality. Averaging them implies they trade off linearly, which is false.

Publish the five sub-scores prominently. Refuse to compute a composite for executive dashboards unless it is clearly labeled as a coarse health indicator. If a stakeholder asks "what is our quality score?" the correct answer is five numbers, not one.

---

## Failure Modes

### Q15. What are the most common failure modes in multimodal AI products?

In rough order of frequency: ingestion errors, cross-modal contradictions, faithful-but-wrong outputs, output quality artifacts, and abstention failures.

**Ingestion errors** dominate because every multimodal pipeline has an ingestion stage and most have under-evaluated it. ASR errors on domain terminology, OCR failures on tables and forms, frame sampling that misses key visual events, PDF parsing that mangles structure — all common, all silent at the output.

**Cross-modal contradictions** are second because most teams evaluate per-modality and have no cross-modal evaluator at all.

**Faithful-but-wrong outputs** appear in open-world products where the model is faithful to its source but the source itself was incomplete or wrong, and the model did not flag the limitation.

**Output quality artifacts** — bad prosody, image generation defects, video flicker, broken layout — are common in newer products and decline as generation models improve. They are highly visible to users and disproportionately damaging to trust.

**Abstention failures** — answering when refusing was correct, or refusing when answering was correct — appear in every product but are most damaging in high-stakes domains.

A rarer failure worth flagging: silent personalization drift. In products that learn from user behavior, a fixed eval set can show stable scores while the actual experience degrades for cohorts the eval set does not capture.

### Q16. Why do so many multimodal failures originate in ingestion?

Three reasons that compound, each sufficient on its own.

**Ingestion is where modality conversion happens.** Every multimodal system translates non-text inputs into model-readable representations — audio to transcript, image to embedding, PDF to structure. Conversion is lossy by nature. Information that does not survive the conversion is information the model never has and cannot recover.

**Ingestion is the least-evaluated stage.** Teams evaluate models because models are interesting. They evaluate outputs because outputs are what users see. Ingestion is plumbing, and it usually gets attention only after a downstream failure has been traced back to it. By then the team has shipped a product that depends on its silent assumptions.

**Ingestion errors are systematic.** A model that fails 2% of the time fails on a roughly random 2% of inputs. An ASR system that mistranscribes medical terms fails on every medical input that contains those terms. Systematic errors compound — the same user with the same input format gets the same wrong answer every time, creating concentrated failure pockets that aggregate metrics flatten.

The combination is brutal: ingestion does the most damage, gets the least attention, and concentrates errors where they hurt most. A team that adds a single ingestion fidelity dashboard usually finds it explains more variance in user satisfaction than every other dashboard combined.

### Q17. What are common PDF, OCR, and document-parsing failures?

These are pervasive in legal, financial, healthcare, and insurance products and account for an outsized share of customer complaints.

**Table structure loss.** A table extracted column-major instead of row-major, so every row is misaligned. Or merged cells flattened, so a header that applied to three columns now applies to one. The model produces a coherent-looking summary that is wrong.

**Reading order errors.** Multi-column documents parsed as single-column, so text reads "first sentence of left column, first sentence of right column, second sentence of left column..." Models sometimes recover from context, sometimes silently treat the broken order as correct.

**Sign and unit drops.** OCR drops a negative sign, a decimal point, or transcribes "mg" as "mcg" — a thousand-fold dosage error in healthcare.

**Footnote and citation collapse.** Footnotes inlined into body text, breaking the claim-evidence relationship. Citations stripped, so the model summarizes claims as if the document asserted them when they were attributed elsewhere.

**Form field misalignment.** A scanned form where labels and values are misaligned: "Name: John, DOB: 1985, Diagnosis: hypertension" parsed as "Name: 1985, DOB: hypertension, Diagnosis: John."

The discipline is to maintain a regression set of fifty to one hundred documents that exercise these failure modes, run the parser on every release, and require a parser engineer to look at every diff. Generic OCR confidence scores correlate weakly with downstream meaning loss and are not a substitute.

### Q18. What are common ASR and audio-ingestion failures?

ASR is good enough that teams forget how often it is wrong, and the failures cluster where they matter most.

**Domain terminology errors.** Medical, legal, and technical vocabulary is systematically mistranscribed by general-purpose ASR. "Metoprolol" becomes "meta prolol." "Voir dire" becomes "vor deer." A model summarizing the transcript produces a fluent and wrong summary.

**Negation and modal flips.** "Patient denies chest pain" transcribed as "patient describes chest pain." "We will not be proceeding" as "we will be proceeding." Single-word errors with full meaning inversion, common because the words are short, unstressed, and acoustically ambiguous.

**Speaker diarization failures.** In multi-speaker audio, attribution gets crossed. Speaker A's question is attributed to Speaker B, and the entire downstream interpretation is wrong.

**Numeric and date errors.** "Fifteen" and "fifty" confused. "March 4th" becomes "March 14th." Currency amounts drop a digit.

**Background and crosstalk.** Two speakers talking over each other merged into one nonsensical utterance. PA announcements transcribed as if they were the primary speaker.

The fix is layered: domain-specific ASR or a domain lexicon, post-ASR verification with context, and an ingestion evaluator that checks high-impact error classes (negations, numbers, names) on every transcript before downstream use. A product that depends on ASR without a dedicated ASR evaluator is shipping on hope.

### Q19. What are common visual understanding failures?

Visual understanding failures are insidious because the model output is fluent prose that sounds like it was based on the image, but does not match it.

**Chart misreading.** The model reads the wrong axis, swaps x and y, misidentifies units (percent vs absolute), or misses a log scale. Revenue described as "growing 30% per year" when the chart is log-scale and actual growth is 30% per decade.

**Trend inversion.** Chart shows decline, model describes growth, or vice versa. Disproportionately common with non-zero baselines or inverted axes.

**Legend ignorance.** A multi-series chart described as a single series, or two series confused with each other, because the model did not parse the legend.

**Diagram structure errors.** Architecture diagrams with arrows reversed, components misnamed, or boxes-within-boxes flattened into a list.

**Screenshot grounding errors.** A UI described as having a button that is not visible, or missing one that is, or describing a state two screens away from what is shown.

**Counting and quantity errors.** "Five people in the image" when there are six. "Three bars" when there are four. Counting is one of the most reliable failure modes in current vision models, and it persists in the strongest models.

The evaluator pattern is to maintain a regression set of fifty to one hundred chart and diagram images with verified ground-truth descriptions, score outputs on specific dimensions (axis identification, trend direction, count accuracy), and treat aggregate "image understanding" benchmarks as worthless for product-specific quality.

### Q20. What are cross-modal contradiction failures, and why do they slip past per-modality evaluators?

A cross-modal contradiction is a failure where each modality is internally correct but the modalities disagree. They slip past per-modality evaluators because no per-modality evaluator is looking at the other modality.

The classic pattern: an explainer video where the narration says "as you can see in the chart, sales peaked in Q3" while the on-screen chart shows sales peaking in Q1. The audio is fluent. The chart is well-rendered. An audio judge passes the audio. A visual judge passes the chart. The video fails.

Other patterns. A slide deck where the narration mentions an item that is on a different slide. An accessibility caption that summarizes rather than transcribes the audio, so a deaf user gets different content than a hearing user. A multilingual product where audio and subtitles diverge beyond translation differences. A tutorial where the narrated step is "click the blue button" and the only button is red.

These slip through because detecting them requires three things in one evaluator: access to both modalities, time-alignment between them, and a model capable of cross-referencing claims in one modality against state in another. Most evaluation harnesses lack at least one of the three.

The current best practice is hybrid: an automated cross-modal judge that flags candidates for human review, with a high-recall, low-precision target — surface every plausible contradiction, accept that humans will dismiss most, but ensure none are missed.

### Q21. What is the "quiet failure signature"?

The quiet failure signature is a state where every dashboard metric is at or above target, the regression suite passes, the LLM-as-judge scores are stable, and yet user satisfaction or retention indicates the product is failing. It is the most dangerous diagnostic state because the team has no instrumented signal to act on.

Common causes. 
**Eval-set staleness** — the dataset reflects an input distribution that has shifted. 
**Judge over-fitting** — the LLM-as-judge has been tuned to pass the model's typical outputs and now lacks discrimination. 
**Contract gap** — the failure mode is in a contract you are not measuring, classically Cross-Modal Consistency with no cross-modal evaluator. 
**Personalization drift** — aggregate metrics on a fixed set are stable, but the personalized experience has degraded for cohorts the eval set does not capture. 
**Surface polish without substance** — Output Quality is high, Faithfulness is low, but the dashboard composite weights polish more heavily.

The remedy is to treat the quiet failure signature as a diagnostic question, not a metrics question. When users are unhappy and metrics are green, do not look harder at the metrics. Sample a hundred recent traces from users who churned, complained, or rated negatively, and read them. The reading reveals which contract is failing, and the contract reveals which evaluator is missing. Only then return to the dashboard.

A team with a process for the quiet failure signature is mature. A team without one has only the metrics it built when it was less wise.

---

## Evaluation Design

### Q22. How do I write an Evaluation Brief?

The Evaluation Brief is a one- to two-page document written before the feature ships, declaring what good looks like and how good will be measured. It exists so that quality is a forethought rather than a postmortem.

Seven required sections.

**Feature description** — what the feature does, in three sentences. If you cannot, you do not understand it well enough to evaluate it.

**User and use case** — who uses this, what they're accomplishing, what success looks like from their point of view. Specific personas.

**The Five Contracts, instantiated** — for each contract, one sentence on what it means specifically for this feature. If a contract does not apply, say so explicitly and justify it.

**Failure modes** — the top five to ten hypothesized failure modes, each as a short paragraph with a worked example. This list will be wrong, and the wrongness is informative when corrected by error analysis.

**Evaluation surface** — for each failure mode, what evaluator type is appropriate (assertion, code check, LLM-as-judge, human review), at what cost, with what cadence.

**Pass bar** — for each contract and evaluator, the threshold below which the feature is not shippable. Quantitative if possible, qualitative if not. Specific, not aspirational.

**Decision rules** — what triggers ship, hold, ramp, or rollback. Tied to the pass bars.

The brief is reviewed by PM, ML, design, and a domain expert before instrumentation begins. It is a living document, updated as error analysis reveals new modes — but its existence in v1 is what separates disciplined evaluation from improvised evaluation.

### Q23. How do I define "good" before seeing evaluation scores?

Write it down in the Evaluation Brief, in concrete behavioral terms, with worked examples, before you have seen any model output. The discipline is anchoring.

Once you see model output, your sense of "good" anchors to what the model actually produces. A team that evaluates a podcast generator after listening to a hundred generated podcasts has internalized the model's actual output distribution as the reference point, and is now grading on a curve. Subtle failures become invisible because they are normal.

Define good in advance, against examples that have nothing to do with the model's output. For an audio briefing, write down what a good two-minute briefing sounds like by referencing existing human-produced briefings — not "good for an AI" but "good, full stop." For a generated explainer, reference high-quality human-produced explainers. The reference is the human bar, anchored to artifacts that exist in the world.

Then write paired good/bad examples specific to your feature. For Faithfulness: a faithful summary, an unfaithful summary, with specific lines highlighted. For Cross-Modal Consistency: an aligned example, a contradiction, with timestamps. The paired examples are the rubric. Reviewers refer to them when scoring.

When the model's actual output reveals failure modes you did not anticipate, update the rubric — do not move the bar. Lowering the standard because the model is not meeting it is how product teams ship mediocre AI and convince themselves it is good.

### Q24. Should multimodal evals use binary pass/fail or 1–5 Likert scores?

Binary, with one exception. Hamel and Shreya are right and the case is stronger in multimodal because the failure modes are more discrete.

Binary forces a decision. Likert lets the reviewer hedge with a 3, which contains no information. Binary produces consistent labels across reviewers. Likert produces inconsistent labels because the difference between a 3 and a 4 is unreplicable. Binary is faster to label, which means more labels per hour, which means better evaluator training data.

In multimodal, the phenomena are usually binary. A narration either contradicts the visual or it does not. A chart is either misread or correctly read. An OCR transcript either dropped the negative sign or it did not. Imposing a Likert scale is a methodological mismatch.

The exception: Output Quality has gradient sub-dimensions where a multi-point scale can be defensible. Audio prosody, video composition, and image aesthetics are genuinely continuous. For these, a three-point scale (poor / acceptable / excellent) with anchored examples can be more useful than binary.

Even there, the better approach is to decompose: prosody acceptable yes/no, pacing acceptable yes/no, intelligibility acceptable yes/no. Decomposition preserves the binary discipline and gives finer-grained signal than a Likert score does.

The general rule: if you find yourself wanting a 5-point scale, decompose the question further. If decomposition is impossible, ask whether you understand the failure mode well enough to evaluate it at all.

### Q25. When should I use human judges versus LLM-as-a-Judge?

Humans for what humans are uniquely qualified to judge. LLMs for everything else. Never LLM-as-judge for anything you have not first labeled by hand and validated against.

Humans are uniquely qualified for: domain expertise the LLM lacks (a senior radiologist evaluating a clinical image summary), subjective qualities not yet operationalized (whether narration is "engaging"), cross-modal consistency at high precision (because current multimodal LLMs are unreliable here), and the first thirty to fifty traces of any new evaluator, where the human is generating the labels the LLM-as-judge will be calibrated against.

LLM-as-judge is appropriate for: scaling a check that has been validated against human labels with at least 0.7 Cohen's kappa, faithfulness checks where the source fits in context and the question is structural, code-evaluable but LLM-formattable checks (well-formed JSON, valid SSML), and high-volume regression sweeps where human review is infeasible.

LLM-as-judge is inappropriate for: anything requiring hearing audio (current audio judges are weak), anything requiring watching video (very weak), counting and arithmetic (unreliable), and any failure mode that emerged in production within the last sampling window (because the judge has not been validated on it).

The default order: hand-label fifty traces. Build an LLM-as-judge prompt. Score the same traces. Compute kappa. Below 0.7, redesign. 0.7–0.85, use for triage with human review on disagreements. Above 0.85, the judge can stand alone with periodic spot checks.

Skipping validation is the most common multimodal eval mistake.

### Q26. What is a Dictator Panel?

A Dictator Panel is an evaluator architecture in which several specialized judges each have absolute authority over their own dimension, and a Tiebreaker resolves conflicts. The name comes from the political concept — each dictator is sovereign in its domain — and it is the most reliable architecture for multimodal evaluation we have.

The standard composition has five members. **Content Dictator** — judges Faithfulness and Factuality, with access to the source. **Ingestion Dictator** — judges whether the system's internal representation is faithful to the raw input. **Audio Dictator** — judges audio Output Quality and audio aspects of Cross-Modal Consistency. **Visual Dictator** — judges visual Output Quality and visual Cross-Modal Consistency. **Tiebreaker** — a senior human or high-context judge resolving conflicts.

Each dictator has veto power within its domain. If the Content Dictator says the output is unfaithful, the output fails — no other dictator can overrule. This is deliberate. The architecture refuses the temptation to average disagreements into a composite, which is what makes it reliable.

To build one. Start with the Content Dictator, the easiest to build and validating the most failures. Use a strong text LLM with the source in context, prompted to evaluate Faithfulness and Factuality on a binary rubric. Validate against fifty hand-labeled traces. Then add the Ingestion Dictator. Audio and Visual come last because they are hardest to build and have the weakest model support today. The Tiebreaker is human in v1.

Run the panel asynchronously, in batch. Synchronous Dictator Panel evaluation is too slow for production critical paths and not the architecture's purpose anyway.

### Q27. Should I use a single multimodal judge or specialized per-modality judges?

Specialized per-modality judges, organized as a Dictator Panel. The single-judge pattern is appealing because it sounds simpler, but it fails for three reasons that compound.

**Single judges hide their reasoning.** A judge that takes audio, video, and source as input and produces a single score gives you a number with no diagnostic content. When the score drops, you do not know which contract failed or which modality is responsible. A panel tells you exactly which dimension is degrading.

**Single judges are harder to calibrate.** Validating one judge against human labels on five intertwined contracts requires more labeled data than validating five judges against one contract each, because the joint label space is larger and human reviewers are less consistent on composite judgments. The math goes the wrong way.

**Single judges drift unpredictably.** When you change the prompt, retrain the model, or update part of your product, a single multimodal judge may shift in ways hard to predict and attribute. A panel localizes drift to specific dictators, which makes detection and recalibration tractable.

The exception: if you are running an early prototype and lack the labeled data to build five judges, a single judge is acceptable as a placeholder. The placeholder must be labeled as such, and replaced within a quarter. Single judges that persist past prototype are technical debt that compounds.

### Q28. Where do off-the-shelf multimodal models break down as judges?

Six places, roughly in order of severity.

**Counting and quantification.** Multimodal models are unreliable at counting, reading exact values from charts, and verifying numeric claims. A judge needing to verify "the chart shows three regions" or "the value is exactly 42" will be wrong often enough to be untrustworthy.

**Fine-grained spatial reasoning.** Whether two elements overlap, whether a label is correctly attached, whether arrows in a diagram point correctly — unreliable, with errors clustering on cases a human finds trivial.

**Audio nuance.** Most off-the-shelf multimodal models have weak audio support, especially for prosody, emotion, and quality artifacts. They can transcribe and identify content but struggle with "is this narration well-paced and engaging."

**Long-context grounding.** Faithfulness evaluation against long sources (50-page documents, 30-minute audio) degrades with context length. The judge fails to find evidence that is present, or finds evidence in places where it is not.

**Temporal alignment in video.** Whether the audio at 0:42 matches the visual at 0:42 within tolerance is something current models do badly, especially at cuts and transitions.

**Subtle cross-modal contradictions.** When modalities are individually plausible but mutually inconsistent, models often pass the output. They are stronger when the contradiction is gross than when it is subtle.

The discipline is to test the judge on a labeled set of known failure modes before deploying it, not to assume the judge can do what its marketing says it can do. If the judge fails on these six categories, you know which subset of evaluation it can be trusted with.

### Q29. How do I calibrate an LLM judge using Cohen's kappa?

Cohen's kappa is the chance-corrected agreement metric between two raters, ranging from −1 to 1, with 0 meaning agreement at chance level. For evaluation calibration, treat it as the answer to "is my LLM-as-judge agreeing with my humans for the right reasons, or just by accident?"

Six steps. 
**One** — hand-label fifty to one hundred traces with binary pass/fail on a single contract. One human, the benevolent dictator, no committee. 
**Two** — run the LLM-as-judge on the same traces. 
**Three** — build a 2x2 confusion matrix. 
**Four** — compute kappa using the standard formula. Most labs have a one-line library function. 
**Five** — read the kappa. Below 0.4, the judge is uncorrelated with the human and must be redesigned. 0.4–0.6, partially aligned, needs prompt or model iteration. 0.6–0.75, usable for triage but every disagreement should be human-reviewed. Above 0.75, reliable with periodic spot checks. 
**Six** — recompute kappa monthly on fresh traces. A judge that was 0.8 in March can be 0.5 by September with no one noticing.

Two cautions. Kappa on a single contract is the right unit; kappa on a multi-contract composite is misleading. Kappa with class imbalance — most traces pass — can look high while the judge is bad at detecting failures, which is the case you actually care about. For class-imbalanced scenarios, supplement with True Positive Rate and True Negative Rate explicitly.

A judge with high kappa, high TPR, and high TNR is the bar. Anything else is a draft.

### Q30. How should I design synthetic test cases for multimodal AI products?

Structured by dimensions, generated in two stages, grounded in real failure modes — the same general pattern Hamel and Shreya recommend for text, with multimodal-specific dimensions added.

**Define dimensions specific to multimodal.** For audio: speaker count, accent, background noise, terminology density, length. For documents: format type, layout complexity, table presence, language, scan quality. For images: object count, occlusion, lighting, resolution. For video: cut frequency, motion level, on-screen text density.

**Add cross-modal dimensions.** Modalities present (audio-only, audio plus slides, full video), alignment scenarios (clean, expected drift, deliberate contradiction), and modality dominance.

**Build tuples manually first.** Hand-write twenty tuples covering the dimensional space. This is the irreplaceable work — where you discover which dimensions matter and which combinations are pathological.

**Generate at scale in two stages.** Use an LLM to expand into a few hundred combinations, then convert each tuple into a concrete input artifact specification. Generate the actual non-text artifacts using real generation tools (TTS for audio, real PDFs, real images), not by asking the LLM to simulate them. Synthetic audio that the LLM "describes" is not audio.

**Sanity-check against production.** Take a hundred recent production inputs, classify them by your dimensions, and verify your synthetic set covers the regions production occupies.

**Reserve a large fraction for real, not synthetic.** In multimodal, the gap between synthetic and real distributions is wider than in text. Aim for at least 50% real inputs in your evaluation dataset.

### Q31. How do I evaluate subjective qualities like pacing or engaging narration?

By decomposing the subjective into binary sub-checks, anchoring with paired examples, and accepting that human review is irreplaceable for the long tail.

**Decomposition.** "Engaging narration" is not directly evaluable — different reviewers rate the same audio differently. But it can be decomposed: are sentence-level pauses appropriate (binary), is prosody varied across the artifact rather than monotone (binary), are emphasis and stress on the right words (binary), is speaking rate within the appropriate band (binary)? Each sub-check is operationalizable. The composite is the conjunction plus a small qualitative residual.

**Anchoring.** For each sub-check, maintain a paired-example library: a known-good clip and a known-bad clip, with a one-sentence description of why each is what it is. Reviewers refer to the library before scoring. Without it, every reviewer brings their own implicit standard and labels are inconsistent.

**Human-review acceptance.** The qualitative residual — the part of "engaging" that is not captured by sub-checks — exists and is real. It is what distinguishes a podcast that sounds professional from one that is technically correct but flat. This residual cannot be automated reliably with current technology. Trying to do so produces evaluators that score the sub-checks while missing the residual. The honest discipline is to accept the residual as a human-review item, sample 20% of traces for qualitative review specifically on this dimension, and budget accordingly.

The mistake to avoid: building an LLM-as-judge for "is this engaging," getting 0.5 kappa, and shipping it because it is better than nothing. It is not better than nothing. It is worse, because it gives the team false confidence.

---

## Pipeline Diagnosis

### Q32. What is the nine-stage multimodal AI pipeline?

The nine-stage pipeline is the canonical decomposition of a multimodal AI product into stages where evaluation must be instrumented. Each stage can fail independently and each has a different remedy.

**Stage 1 — Capture.** Raw input is acquired — file upload, recording, image capture, sensor stream. Failures: corrupt files, truncation, encoding errors.

**Stage 2 — Pre-processing.** Input is normalized — audio resampled, image resized, video re-encoded. Failures: lossy resampling, aspect ratio distortion.

**Stage 3 — Ingestion.** Input is converted to model-readable form — ASR transcript, OCR text, frame samples, embeddings. Failures: every mistranscription and parse error.

**Stage 4 — Retrieval and grounding.** Relevant context is gathered — RAG documents, user history, structured data. Failures: missing or stale evidence, grounding on the wrong source.

**Stage 5 — Synthesis.** The model produces the core output — script, answer, structured plan. Failures: faithfulness errors, factuality errors, abstention failures.

**Stage 6 — Cross-modal alignment.** Synthesis is mapped to target modalities — script to SSML, content to slide layout, plan to video timeline. Failures: timing, attribution, structural mismatches.

**Stage 7 — Generation.** Non-text artifacts are produced — TTS audio, rendered images, video frames, charts. Failures: artifact quality issues, generation defects, voice drift.

**Stage 8 — Composition.** Generated artifacts are assembled — audio with music bed, video with subtitles. Failures: alignment, format, accessibility regressions.

**Stage 9 — Delivery and rendering.** Final artifact is delivered to the user's device. Failures: codec issues, latency, device-specific bugs.

Not every product has all nine, but every product has some subset, and the subset must be named and instrumented so failures can be localized.

### Q33. Why evaluate the pipeline instead of only the final output?

Three reasons that compound, each sufficient on its own.

**Final-output evaluation tells you that something is wrong, not what is wrong.** A bad podcast could be an ASR error from Stage 3, a faithfulness error from Stage 5, a TTS prosody error from Stage 7, or a composition error from Stage 8. The remedy for each is different — different team, different tool, different change. Without stage-level instrumentation, the team gets a bad-podcast verdict and starts guessing.

**Stage-level evaluation finds failures the final output hides.** An ingestion error that introduces a small inaccuracy at Stage 3 may be smoothed over by a confident model at Stage 5, producing a fluent output that hides the underlying corruption. The final output looks good. The system is rotting from within.

**Stage-level evaluation enables faster iteration.** When the synthesis model improves, you want to test synthesis in isolation, not re-run the entire pipeline. When the TTS vendor releases a new voice, you want to test only Stage 7. Stage-level eval datasets and evaluators let you test components without re-running the whole pipeline — the difference between weekly and monthly iteration cycles.

The mistake teams make is treating stage-level evaluation as a nice-to-have to get to once final-output evaluation is mature. The opposite is true. Stage-level evaluation is the foundation; final-output evaluation is the integration test on top.

### Q34. How do I trace a failure back to its origin stage using causal chain analysis?

Causal chain analysis is the procedure of walking a failure backward through the pipeline until you find the first stage where the artifact deviated from what it should have been. The procedure is mechanical, and the discipline is to do it before forming a hypothesis about cause.

The steps. Start at the final output and identify the specific failure — the exact claim, the exact frame, the exact moment. Move one stage upstream. Examine the artifact at that stage for the same failure. If present, the origin is at or upstream of that stage. If not present, the origin is between this stage and the next downstream. Continue upstream until you find the stage where the failure first appears.

The discipline matters because hypothesis-first debugging is a major source of misattribution. A team that sees a bad podcast and assumes the model hallucinated will spend a week tuning the synthesis prompt before discovering the failure was in ASR. The mechanical walk-backward procedure prevents this.

Two patterns to watch for. **Compound failures** — multiple origin stages, each contributing. The walk-backward procedure finds the first; once it is fixed, repeat to find the next. Do not assume single-cause attribution. **Phantom failures** — the failure appears at a stage but was actually introduced at the previous stage in a form that became visible only after the next transformation. Inspect each stage's input as well as its output.

The output of causal chain analysis is a fix surface — a specific stage and a specific change to make at that stage.

### Q35. What should I log at each pipeline stage?

At every stage: the input artifact, the output artifact, transformation parameters, the model or component version, and a stable trace identifier tying the stage to the rest of the pipeline.

The minimum log entry per stage. **Trace ID** — primary key tying all nine stages together. Without it, post-hoc reconstruction is impossible. **Stage timestamp** — millisecond precision for diagnostics. **Input artifact reference** — path or content hash. Inline content for small text, references for large media. **Output artifact reference** — same. **Component version** — model name, library version, configuration parameters. This is what enables you to detect and isolate regressions when something changes. **Stage-specific metrics** — for ASR, confidence scores; for retrieval, documents and scores; for generation, latency and tokens.

What to never lose: the raw input, end to end, even after the user request is over. Without it, you cannot replay the pipeline against an updated component to verify a fix. The raw input is the most valuable thing in your trace store.

What to handle carefully: PII in audio, video, and document inputs. Default to encryption at rest and access logging for raw artifacts, with a separate redacted trace for general team access. The dichotomy is real — the team needs raw access for diagnostics, the user needs privacy. Solve it with access controls, not by losing the artifacts.

What to compress aggressively: intermediate generated artifacts that can be regenerated from inputs and parameters. If you log the script, the SSML, and the TTS configuration, you do not need to permanently store the audio at full fidelity.

### Q36. How do I tell if a downstream failure is actually an ingestion failure in disguise?

Run causal chain analysis backward and inspect the Stage 3 ingestion artifact against the Stage 1 raw input. If the failure is present in the ingestion artifact but not in the raw input, the failure is an ingestion failure regardless of how it manifests downstream.

Heuristics that suggest it. **The failure involves a domain-specific term, name, number, or unit.** Ingestion errors cluster on these. **The failure is consistent across runs of the pipeline against the same input.** Model failures are stochastic; ingestion failures are deterministic. **The failure correlates with a specific input format or source type.** Ingestion errors are usually format-specific. **The model's downstream output is fluent and well-structured around the error.** A model with corrupted input produces a fluent output with the corruption baked in.

The diagnostic technique. Re-run the pipeline against the same input with the synthesis stage held constant. If the failure persists, the origin is upstream of synthesis. Then re-run with the ingestion stage held against a known-good ingestion (manually transcribed audio, manually parsed document). If the failure disappears, ingestion was the origin. Mechanical and unambiguous.

Why this matters operationally. Teams that misattribute ingestion failures to model failures spend cycles on the wrong stage. They tune prompts, swap models, retrain — none of which fix the corruption injected at Stage 3. A team that runs ingestion-isolation diagnostics on every recurring failure usually finds 30–50% of "model" failures are ingestion failures wearing a model failure costume.

### Q37. What are the thirteen fix surfaces in the Optimization Action Library?

The Optimization Action Library names the thirteen places in a multimodal product where a fix can be applied, organized by pipeline stage, so every diagnosed failure can be mapped to a specific change rather than a vague "improve the model."

**Capture & Pre-processing (3).** Input format constraints (restrict to formats you can ingest reliably). Pre-processing parameters (sample rates, resolutions, normalization). Capture-time validation (reject inputs that fail integrity checks).

**Ingestion (2).** Ingestion model choice (which ASR, OCR, vision encoder). Ingestion post-processing (lexicon application, error correction, structural normalization).

**Retrieval and grounding (2).** Retrieval strategy (chunking, embedding model, search algorithm). Grounding policy (which sources to trust, freshness rules, deduplication).

**Synthesis (3).** Prompt engineering. Model choice (size, vendor, version). Constraint mechanisms (structured output schemas, output validation, refusal triggers).

**Generation and rendering (3).** Generation model and parameters (TTS voice, image model, sampling parameters). Cross-modal mapping (timing, layout, attribution rules). Composition rules (how artifacts are assembled).

The discipline is that every diagnosed failure is assigned to one of these thirteen surfaces, not to the abstract "the model is bad." The surface determines the owner, the cost of the fix, and the time to remediate. A failure assigned to "input format constraints" is a one-day fix by the platform team. A failure assigned to "model choice" is a quarter-long evaluation. Naming the surface enables prioritization.

There are thirteen because that is what appears in production multimodal products. Adjust the count to match your stack, but resist collapsing surfaces. Fewer surfaces means coarser ownership, which means slower fixes.

### Q38. How do I run driver analysis when a failure could be caused by any of three modalities?

Five-hypothesis driver analysis, adapted for multimodal: enumerate plausible causes, design a discriminating test for each, run the tests in cost order, stop when one isolates the cause.

For a multimodal failure with multiple candidates. **Hypothesis 1** — ingestion in modality A failed (e.g., ASR error). **Hypothesis 2** — ingestion in modality B failed (e.g., OCR error on a referenced document). **Hypothesis 3** — synthesis failed (model invented or distorted). **Hypothesis 4** — cross-modal alignment failed (script was right but the audio-visual sync rule misapplied). **Hypothesis 5** — generation in the output modality failed (TTS or image generation produced a defective artifact).

For each hypothesis, design a discriminating test. **For ingestion** — replace the ingested artifact with a manually verified version, re-run downstream, see if the failure persists. **For synthesis** — replace the synthesis output with a verified-correct script, re-run downstream. **For cross-modal alignment** — inspect the alignment artifact directly against the source script. **For generation** — replace with an alternative (different TTS voice, different image model).

Run tests in cost order — cheap first. Inspecting the ingestion artifact and the alignment artifact directly costs nothing. Re-running with substitutions costs more. Custom diagnostics cost the most. Most failures isolate from the first one or two cheap tests.

Run the protocol as a list, not a tree. Do not branch into "it could be A and C combined" until you have ruled out A alone and C alone. Combinations are real but rare; isolated single causes account for the large majority.

The output is a fix-surface assignment, which feeds the optimization backlog.

---

## Tooling & Infrastructure

### Q39. Can I run multimodal evaluation no-code in Braintrust?

Mostly yes for content evaluation, no for ingestion-level evaluation, partially for cross-modal. The no-code framing is useful but not literal — practitioners using Braintrust for multimodal almost always end up writing some code.

The no-code path covers: dataset management (uploading inputs, expected outputs, and metadata via the UI), playground experimentation, built-in scorer configuration for text-grounded checks (faithfulness against a source, factuality against a reference), and dashboard review. For a Faithfulness-focused evaluation of an audio-summarization product, where the source is text and the output is text-graded after transcription, you can move a long way without writing code.

The no-code path stops at: custom scorer logic (any non-trivial cross-modal check, anything reaching into a non-text artifact, specialized validators), evaluator validation against human labels (kappa computation, sampling logic for spot checks), and CI integration. Each requires Python or TypeScript with the SDK.

The honest framing for Braintrust on a multimodal product: it is "low-code" rather than "no-code." A PM or domain expert can run experiments, label data, review traces, and inspect dashboards without code. An ML engineer is required to set up scorers, build dataset ingestion, and integrate with CI. The discipline of separating who does what — domain experts in the UI, engineers in the SDK — is what makes the platform productive.

For a team starting out: pick Braintrust if you have at least one engineer who can write the scorers and at least one domain expert who can drive labeling without writing code. If you have only one of the two, you will under-use it.

### Q40. What custom annotation tooling do I need for audio review?

A custom tool an engineer can build in a day with an AI coding assistant, doing five things. Off-the-shelf tools generally cannot do all five at once.

**One.** Synchronized waveform with playback — the reviewer sees the waveform and a play head, can click anywhere to jump, and can play short segments without re-loading.

**Two.** Aligned transcript or script display — as audio plays, the corresponding text is highlighted in real time. The reviewer can click a word in the transcript to jump audio to that point, and vice versa. This is the highest-leverage feature for ASR evaluation and faithfulness review.

**Three.** Source artifact alongside — for evaluating Faithfulness, the source document is visible in the same view as the audio and transcript, scrollable independently, with the ability to highlight regions for cross-reference.

**Four.** Per-segment binary labels with hotkeys — pass/fail on each contract with single keystrokes (P for pass, F for fail, B for bookmark, N for next).

**Five.** Progress and review state — visible counter, persistent saved labels across sessions, ability to amend previous labels.

Optional but valuable: speed adjustment (1.5x or 2x for skimming), regions of interest pre-flagged by automated scorers, side-by-side comparison of multiple model outputs.

The tool exists to make audio review fast enough that a reviewer can sustain attention for a session of fifty traces. Without these affordances, audio review is so slow that teams give up and revert to spot-checking, which is how multimodal failure modes hide.

### Q41. What custom annotation tooling do I need for video review?

Everything from audio review plus three video-specific affordances.

**One.** Frame-stepping with timecode — the reviewer can step frame by frame, jump by intervals (5 seconds, 30 seconds), and see the exact timecode at all times. Cross-modal contradictions live at specific timecodes; a tool that cannot show the timecode cannot localize them.

**Two.** Side-by-side rendering of audio waveform, transcript, and video — all three modalities aligned to the same time axis, scrolling together. This is the only interface where cross-modal consistency can be evaluated efficiently. A tool that shows them serially or in separate tabs is unusable for this contract.

**Three.** Bookmark-and-comment at timecode — flag a specific timecode, attach a category and a comment, persist these flags across the entire trace. The flagged timecodes become the dataset for cross-modal consistency analysis.

What makes video review uniquely hard is fatigue. Reviewing fifty thirty-second video clips with cross-modal scrutiny is more demanding than reviewing five hundred text traces. The tool's job is to minimize friction at every point so the reviewer's attention is preserved for actual judgment.

The minimum viable version is a notebook-based tool with HTML video, hotkey navigation, and a JSON-backed labels store. Build it before buying anything.

### Q42. How do I handle production trace storage for audio and video?

Three policies, applied in combination.

**Tiered retention by modality.** Raw audio and video at full fidelity for 7–30 days, then dropped to compressed reference (low-bitrate audio, key-frame-only video) for another 60–90 days, then dropped entirely except for traces tagged for the eval set. Tagging happens at sampling time — flagged traces are pulled into permanent storage, the rest retained briefly.

**Aggressive sampling.** Do not store every production trace. Sample 1–5% by default, with stratified oversampling on segments that matter (new feature usage, low-confidence inferences, user-flagged interactions). The sampling logic is the difference between trace storage that fits in a reasonable budget and storage that does not.

**Reproducibility-first storage.** Store inputs, parameters, and component versions; do not permanently store every intermediate artifact. If you have the raw audio file and the ASR model version, you can regenerate the transcript. If you have the script and the TTS configuration, you can regenerate the audio. Permanent storage is for inputs and deterministic-replay information.

What to never compromise on: the raw input artifact for sampled traces, kept at full fidelity, indefinitely if possible, encrypted and access-controlled. Without it, you cannot reproduce the failure. The component versions and parameters — tiny, and they enable forensics six months later. Labels and reviewer comments — the most valuable thing in your eval system and the cheapest to store.

The cost-modeling exercise to do up front: per-trace storage cost at full fidelity, multiplied by sampled volume, multiplied by retention period. The number is usually larger than expected by an order of magnitude. The tiered policy and sampling rate are the two knobs.

### Q43. What gaps in eval tooling should I expect to fill myself in 2026?

Five, in rough order of pain.

**Stage-level evaluation infrastructure.** Existing platforms are oriented around end-to-end traces. Evaluating Stage 3 ingestion in isolation, with its own dataset and scorers, requires custom plumbing. Build it.

**Cross-modal evaluators.** No off-the-shelf evaluator handles content-and-temporal alignment between audio and visual reliably. Hybrid human-plus-LLM workflows are the current state of the art and need to be built for your specific product.

**Audio and video annotation tooling.** Off-the-shelf options are weak. Build a custom interface; budget a week, expect to revise it monthly as the team's review patterns mature.

**Judge validation and drift monitoring.** Computing kappa once is easy. Computing it on a rolling window, alerting on drift, recalibrating judges, surfacing the recalibration history — this infrastructure does not exist as a platform feature. Build it as a notebook plus a scheduled job.

**Production-to-eval feedback loop.** When a production trace is flagged, getting it into the eval dataset, into the labeling queue, and out as an updated regression test should be one click. It is usually four manual steps. Automate the path.

What you should not have to build: basic trace logging, dataset management, experiment tracking, dashboarding. The platforms (Braintrust, Langsmith, Arize) handle these well. If you find yourself building these, you have chosen the wrong platform or you are over-investing.

Plan for 30–50% of your eval engineering time in the first year to be platform-supplementing rather than platform-using. This is the cost of working in a maturing tooling ecosystem; it will decline over the next two to three years.

---

## Domain Playbooks

### Q44. How should multimodal evaluations differ in healthcare?

Higher cost of factuality failures, asymmetric error costs, regulatory overlay, and a domain-expert-only reviewer pool.

**Factuality dominates Faithfulness.** A clinical output that faithfully summarizes a wrong source is still wrong, and the harm propagates to patient care. Factuality evaluators must be grounded in clinical knowledge — guidelines, formularies, drug interaction databases — not just the input source.

**Asymmetric error costs.** A false negative in diagnostic summarization (missing a finding) is much worse than a false positive (mentioning a finding that turned out benign). Evaluators must reflect this asymmetry — sensitivity bar higher than specificity, reported separately.

**Reviewer pool is constrained.** Only licensed clinicians can reliably evaluate clinical outputs, and clinician time is expensive and scarce. The annotation tool must be ruthlessly efficient, the eval set small and high-quality, and synthetic data is much riskier than in other domains because clinical edge cases are subtle and underrepresented in training data.

**Regulatory overlay.** HIPAA for data handling, FDA scrutiny for products meeting the device threshold, state-level rules for clinical decision support. Evaluation infrastructure must produce auditable records — every label, reviewer, model version, change. Plan for it from day one.

**Specific failure modes.** Drug name confusion, dosage errors, negation flips, demographic misattribution, missed co-morbidities. Each requires a dedicated evaluator with a curated test set.

The playbook: over-invest in Factuality, under-invest in synthetic data, build a clinician-friendly annotation tool that respects clinician time, and treat regulatory documentation as a first-class output of the eval system rather than a bolt-on.

### Q45. How should multimodal evaluations differ in legal and compliance-heavy domains?

Faithfulness dominates absolutely, citation integrity is non-negotiable, the eval set must reflect adversarial use.

**Faithfulness as the master contract.** Legal outputs are referred to and relied on; an unfaithful summary is professionally indefensible. Faithfulness evaluators must be conservative — every claim traceable to a specific span in the source, no exceptions for "general background." Span-level, not paragraph-level: each generated sentence mapped to source spans, unsourced sentences fail by default.

**Citation integrity.** When the system cites cases, statutes, or contract clauses, citations must be correct in form and in content. Citation hallucination has produced sanctions against attorneys. Evaluators must verify both that the cited authority exists and that it stands for the proposition cited — usually requiring integration with legal databases (Westlaw, Lexis, court record APIs).

**Adversarial eval inputs.** Legal users include opposing counsel. The eval set must include inputs designed to exploit known failure modes — ambiguous contract clauses, contradictory case law, jurisdiction-specific edges — not just average-case examples.

**Document parsing as a first-class evaluation surface.** Legal documents are structurally complex — nested clauses, footnotes, exhibits, redlines. Stage 3 ingestion failures are disproportionately damaging because downstream legal reasoning depends on structure. A dedicated document-parsing evaluator is required.

**Confidentiality and privilege.** Trace storage and access controls must respect attorney-client privilege and matter-level access boundaries. Designed into the infrastructure, not added later.

The playbook: span-level Faithfulness evaluation, integrated citation verification, adversarial eval sets, matter-level access controls.

### Q46. How should multimodal evaluations differ for education and tutoring?

Pedagogical correctness, age-appropriateness, learning-outcome alignment, and adversarial student behavior as a first-class scenario.

**Pedagogical correctness is its own contract.** A tutoring response can be factually correct and pedagogically wrong — giving the answer instead of guiding the student to it, providing detail beyond grade level, using analogies that confuse. Pedagogical correctness requires teacher-trained reviewers and rubrics that go beyond Faithfulness and Factuality.

**Age-appropriateness is a hard constraint.** Tone, vocabulary, examples, and content must match the target age. Evaluators must include age-band-specific scorers — what is appropriate for a 14-year-old is not appropriate for an 8-year-old. Age-band misalignment is invisible to most generic evaluators and damages product trust quickly.

**Learning-outcome alignment.** The product is being used to improve specific outcomes — a math skill, reading comprehension, a conceptual understanding. Evaluation must include outcome-level metrics: did students who used the product improve more than a control group. Closer to clinical-trial evaluation than typical AI evaluation, and most teams skip it.

**Adversarial student behavior.** Students will try to get the system to do their homework, give them the answer, or generate inappropriate content. The eval set must include adversarial inputs explicitly — jailbreak attempts, off-topic redirects, content extraction — with a clear rubric for what the system should do in each case.

**Multimodal-specific concerns.** If the product generates visual or audio content, age-appropriate visual style and pronunciation become Output Quality contracts. A geometry diagram with calculus-level notation fails for a middle-schooler regardless of correctness.

The playbook: teacher-reviewer involvement, age-band-specific evaluators, outcome-level evaluation, adversarial coverage.

### Q47. How should multimodal evaluations differ for insurance, finance, and claims?

Numeric correctness is non-negotiable, document structure dominates, regulatory reporting is part of the system, reproducibility is a hard requirement.

**Numeric correctness is the master output contract.** A financial summary with a transposed digit, a dropped decimal, or a misattributed currency unit is materially wrong. Evaluators must include strict numeric verification — every number traced to a specific source location, with format validation. Generic Faithfulness evaluators that pass paragraph-level paraphrases miss numeric errors regularly; numeric-specific evaluators are required.

**Document structure dominates.** Financial documents are heavily structured — tables, line items, footnotes, schedules. Stage 3 ingestion failures are disproportionately damaging. The ingestion evaluator must specifically test table extraction, line-item attribution, and footnote handling.

**Regulatory reporting integration.** Outputs and decisions are subject to regulatory reporting. The eval system must produce evidence that satisfies regulator expectations — model versioning, dataset characteristics, performance over time, change logs. This is infrastructure, not optional, and changes how the system is architected.

**Reproducibility is a hard requirement.** Six months later, the team must be able to reproduce a specific output from a specific input — what model version, what prompt, what retrieval results. This requires comprehensive trace logging and immutable model versioning. Teams used to LLM eval norms ("we'll just use the latest model") are unprepared for the rigor.

**Specific failure modes.** Currency confusion (USD vs EUR), unit confusion (basis points vs percentage points, thousands vs millions), date range errors, jurisdiction confusion, claim-amount errors.

The playbook: numeric-specific evaluators, document-structure-specific ingestion evaluators, regulator-aware infrastructure, immutable trace logging with comprehensive versioning.

### Q48. How should multimodal evaluations differ for robotics and physical AI?

Real-world consequences, physical safety contracts, perception-action coupling, vastly larger input space.

**Physical safety as a master contract.** A robot that misperceives or mis-acts can hurt people or property. Safety contracts — never collide with humans, never exceed force or speed thresholds, always have a recoverable state — have hard pass bars, not soft thresholds.

**Perception-action coupling.** A perception failure that does not lead to a wrong action is much less serious than one that does. Evaluation must cover the perception-action chain end-to-end, not just perception. A vision system misclassifying objects 5% of the time is acceptable if the action policy is conservative; the same rate is dangerous if the policy is aggressive.

**Sim-to-real evaluation.** Simulation is irreplaceable for scale (you cannot run millions of real-world trials), but the gap between sim and real is real and consequential. Eval infrastructure must support both, measure the gap explicitly, and use real-world evaluation where the gap matters.

**Input space is vastly larger than digital products.** Lighting, weather, surface conditions, dynamic actors, sensor noise, hardware drift. Evaluation must include systematic coverage with stress testing at edges. The eval dataset is an order of magnitude larger and must be continuously expanded.

**Long-tail safety events.** The most dangerous failures are rare, and rare events are not captured by standard procedures. Counterfactual evaluation, adversarial scenario generation, and red-team simulation are required.

The playbook: safety-as-master-contract, perception-action chain evaluation, sim-to-real gap monitoring, environmental coverage, adversarial scenario generation.

### Q49. How should multimodal evaluations differ for creative tools?

Subjective quality is a first-class contract, reference-free evaluation dominates, brand and style alignment matter, and the failure space is dominated by Output Quality artifacts.

**Subjective quality is the dominant contract.** For a creative tool, "is this output beautiful, surprising, useful to the artist" is the question, not "is this faithful to a source." Faithfulness and Factuality may not apply at all. Evaluation rubrics are aesthetic and decompositional — composition, color, motion, originality, prompt adherence — calibrated by reference to high-quality human-produced reference work.

**Reference-free evaluation dominates.** There is no "correct" output for "draw me a sunset over mountains." There are better and worse outputs, and evaluation is comparative rather than absolute. Eval sets are built around prompts with paired good/bad outputs as anchors, and pairwise preference comparisons (output A vs output B) are often more reliable than absolute scoring.

**Generation defects as a primary failure category.** Extra fingers, warped text, broken anatomy, video flicker, voice drift in audio. These are the bread-and-butter failures, detectable by trained reviewers and partially by automated evaluators, and the failures users complain about loudest.

**Brand and style alignment as a contract.** When the tool is used by enterprises, outputs must conform to a brand style guide — colors, typography, voice tone, prohibited imagery. A configurable contract, varying by customer, requiring per-customer style rubrics.

**IP and safety as hard constraints.** Outputs that infringe copyright, depict identifiable individuals without consent, or contain prohibited content are categorical failures regardless of artistic quality. Evaluators must include IP and safety checks as gates running before the output reaches the user.

The playbook: decompositional aesthetic rubrics with paired-example anchors, pairwise preference evaluation, dedicated generation-defect detectors, configurable per-customer style contracts, hard gates for IP and safety.

---

## Production, Monitoring & Governance

### Q50. How do I move from offline evals to production monitoring?

Three transitions, sequenced.

**One. Sample production traces into the eval pipeline.** Configure the production system to emit a sampled stream — 1–5% by default, stratified oversampling on segments that matter — into the same trace store the offline eval system uses. The sampling logic is the foundation; without it, production monitoring is impossible.

**Two. Deploy reference-free evaluators against the sampled stream.** Offline eval typically uses reference-based evaluators (compare output to expected output). Production has no expected output. Evaluators must be reference-free — Faithfulness against the input source, Output Quality against intrinsic rubrics, Cross-Modal Consistency against the artifact's own modalities. This is a meaningful shift in evaluator design and most teams underestimate it.

**Three. Build the monitoring loop.** Aggregate scores by cohort and time, visualize on dashboards with confidence intervals, alert on drift beyond thresholds, route alerted traces into the human review queue, feed the human reviews back into evaluator validation. A continuous loop, not a one-time setup.

What changes operationally: offline eval runs when changes are made; production monitoring runs continuously. Offline eval has a fixed dataset; production sees the live distribution. Offline eval has reference outputs; production has only actual outputs. Offline eval is forgiving of slow evaluators; production needs them to keep up with traffic.

The mistake to avoid: trying to use the offline eval dataset as the production monitoring set. They are different artifacts with different purposes. Both are needed; neither replaces the other.

### Q51. What metrics should be monitored continuously?

A small number of contract-level metrics, segmented by cohort and feature, with confidence intervals.

**The Five Contract scores, separately.** Ingestion Fidelity, Faithfulness, Factuality, Cross-Modal Consistency, Output Quality. Each as a percentage of sampled traces over a rolling window (24 hours, 7 days, 30 days). Tracked separately, never composited for the primary dashboard.

**Per-stage error rates.** ASR error rate, OCR error rate, retrieval miss rate, generation defect rate. These are leading indicators — they shift before contract scores shift, and they tell you which fix surface to investigate.

**Cohort cuts.** The same metrics segmented by user type, feature, input format, and other dimensions specific to the product. Aggregate metrics hide cohort-level failures, and cohort failures are where churn happens.

**Latency and cost.** End-to-end latency at p50, p95, p99. Cost per trace. Not quality metrics directly, but they constrain quality decisions — a Cross-Modal Consistency check that doubles cost may not be deployable.

**User-signal metrics.** Thumbs-up / thumbs-down rate, edit rate, retry rate, escalation rate. Noisy and biased, but the only direct user-quality signal, and they correlate with the contract metrics.

What to not monitor as a primary metric: composite quality scores, generic "helpfulness" or "engagement," vendor-supplied benchmark scores. Each is decoration without a clear decision attached.

The dashboard discipline is fewer metrics, better understood. Five contract scores plus five leading indicators plus three cohort cuts beats fifty undifferentiated dashboards. The team should be able to recite the five from memory.

### Q52. How do I detect evaluator drift or judge drift?

Three mechanisms running concurrently.

**Periodic kappa recomputation.** Every month, sample 50–100 fresh traces, have the human reviewer re-label them, run the LLM-as-judge, compute kappa. If kappa drops by more than 0.1 from baseline, the judge has drifted and must be recalibrated. The canonical drift detection mechanism, and most teams do not run it.

**Score distribution monitoring.** Track the distribution of judge scores over time — pass percentage, fail percentage, histogram shape. If the distribution shifts significantly (more passes than expected, fewer failures than human reviewers find), the judge has drifted. Distribution shift alone is not proof — the underlying data may have shifted — but it is a strong signal that triggers investigation.

**Disagreement sampling.** When the LLM-as-judge disagrees with another automated check or another judge, route those disagreements into human review at a higher rate than baseline. Disagreements are diagnostic — they reveal where the judge's reasoning is unstable and they generate fresh labeled examples for recalibration.

The honest reality of judge drift: multimodal judges drift faster than text judges because the underlying multimodal models are evolving faster, input distributions shift faster, and failure modes are less stable. A text Faithfulness judge calibrated in March may still be reliable in September; an audio cross-modal judge calibrated in March is probably stale by June. Plan for quarterly recalibration as the default and shorten it for newer products.

The drift you cannot detect this way is when both the judge and the human reviewer drift in the same direction. The defense is to maintain a permanent reference set of historical traces with original labels, and periodically re-label them.

### Q53. How should multimodal evals integrate into CI/CD and release gates?

A small fast regression suite as a gating check, a slower comprehensive suite as a non-blocking signal, a manual review gate for substantial changes.

**Regression suite as gate.** A curated set of 100–500 traces, fast to evaluate (no human review, no slow LLM judges), covering known failure modes the team has decided must not regress. Runs on every PR and release. Fails the build if any trace's score regresses by more than a threshold. Total wall time under 10 minutes.

**Comprehensive suite as signal.** A larger set (1,000–5,000) with the full battery including LLM-as-judge. Runs nightly against the latest build. Results posted to a dashboard, alerts fired on significant regressions, but does not block. Total wall time can be hours.

**Manual review gate for substantial changes.** Any change to the synthesis prompt, model version, ingestion model, or cross-modal alignment logic triggers manual review by the benevolent dictator. A sample of 50 fresh traces, scored against the full rubric. The change does not ship until the review passes.

What not to do. Do not run the comprehensive suite as a gate — too slow, too expensive, and LLM-as-judge variance will block legitimate releases. Do not skip manual review for substantial changes — automated evaluators miss novel failure modes by definition. Do not have a single gate covering everything — different changes warrant different scrutiny.

The CI/CD philosophy: fast feedback for routine changes, deep scrutiny for risky changes, automation for everything that is automatable. Manual review is a feature of the system, not a bug to be eliminated.

### Q54. What does a progressive rollout gate waterfall look like?

A multi-stage exposure ramp where each stage is gated by quality metrics, with explicit pass criteria, hold conditions, and rollback triggers.

**Stage 0 — internal dogfood.** Team-only exposure. Pass: no critical failures, qualitative review passes. Duration: 3–7 days.

**Stage 1 — design partners.** 5–10 selected organizations. Pass: contract scores within 5% of dogfood baseline, partner feedback positive. Duration: 1–2 weeks.

**Stage 2 — beta cohort.** 1–5% of total user base. Pass: contract scores stable, user-signal metrics within tolerance, no novel failure modes. Duration: 2–4 weeks.

**Stage 3 — gradual ramp.** Exposure increased to 10%, 25%, 50%, 100% over a defined schedule. Each step gated by stable metrics for at least 48 hours. Pass: no regression in contract scores, no spike in support tickets, no novel cohort failures.

**Stage 4 — full deployment.** 100% with continuous monitoring. The rollout is not "done" — monitoring continues indefinitely.

**Hold conditions.** A contract score regression of 3–10% triggers a hold at the current exposure level. Investigation, fix, re-validation before the ramp resumes.

**Rollback triggers.** A contract score regression of more than 10%, a safety-contract violation, or a novel failure mode affecting a substantial cohort triggers automatic rollback to the previous stage. Automated, not a meeting.

The discipline is that each gate is a real decision point, not a formality. Most teams have the gates on paper and pass criteria as aspirations; the work is making each gate actually block when criteria are not met. A gate that has never blocked a release is not a gate.

### Q55. What should trigger a ship, hold, rollback, or ramp decision?

Specific contract-score conditions, mapped explicitly to specific decisions, with the decision authority pre-assigned.

**Ship — proceed to the next exposure stage.** All five contract scores at or above baseline. No novel failure modes in sampled review. User-signal metrics stable or improving. Authority: quality forum chair with input from the benevolent dictator.

**Hold — pause at the current stage.** One or more contract scores regressed by 3–10% from baseline, no critical safety issues, new failure modes appearing at low volume. Authority: the benevolent dictator can call a hold unilaterally; the quality forum reviews within 48 hours.

**Rollback — return to the previous stage or version.** A contract score regressed by more than 10%, a safety-contract violation, or a novel failure mode affecting a substantial cohort. Authority: the on-call engineer can trigger rollback unilaterally for safety; the quality forum is informed, not consulted.

**Ramp — increase exposure to the next stage.** All ship criteria met, plus stable metrics for the minimum dwell time at the current stage (24–48 hours typical), plus no open investigations. Authority: same as ship.

The discipline is that criteria are written down before the rollout begins, decision authority is named before the rollout begins, and decisions are made against the criteria not against vibes. A rollout where criteria are decided in real time is one where the team will rationalize whatever metrics they get.

The hardest decision is hold. Ship is exciting, rollback is dramatic, hold is uncomfortable — the product is in the world, the team is committed, and pausing feels like failure. The forum's most important function is to make hold a normal decision rather than an exceptional one.

### Q56. How do I create an ongoing governance cadence?

Weekly review, monthly audit, quarterly recalibration. Three cadences, three different forums, three different agendas.

**Weekly review (60 minutes).** Attendees: PM, ML, design, domain expert. Agenda: contract score trends, novel failure modes from the past week, sampled trace review (5–10 traces, focused on edge cases), open issues from the optimization backlog. Output: prioritized work for the next week, decisions on hold/ship for active rollouts. The operational heart of the system.

**Monthly audit (90 minutes).** Attendees: weekly forum plus engineering leadership and risk/compliance. Agenda: full contract score history, judge drift report, regression suite expansion proposals, evaluator validation status, infrastructure cost review. Output: cross-team commitments, infrastructure investment decisions, risk-flag escalations.

**Quarterly recalibration (full day).** Attendees: monthly audit forum plus product leadership and at least one external reviewer. Agenda: review of the eval framework itself (are we measuring the right things), benchmark against external products and vendor capabilities, recalibration of judges against fresh human-labeled data, eval dataset refresh, retirement of obsolete evaluators. Output: framework changes, dataset updates, strategic decisions about platform and tooling.

What makes the cadence work. Each forum has a designated chair accountable for agenda, decisions, and follow-through. Each produces written outputs visible to the team. Each has authority — decisions are made, not deferred. The cadences are on the calendar before the year starts, and they happen even when the team is busy.

What kills it. The weekly becomes a status meeting and stops producing decisions. The monthly becomes a slideshow and stops surfacing risks. The quarterly is skipped because "we did it last year." Once any of these decay, the eval system is no longer governed.

The cadence is the eval system. Dashboards and evaluators are instruments; the cadence turns the instruments into decisions. A team with weak evaluators and a strong cadence will improve. A team with strong evaluators and a weak cadence will not.
