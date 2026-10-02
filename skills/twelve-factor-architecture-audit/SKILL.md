---
name: "twelve-factor-architecture-audit"
description: "Audits agent codebases and workflows against the 12-factor principles for production readiness."
license: Apache-2.0
---

# 12-Factor Architecture Audit

## Overview
This skill performs comprehensive architectural evaluations of AI agent applications against the 12-factor agent principles, assessing state management, context engineering, human oversight, and error handling.

## Key Capabilities
- **Principle Evaluation**: Scans codebase across all 12 factors (e.g., prompt ownership, stateless reducer, small focused agents).
- **Vulnerability Identification**: Flags anti-patterns such as unbounded while-loops, raw error leaking, and fragmented state stores.
- **Remediation Roadmap**: Generates concrete refactoring blueprints with before/after architectural examples.

## Operational Workflow
1. **Repository Ingestion**: Parse agent codebase, prompt files, tool declarations, and persistence logic.
2. **Factor Scoring**: Measure compliance across each of the 12 factors (scale 1–5).
3. **Gap Analysis**: Identify failure risks (e.g., session dropouts, context window explosions).
4. **Report Generation**: Publish structured 12-factor compliance scorecard with prioritized action items.
