---
name: "stateless-reducer-orchestration"
description: "Structures agent control loops as deterministic, stateless reducers managing unified business and execution state."
license: Apache-2.0
---

# Stateless Reducer Orchestration

## Overview
This skill operationalizes Factors 5 and 12, organizing the core agent loop as a deterministic, stateless reducer that processes incoming events against a unified, durable state object.

## Key Capabilities
- **Pure Function Mapping**: Models transitions as `(State, Event) -> (NextState, OutgoingActions)`.
- **Unified State Schema**: Consolidates conversation history, execution variables, and domain business records into one schema.
- **Deterministic Replay**: Replays event logs from zero state to reproduce bugs and audit decision pathways.

## Operational Workflow
1. **State Definition**: Author typed state interfaces covering business entities and execution pointers.
2. **Event Dispatch**: Ingest incoming events (user messages, tool results, human approvals, timeouts).
3. **Reducer Evaluation**: Compute the next state immutably without side effects.
4. **Action Execution**: Emit and trigger resulting actions (LLM calls, tool executions, pause signals).
