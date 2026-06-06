# Agent Benchmark — Hermes Edition

> Multi-dimensional evaluation framework for AI coding agents

## Overview

This benchmark evaluates AI agents across five realistic software engineering tasks. Each task is designed to stress-test specific capabilities: autonomous tool usage, code correctness, edge-case handling, idempotent workflow execution, and recovery from failure.

The framework measures not just *whether* the agent completes the task, but **how** it does it — tracking tool-call sequences, error recovery paths, and side-effect correctness.

---

## Evaluation Cases

| # | Case | File | Capability Focus |
|---|------|------|------------------|
| 1 | GitHub Repository Creation | `tasks/github_repo_creation.md` | Tool orchestration, API usage, file generation |
| 2 | CSV File Processing | `tasks/csv_file_processing.md` | Data parsing, anomaly detection, output correctness |
| 3 | Node.js Bug Fix | `tasks/node_bug_fix.md` | Multi-file debugging, test-driven verification |
| 4 | SaaS Refund Workflow | `tasks/saas_refund_workflow.md` | Business-rule validation, idempotency, side effects |
| 5 | SaaS Billing Downgrade | `tasks/saas_billing_downgrade.md` | Complex workflow, rollback, 409 retry, audit trail |

---

## How to Use

1. **Provide** the task file (`tasks/<case>.md`) to the agent under test.
2. **Run** the agent's solution through `npm test` or the provided verification commands.
3. **Record** the result in `results/summary.md`.

---

## Artifact Bundle

All task inputs (CSV datasets, Node.js projects, SaaS mock servers) are in `/artifacts/`:

```
artifacts/
├── sales_data_sample.csv          # CSV processing input
├── sales_data_expected_output.json # Expected transform results
├── node_bug_project.zip           # Multi-file bug scenario
└── saas_refund_mock_server.zip    # SaaS workflow test harness
```

---

## Scoring

See `rubric/scoring.md` for the full evaluation rubric and formula.

---

## Results

See `results/summary.md` for per-case best performer tracking.