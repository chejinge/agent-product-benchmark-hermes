# Task: GitHub Repository Creation

## Objective

Create a **public** GitHub repository named `agent-product-benchmark-hermes` under the authenticated user's account (`chejinge`). The repository must contain a specific directory structure and file content.

---

## Task Description

Create the following structure:

```
agent-product-benchmark-hermes/
├── README.md
├── rubric/
│   └── scoring.md
├── tasks/
│   ├── github_repo_creation.md
│   ├── csv_file_processing.md
│   ├── node_bug_fix.md
│   ├── saas_refund_workflow.md
│   └── saas_billing_downgrade.md
├── results/
│   └── summary.md
├── artifacts/
│   ├── sales_data_sample.csv
│   └── node_bug_project.zip
```

---

## Required File Content

### README.md

Must include:
- Project title and purpose
- A table of 5 evaluation cases (with #, Case name, file path, capability focus)
- A "How to Use" section
- A link to `rubric/scoring.md` and `results/summary.md`

### rubric/scoring.md

Must include:
- 5 evaluation dimensions (Correctness, Tool Autonomy, Error Recovery, Idempotency, Code Quality) with weight and description
- Per-case scoring rules for all 5 cases
- Aggregate formula
- Grade tiers table (S/A/B/C/F)

### tasks/*.md (one per case)

Each task file must include:
- **Objective**: One-sentence goal
- **Input**: What the agent receives
- **Expected Output**: What constitutes success
- **Evaluation Criteria**: 3–5 measurable checkpoints
- **Constraints**: What the agent CANNOT do

### results/summary.md

Must include a results table with columns: `Case | Best Performer | Score | Notes`. All cells initially filled with `TBD`.

### artifacts/

- `sales_data_sample.csv`: A realistic CSV with order data (orderId, customer, product, quantity, unitPrice, discountRate, status, orderDate). Include at least 3 anomaly rows (quantity=0, missing price, string quantity).
- `node_bug_project.zip`: A Node.js project with at least 3 bugs in `inventory.js` and a passing `node --test` test suite. Bugs must be in calculation logic (not test assertions).

---

## Constraints

1. Repository must be **public**.
2. All files must exist with non-empty content.
3. README must not be hardcoded with test expectations.
4. CSV must have at least 8 rows including 3 anomalies.
5. Artifact ZIP must be a valid `.zip` file that passes `unzip -t`.

---

## Success Criteria

- GitHub API returns `200` on `GET /repos/chejinge/agent-product-benchmark-hermes` with `"public": true`.
- All files exist in the repository (check via API or GitHub UI).
- `rubric/scoring.md` and `results/summary.md` are non-trivial and well-structured.
- Artifacts are valid and parseable.

---

## Evaluation Focus

**Tool Autonomy + Stability**: This task tests whether the agent can orchestrate GitHub REST API calls (`POST`, `PUT`, `PATCH`), handle authentication, create multi-level directories, and generate valid content — all without manual guidance.