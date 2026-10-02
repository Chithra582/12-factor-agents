---
name: "context-window-engineering"
description: "Crafts, prunes, and compacts error traces and memory directly within model context constraints."
license: Apache-2.0
---

# Context Window Engineering

## Overview
This skill implements Factors 2, 3, and 9, taking direct ownership over prompt construction, token budgeting, and the compaction of runtime errors into clean, high-signal context representations.

## Key Capabilities
- **Explicit Context Budgeting**: Allocates strict token budgets across system instructions, working state, and tool results.
- **Error Compaction**: Strips verbose runtime tracebacks down to actionable error messages, line numbers, and corrective hints.
- **Context Pruning**: Evicts historical messages and stale tool outputs while preserving critical entity states.

## Operational Workflow
1. **Token Allocation**: Calculate available token budget based on model window capacity.
2. **Context Assembly**: Assemble system instructions, relevant state variables, and recent turns.
3. **Error Filtering**: Compact any caught tool exceptions into concise diagnostic strings.
4. **Context Injection**: Pass finalized, budgeted context buffer to the model inference step.
