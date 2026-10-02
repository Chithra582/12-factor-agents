---
name: "human-in-the-loop-tooling"
description: "Implements pause/resume lifecycles and contacts humans through typed structured tool calls."
license: Apache-2.0
---

# Human-in-the-Loop Tooling

## Overview
This skill operationalizes Factors 6 and 7, treating human communication and authorization not as ad-hoc interruptions, but as first-class, structured tool calls with clean pause and resume semantics.

## Key Capabilities
- **Human-as-a-Tool Model**: Exposes approval requests, input solicitations, and escalations as typed tool signatures.
- **Idempotent Pause/Resume**: Freezes agent reducer state in durable storage while awaiting human response webhooks.
- **Timeout & Escalation**: Handles unresponded approval requests with configurable fallback policies.

## Operational Workflow
1. **Tool Invocation**: Agent emits a `request_human_approval` tool call with action details and justification.
2. **State Freezing**: Persist execution state and transition agent to `PAUSED` status.
3. **Notification Dispatch**: Send interactive approval request to human channel (Slack, email, UI).
4. **Resumption Event**: Ingest human approval event webhook, update state, and resume reducer execution.
