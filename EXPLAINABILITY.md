# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **12-Factor Agents** (`twelve-factor-agents`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** 12-Factor Agents (`twelve-factor-agents`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Agent Architecture & Production Engineering Principles  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

12-Factor Agents is an architectural methodology, reference runtime, and auditing framework engineered to transform autonomous AI agents from experimental script loops into reliable, production-grade software applications. Rather than treating agents as chaotic while-loops that feed arbitrary tool feedback back into monolithic prompts, 12-Factor Agents models agent execution as a deterministic, stateless reducer. State is unified across business data and execution pointers, human contact is treated as a first-class structured tool call, and context windows are strictly engineered and budgeted.

### 1. Decision Architecture

The user event intake, state unification, context budget compilation, stateless reducer execution, and action dispatch pipeline operates across a deterministic, five-stage architecture:

```
Incoming Event (User Message / Tool Result / Human Approval / Cron Trigger)
    │
    ▼
[Stage 1: Event Ingestion & State Hydration]
    │  - Ingests incoming event payload with idempotency key and correlation ID
    │  - Hydrates unified business and execution state from durable storage (PostgreSQL/Redis)
    │  - Enforces stateless reducer invariants: zero in-memory ephemeral state dependencies
    ▼
[Stage 2: Context Window Engineering & Error Compaction]
    │  - Constructs tight, budgeted prompt context according to explicit token allocations
    │  - Compacts raw runtime errors and tool stack traces into high-signal summaries (Factor 9)
    │  - Evicts stale conversational turns and redundant payload artifacts
    ▼
[Stage 3: LLM Inference & Structured Tool Formulation]
    │  - Evaluates current state against task objectives via frontier reasoning LLM
    │  - Restricts outputs strictly to validated, typed tool calls (Factor 1 & Factor 4)
    │  - Verifies tool arguments against JSON Schema definitions prior to execution
    ▼
[Stage 4: Stateless Reducer State Transition]
    │  - Pure function mapping: `(CurrentState, IncomingEvent) -> (NextState, OutgoingActions)`
    │  - If human verification is required, emits `request_human_approval` tool call (Factor 7)
    │  - Transitions agent status to `PAUSED` or `ACTIVE` with simple APIs (Factor 6)
    ▼
[Stage 5: State Persistence & Action Dispatch]
    │  - Commits updated unified state atomically to durable storage
    │  - Dispatches resulting external tool actions, HTTP webhooks, or user responses
    │  - Emits structured telemetry event traces for deterministic replayability
    ▼
Durable Unified State Commit & Auditable 12-Factor Event Log
```

### 2. Decision Logic & Routing Formulations

12-Factor Agents evaluates architectural compliance, state transition validity, and context budget efficiency using deterministic mathematical models:

1. **12-Factor Architecture Compliance Score ($S_{\text{compliance}}$)**:
   $$S_{\text{compliance}} = \frac{1}{12} \sum_{i=1}^{12} F_i$$
   where $F_i \in [0, 1]$ represents compliance with Factor $i$ (e.g., $F_1$: tool structured output, $F_3$: context window ownership, $F_5$: unified state, $F_{12}$: stateless reducer). A production architecture passes the audit gate only when $S_{\text{compliance}} \ge 0.85$.

2. **Context Window Efficiency Ratio ($E_{\text{context}}$)**:
   $$E_{\text{context}} = \frac{T_{\text{signal}}}{T_{\text{total}}} = \frac{T_{\text{system}} + T_{\text{state}} + T_{\text{compacted\_error}}}{T_{\text{system}} + T_{\text{state}} + T_{\text{raw\_payloads}}}$$
   where $T_{\text{signal}}$ represents high-value semantic tokens and $T_{\text{raw\_payloads}}$ contains uncompacted stack traces and raw JSON blobs. The compactor ensures $E_{\text{context}} \ge 0.75$.

### 3. Thresholding & Refusal Decision Criteria

12-Factor Agents enforces strict operational guardrails and architectural constraints:
- **Refusal to Execute Unstructured Text Output as Action**: Agent actions must map to validated JSON schemas; raw markdown commands or unvalidated shell strings are rejected (`ERR_UNSTRUCTURED_ACTION_REJECTED`).
- **Refusal of Out-of-Band State Mutations**: State mutations occurring outside the formal reducer event loop are blocked (`ERR_OUT_OF_BAND_MUTATION_FORBIDDEN`).
- **Token Budget Ceilings**: Context payloads exceeding allocated token limits (e.g., 8,000 tokens for working context) trigger immediate truncation (`WARN_TOKEN_BUDGET_EXCEEDED`).
- **Human Approval Mandatory Gate**: Destructive actions (financial disbursement, database drop, production deploy) must route through `human_approval_tool` (`ERR_HUMAN_APPROVAL_REQUIRED`).

### 4. Fallback Decision Mechanism

Continuous operational stability and deterministic recovery are guaranteed through multi-tier fault recovery:
- **Provider & Model Cascade**: When the primary foundation model provider experiences rate limits (HTTP 429) or service outages, the orchestrator cascades across configured secondary endpoints.
- **Event Replay Recovery**: If a worker process crashes mid-step, the stateless reducer re-hydrates from the last persisted state checkpoint and replays uncommitted events.
- **Human Escalation Timeout**: If an asynchronous human approval request remains unacknowledged after the timeout window (e.g., 1 hour), the agent executes a safe cancel fallback and notifies administrators.

### 5. Human-in-the-Loop Governance

Human operators retain supreme authority and operational oversight:
- **First-Class Human Tool Calls**: Contacting humans is treated as a native structured tool call (`contact_human_tool`), affording full tracking of requests, rationale, and responses.
- **Idempotent Pause & Resume**: Agents pause cleanly in durable storage without holding open network sockets or active process threads.
- **Deterministic Time-Travel Replay**: The complete event history allows developers to replay past agent decisions step-by-step from zero state for transparency and auditing.

---

## The Data It Uses

12-Factor Agents operates under strict principles of data minimization, state isolation, and explicit context engineering.

### 1. Ingested Input Data

The agent processes only operational assets necessary to advance the stateless reducer:
- **Incoming Events**: User prompts, tool completion payloads, human approval decisions, and timer ticks.
- **Unified State Snapshots**: Durable business domain records and execution thread state pointers.
- **Diagnostic Traces**: Tool execution exit codes, error messages, and output summaries.

### 2. Configuration & Reference Data

- **Prompt Templates**: Versioned markdown files containing system directives and personality anchors (Factor 2).
- **Tool JSON Schemas**: Strict schema definitions specifying input argument types, descriptions, and required fields.
- **12-Factor Rulesets**: Architectural compliance evaluation rubrics and scoring parameters.

### 3. Base Model & Inference Lineage

- **Deterministic Reducer Runtime**: TypeScript/Python reducer functions, state stores, and event dispatchers run with 100% determinism.
- **Foundation LLMs**: High-performance reasoning models (GPT-4o, Claude 3.5 Sonnet) utilized for natural language intent translation and structured tool selection.
- **Zero Training on State Data**: Customer business state, execution event logs, and user inputs are never utilized for model fine-tuning or retraining.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against prompt injection through untrusted tool returns, excessive agency, and state poisoning.
- **Durable Persistence Isolation**: Unified state resides strictly within developer-managed database instances (PostgreSQL, DynamoDB, Redis).
- **Automated PII & Secret Redaction**: Credentials, tokens, and personally identifiable information are scrubbed before prompt assembly and telemetry export.
- **Zero Commercial Monetization**: Application state data, event histories, and developer prompts are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of 12-Factor Agents ensures successful deployment.

### 1. Non-Deterministic Model Variance
- **Limitation**: While the reducer framework is 100% deterministic, underlying generative LLMs exhibit stochastic variation in output tokens.
- **Mitigation**: Temperature is pinned to 0.0, and structured outputs (JSON schema mode) enforce deterministic output syntax.

### 2. Long-Horizon Human Response Latency
- **Limitation**: Workflows paused awaiting human review may remain suspended for hours or days if human reviewers are unavailable.
- **Mitigation**: Configurable timeout policies, automated escalation reminders, and default-safe cancellation actions prevent hanging states.

### 3. External API Idempotency Gaps
- **Limitation**: Retrying actions against external third-party legacy APIs that lack idempotency keys risks duplicate side effects.
- **Mitigation**: The runtime assigns client-generated idempotency tokens and records outgoing action receipts in unified state.

### 4. High-Frequency Event Contention
- **Limitation**: Rapid bursts of concurrent events for the same agent session can create optimistic concurrency lock conflicts in the state store.
- **Mitigation**: The framework implements per-session sequential event queuing and atomic compare-and-swap state commits.

### 5. Multi-Megabyte Tool Output Overhead
- **Limitation**: Ingesting massive raw tool outputs (e.g., 50MB SQL dumps) directly into reducer memory exhausts runtime resources.
- **Mitigation**: Large tool outputs are persisted out-of-band in object storage, with only compact metadata references passed to the reducer.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested incoming events, unified state & diagnostic traces | Section 1 | Verified |
| - Configuration, prompt templates & tool JSON schemas | Section 2 | Verified |
| - Base model lineage & deterministic reducer runtime | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Non-deterministic model variance | Section 1 | Verified |
| - Long-horizon human response latency | Section 2 | Verified |
| - External API idempotency gaps | Section 3 | Verified |
| - High-frequency event contention | Section 4 | Verified |
| - Multi-megabyte tool output overhead | Section 5 | Verified |
