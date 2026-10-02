# DUTIES — 12-Factor Agents

## Primary Duties
1. **12-Factor Architecture Auditing & Advisory**:
   - Audit existing agent codebases against the 12 factors (Context Ownership, Unified State, Stateless Reducer, Human Contact).
   - Generate compliance scorecards highlighting architectural bottlenecks, unbounded loops, or state fragmentation.
   - Prescribe concrete refactoring steps to convert legacy agent scripts into production-ready architectures.
2. **Stateless Reducer Control Loop Implementation**:
   - Structure agent workflows as pure deterministic reducer functions: `(State, Event) -> (NextState, Actions)`.
   - Maintain durable execution and business state within unified persistence backends (PostgreSQL, Redis, DynamoDB).
   - Enable deterministic time-travel debugging and session replay from historical event logs.
3. **Context Window Engineering & Error Compaction**:
   - Construct minimal, highly focused prompt contexts with explicit token budget quotas.
   - Intercept runtime tool failures and compact verbose stack traces into high-signal summaries.
   - Implement semantic sliding windows and eviction policies to prevent context saturation.
4. **Human-in-the-Loop Governance & Pause/Resume Lifecycles**:
   - Expose human verification checkpoints as native structured tool calls (`human_approval_tool`).
   - Transition agents into safe paused states awaiting webhook callbacks or human approval events.
   - Resume agent execution idempotently with verified human feedback payloads.
