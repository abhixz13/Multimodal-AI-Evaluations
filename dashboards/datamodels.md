# Multimodal AI Evaluations Platform — Data Model

**Core entity definitions for the MuE Platform frontend.**
TypeScript interfaces for all platform entities. Product-specific values (sub-contract labels, failure tags, pipeline stage labels) are injected at the product level — the schema is always the same.

---

## Entities

### EvalProduct
The root entity. Defines the product being evaluated, its pipeline architecture, and its evaluation configuration.

```typescript
interface EvalProduct {
  id: string                         // url-safe slug, e.g. "huxe-v1"
  name: string                       // display name, e.g. "Huxe"
  category: ProductCategory
  description: string
  maturityLevel: 1 | 2 | 3 | 4 | 5
  maturityPartial: number            // e.g. 1.5 = between L1 and L2
  currentLifecycle: "dev" | "release" | "production"
  amiPhase: "analyze" | "measure" | "improve"
  createdAt: string                  // ISO 8601
  briefSubmitted: boolean            // gates all eval runs
  pipelineStageLabels: Record<1|2|3|4|5|6|7|8|9, string>
  inputModalities: InputModality[]
  outputModalities: OutputModality[]
  hasInteractionLayer: boolean
  audioNativeRequired: boolean
}

type ProductCategory =
  | "document-to-audio"
  | "text-to-video"
  | "document-to-image"
  | "async-video-summary"
  | "multimodal-agent"
  | "custom"

type InputModality =
  | "text" | "document-pdf" | "document-structured"
  | "image" | "video" | "audio" | "email"
  | "calendar" | "web" | "social" | "voice" | "other"

type OutputModality =
  | "text" | "audio" | "video" | "image"
  | "transcript" | "structured" | "other"
```

---

### EvalRun
A single evaluation run against a product. Many runs per product. Each run produces a set of ContractScores and Traces.

```typescript
interface EvalRun {
  id: string                         // e.g. "run-047"
  productId: string
  lifecycle: "dev" | "release" | "production"
  runAt: string                      // ISO 8601
  caseCount: number
  durationMs: number
  triggeredBy: "manual" | "ci" | "scheduled"
  overallVerdict: "pass" | "hold" | "ramp" | "rollback"
  notes: string
}
```

---

### ContractScore
One score per sub-contract per run. The Five Quality Contracts (C1–C5) plus C-SAFETY each have one or more sub-contracts.

```typescript
interface ContractScore {
  runId: string
  contract: ContractId
  subContract: string                // product-specific key, e.g. "email-parse-plain"
  subContractLabel: string           // display label, e.g. "Email Parse — Plain Text"
  score: number | null               // null = not yet measured
  gate: number                       // threshold from T-01 Brief
  evidenceLevel: EvidenceLevel
  status: "pass" | "warn" | "fail" | "unknown" | "override"
  trendDirection: "up" | "down" | "flat" | null
  trendHistory: number[]             // last 14 data points for sparkline
  zeroToleranceBreach: boolean
  notes: string
}

type ContractId = "C1" | "C2" | "C3" | "C4" | "C5" | "C-SAFETY"

type EvidenceLevel = "SIM" | "EST" | "REV" | "INF" | "LIVE"
// SIM  — Simulated: structured reasoning, no trace access
// EST  — Estimated: calibrated against public data sources
// REV  — Review-derived: directly supported by public evidence
// INF  — Architectural inference: derived from known pipeline patterns
// LIVE — Live measured: actual traced production data
```

---

### Trace
One evaluation trace — a single input run through the product pipeline, producing spans, contract scores, and (optionally) audio-native scores.

```typescript
interface Trace {
  id: string                         // e.g. "eval-2847-huxe"
  runId: string
  productId: string
  inputSummary: string               // human-readable input description
  inputModalities: InputModality[]
  outputModalities: OutputModality[]
  outputDurationMs?: number          // for audio/video outputs
  userProfile?: string               // e.g. "profile-a-heavy-email"
  contractScores: ContractScore[]
  spans: Span[]
  audioNativeScores?: AudioNativeScore[]
  failureTags: string[]              // namespaced tags, e.g. "ingestion.email.thread_collapse"
  flagged: boolean
  addedToDataset: boolean
  causalChainsTriggered: string[]    // chain IDs triggered in this trace
}
```

---

### Span
One pipeline stage span within a trace. Spans can be nested (parentSpanId). Colored and labeled by their stage number.

```typescript
interface Span {
  id: string
  traceId: string
  parentSpanId: string | null
  stage: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
  name: string                       // e.g. "Email Thread Reader"
  startMs: number
  durationMs: number
  status: "pass" | "warn" | "fail"
  failureTags: string[]
  notes: string
  contractImpact: ContractId[]       // which contracts this span affects
}
```

---

### AudioNativeScore
One score per audio evaluation dimension per trace. Only populated for products where `audioNativeRequired = true`.

```typescript
interface AudioNativeScore {
  traceId: string
  dimension: AudioNativeDimension
  score: number
  floor: number
  evidenceLevel: EvidenceLevel
  status: "pass" | "warn" | "fail"
  notes: string
  timestampMarkers: number[]         // ms positions of notable audio events
}

type AudioNativeDimension =
  | "interruption_recovery"          // naturalness of recovery from user interruption
  | "host_differentiation"           // distinctness of two hosts over a session
  | "prosodic_appropriateness"       // tone, stress, and rhythm fit to content
  | "topic_transition_smoothness"    // naturalness of transitions between topics
  | "conversational_latency"         // latency feel of responses to interaction
  | "energy_arc"                     // energy level trajectory over full session
```

---

### FailureMode
A named, recurring failure pattern. Product-specific. Source of truth for all trace tagging. Tags use a namespaced dot notation: `junction.modality.failure_name`.

```typescript
interface FailureMode {
  tag: string                        // e.g. "ingestion.email.thread_collapse"
  productId: string
  displayName: string                // e.g. "Email Thread Collapse"
  description: string
  originStage: number                // pipeline stage where failure originates
  blastRadius: number                // count of downstream failures resolved by fixing this
  contractsAffected: ContractId[]
  causalChainIds: string[]
  occurrenceCount: number
  evidenceLevel: EvidenceLevel
  fixSurface: string                 // pipeline stage + component description
  backlogItemId: string | null
  status: "open" | "in-progress" | "resolved"
}
```

---

### CausalChain
A named upstream → downstream failure relationship. Explains why an upstream fix ranks higher than a faster downstream fix — the blast radius mechanism.

```typescript
interface CausalChain {
  id: string                         // e.g. "HC-01"
  productId: string
  name: string                       // "[Root Failure] → [Downstream Failure]"
  story: string                      // plain-language explanation of the causal mechanism
  rootTag: string                    // upstream failure tag
  rootStage: number
  downstreamTags: string[]           // downstream failure tags resolved by fixing root
  estimatedTraceCount: number
  evidenceLevel: EvidenceLevel
  expectedLiftAfterFix: string       // e.g. "C2: 72% → 87%"
  backlogItemId: string | null
  blastRadius: number
  contractsAffected: ContractId[]
  fixStatus: "not-started" | "in-backlog" | "in-progress" | "resolved"
}
```

---

### BacklogItem
One fix item. Safety overrides (`isOverride: true`) bypass the priority formula entirely and render in a separate zone above all formula-ranked items.

Priority formula: **Score = (S × F × C × B × U) ÷ TTL**

```typescript
interface BacklogItem {
  id: string
  productId: string
  description: string
  tag: string                        // linked failure mode tag
  isOverride: boolean                // true = safety override; skips formula
  overrideReason?: string
  formulaScore?: number
  s: 1 | 2 | 3                      // Severity:      High=3 / Medium=2 / Low=1
  f: number                          // Frequency:     0.0 – 1.0
  c: 1 | 2 | 3                      // Confidence:    High=3 / Medium=2 / Low=1
  b: number                          // Blast Radius:  downstream failures resolved
  u: 1 | 2 | 3 | 4 | 5              // Upstream-ness: S1=5 / S2=4 / ... / S9=1
  ttl: number                        // Time to deploy in days
  stage: number
  causalChainIds: string[]
  evidenceLevel: EvidenceLevel
  rank: number
  sprintStatus: "not-started" | "in-progress" | "done"
}
```

---

### DriverAnalysis
A structured root-cause analysis (T-13). Triggered by any alert, backlog item, or causal chain. Produces a T-14 Decision Memo on completion.

```typescript
interface DriverAnalysis {
  id: string
  productId: string
  triggeredBy: "alert" | "manual" | "backlog-item" | "causal-chain"
  triggerSource: string              // source entity ID
  runId: string
  createdAt: string
  status: "draft" | "complete"
  observedPattern: string            // Step 1: what metric, magnitude, profiles affected
  hypotheses: DriverHypothesis[]     // Step 2: structured hypotheses
  confirmedCause: string             // Step 5: root cause
  secondaryCause?: string
  amplifiers: string[]               // factors that make the failure worse in production
  primaryFix: string                 // Step 3: primary fix surface
  secondaryFix?: string
  expectedLift: string               // e.g. "C1: 62% → 85%"
  validationPlan: string             // how to confirm the fix worked
  linkedMemoId?: string              // T-14 Decision Memo ID
}

interface DriverHypothesis {
  id: string
  label: string                      // "H1", "H2", etc.
  stage: "ingestion" | "retrieval" | "generation" | "output" | "other"
  hypothesis: string
  testMethod: string
  result: string
  confirmed: boolean
  evidenceLevel: EvidenceLevel       // required before Step 4 unlocks
}
```

---

### Judge
One member of the multi-rater evaluation panel. Kappa is calculated per judge pair after blind labeling is complete.

```typescript
interface Judge {
  productId: string
  role: string                       // e.g. "Content Judge", "Audio Judge"
  profile: string                    // e.g. "Senior journalist / AI news editor"
  jurisdiction: string[]             // evaluation domains this judge covers
  labelingProgress: number
  labelingTarget: number
  kappa?: number                     // Cohen's kappa, available after labeling complete
  kappaFloor: number                 // minimum acceptable kappa (typically 0.70)
  status: "pending" | "in-progress" | "complete"
  isTiebreaker: boolean
  escalationCount?: number
  nextRecalibrationDate?: string
}
```

---

### DriftEntry
One entry for each of the five drift types. Reviewed on a governance cadence, not per eval run.

```typescript
interface DriftEntry {
  productId: string
  driftType: "input" | "ingestion" | "model" | "judge" | "consumption"
  riskLevel: "high" | "medium" | "low"
  observation: string                // product-specific observation about this drift type
  detectionMechanism: string         // canary corpus / kappa recalibration / engagement tracking
  lastChecked: string
  nextCheckDue: string
  status: "active" | "monitoring" | "resolved"
}
```

---

## Entity Relationship Summary

```
EvalProduct
  └── EvalRun (many per product)
        └── Trace (many per run)
              ├── Span (many per trace, nested by parentSpanId)
              ├── ContractScore (per sub-contract per trace)
              └── AudioNativeScore (per dimension, if audioNativeRequired)

EvalProduct
  ├── FailureMode (product-specific taxonomy)
  │     └── CausalChain (chains between failure modes)
  ├── BacklogItem (includes override and formula-ranked)
  │     └── DriverAnalysis (one per investigation)
  ├── Judge (panel members)
  └── DriftEntry (one per drift type)
```

---

## Design System Mapping

Each entity maps to one or more UI components:

| Entity | Primary Screen | Key Component |
|---|---|---|
| `EvalProduct` | Mission Control | `ContractScoreCard`, `PipelineBar`, `SafetyOverrideStrip` |
| `ContractScore` | Five Contracts | `SubContractScoreRow`, `EvidenceBadge`, `ScoreSparkline` |
| `Trace` | Trace Explorer | `SpanRow`, `ContractVerdictBlock`, `AudioNativeDimensionRow` |
| `FailureMode` | Failure Mode Library | `FailureModeCard`, `StageOriginChart`, `BlastRadiusLeaderboard` |
| `CausalChain` | Causal Chains | `CausalChainCard`, `BlastRadiusExplanation`, `ChainDiagram` |
| `BacklogItem` | Backlog (T-17) | `BacklogItem`, `SafetyOverrideZone`, `ScoreCalculator` |
| `DriverAnalysis` | Driver Analysis (T-13) | `DriverAnalysisStep`, `HypothesisCard`, `FixSurfaceRow` |
| `Judge` | Judge Panel | `JudgeRosterRow`, `KappaPairTable`, `DisagreementQueueItem` |
| `DriftEntry` | Five Drift Monitor | `DriftEntry`, `CanaryCorpusPanel`, `ConsumptionDriftChart` |

---

*Framework: Multimodal AI Product Evaluation · Multimodal-AI-Evaluations*
