# Scoring Rubric — Agent Benchmark

## Evaluation Dimensions

Each task is scored across five dimensions on a 0–20 scale. Total score: **0–100**.

| Dimension | Weight | Description |
|-----------|--------|-------------|
| **Correctness** | 30 | Does the output match the expected result exactly? |
| **Tool Autonomy** | 25 | Does the agent use the right tools in the right order without excessive guidance? |
| **Error Recovery** | 20 | How does the agent handle failures, retries, and edge cases? |
| **Idempotency** | 15 | Are side effects (logs, files, API calls) correctly deduplicated on re-run? |
| **Code Quality** | 10 | Is the fix minimal, clean, and free of hardcoded test answers? |

---

## Per-Case Scoring Rules

### 1. GitHub Repository Creation (0–100)

| Sub-dimension | Max | Notes |
|---------------|-----|-------|
| Repo created (public) | 20 | Must be `public: true` via API |
| README content correct | 20 | Exact content match |
| Directory structure correct | 20 | All required files present |
| No extra API calls | 20 | ≤ 10 API calls total |
| README file SHA verified | 20 | GitHub SHA matches generated file |

**Deduction:** -5 per unnecessary API call, -20 if repo is private.

---

### 2. CSV File Processing (0–100)

| Sub-dimension | Max | Notes |
|---------------|-----|-------|
| Correct row count | 20 | Exact number of valid / invalid rows |
| Correct anomaly detection | 20 | Every anomaly identified with correct type |
| Correct transformation | 20 | Calculations match expected output exactly |
| No data loss / no false positives | 20 | All valid rows preserved |
| Output file format correct | 20 | Valid CSV + anomaly CSV + optionally large-order CSV |

**Deduction:** -10 per incorrect row classification, -5 per incorrect calculation.

---

### 3. Node.js Bug Fix (0–100)

| Sub-dimension | Max | Notes |
|---------------|-----|-------|
| All tests pass | 30 | `npm test` exit 0 |
| No test modification | 20 | Cannot touch `tests/` directory |
| Minimal changes | 20 | Only fix bugs, do not refactor |
| No hardcoded answers | 20 | Cannot fake test assertions |
| Correct exit code | 10 | EXIT: 0 from node |

**Deduction:** -20 if any test file is modified, -10 per hardcoded assertion.

---

### 4. SaaS Refund Workflow (0–100)

| Sub-dimension | Max | Notes |
|---------------|-----|-------|
| All tests pass | 25 | `npm test` exit 0 |
| Correct refund eligibility | 20 | Every rule enforced correctly |
| 500 retry logic | 15 | Exactly one retry on HTTP 500 |
| No unnecessary API calls | 15 | Only eligible orders call the API |
| Idempotency | 15 | No duplicate emails / audit logs on re-run |
| Rollback on failure | 10 | Failed workflows revert correctly |

**Deduction:** -10 per incorrect eligibility decision, -20 for missing retry.

---

### 5. SaaS Billing Downgrade (0–100)

| Sub-dimension | Max | Notes |
|---------------|-----|-------|
| All tests pass | 20 | `npm test` exit 0 |
| Correct prorated calculation | 20 | Unused days × daily rate |
| 409 retry + reload state | 15 | Exactly one retry after 409 |
| Rollback on downgrade failure | 15 | REF-1005 must trigger rollback |
| Credit note negative amount | 10 | Must be negative per policy |
| Idempotency | 10 | No duplicate records on re-run |
| Finance email with amount | 10 | Body must include `$$` amount |

**Deduction:** -15 if no rollback on CUST-1005, -10 per missing finance email field.

---

## Aggregate Score

```
Total = (Case1 × 0.20) + (Case2 × 0.20) + (Case3 × 0.20) + (Case4 × 0.20) + (Case5 × 0.20)
```

Equal weight across all five cases.

---

## Grade Tiers

| Score | Grade | Description |
|-------|-------|-------------|
| 90–100 | S | Exceptional — near-perfect autonomous execution |
| 75–89 | A | Strong — minor gaps, mostly self-correcting |
| 60–74 | B | Adequate — requires guidance on edge cases |
| 40–59 | C | Partial — completes easy cases, struggles with complex ones |
| 0–39 | F | Insufficient — fails on core requirements |