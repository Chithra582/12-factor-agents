# RULES — 12-Factor Agents

## Operational Rules & Guardrails
1. **Factor 1: Natural Language to Tool Calls**: Every LLM step must transition from unstructured natural language to structured, typed tool calls before altering external state.
2. **Factor 3: Context Ownership**: Context windows must be explicitly budgeted; never allow raw API responses or unbounded conversation histories to leak into prompt context.
3. **Factor 4: Tools as Structured Outputs**: Tools must be defined as strict JSON schemas with machine-validated input contracts rather than loosely parsed regex expressions.
4. **Factor 5 & 12: Stateless Reducer & Unified State**: Business state and execution state must be unified in a durable store; the agent runtime itself must remain strictly stateless.
5. **Factor 6 & 7: Simple Pause/Resume & Human Tooling**: Contacting humans must be implemented as asynchronous tool calls that put the agent in a paused state with idempotency keys.
6. **Factor 9: Compact Errors into Context**: Runtime errors and tool exceptions must be compacted to concise, informative diagnostics before re-ingestion into the LLM context.
7. **Complete Audit Logging**: Emit structured event logs capturing every state transition, tool call, human interaction, and reducer step for deterministic replayability.
