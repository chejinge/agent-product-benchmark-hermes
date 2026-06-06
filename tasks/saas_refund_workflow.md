# Task: SaaS Refund Workflow

## Objective

Implement a complete, idempotent refund processing workflow in Node.js. All business rules must be enforced, `npm test` must pass, and the workflow must be safe to re-run.

---

## Task Description

Build a refund workflow for a SaaS platform. Given a batch of orders, process only those that qualify under the refund policy. Each order must go through:

1. **Query** — read order from `data/orders.json`
2. **Eligibility check** — enforce all 7 refund rules
3. **API call** — call `mockRefundApi.refund(orderId, amount)` for eligible orders
4. **Retry** — if API returns HTTP 500, retry **exactly once**
5. **Side effects** — on success: send email, write audit log, mark `refunded: true`
6. **Idempotency** — re-running must not duplicate emails, logs, or API calls

---

## Refund Policy Rules

1. `status` must be exactly `"paid"`.
2. `refunded` must be `false`.
3. `quantity` must be a **positive integer**.
4. `unitPrice` must be **greater than 0**.
5. `discountRate` must be `>= 0` and `< 1`.
6. Order must be within **30 calendar days** (inclusive) of today (`2026-06-06`).
7. Refund amount = `quantity * unitPrice * (1 - discountRate)`, rounded to 2 decimals.

---

## Mock API Behavior

```
refund("ORD-9001", amount):
  1st call → HTTP 500
  2nd call → HTTP 200, { success: true, refundId: "REF-9001" }

All other orders:
  If eligible → HTTP 200, { success: true, refundId }
  If not eligible → must NOT call API
```

---

## Idempotency Requirements

- Calling `processRefund("ORD-9001")` twice must not create two audit log entries.
- Calling `processRefund("ORD-9001")` twice must not send two emails.
- The workflow must be safe to re-run after partial failure.

---

## Success Criteria

- `npm test` exits with code 0.
- All refund-eligible orders are processed correctly.
- All refund-ineligible orders are rejected with correct `reason`.
- ORD-9001 triggers one retry (500 → 200), `retryCount` = 1 in result.
- No unnecessary API calls for ineligible orders.
- Re-running produces no duplicate side effects.

---

## Evaluation Focus

**Business Logic + Idempotency + Error Recovery**: Tests rule enforcement accuracy, HTTP error handling with retry, side-effect deduplication, and batch processing correctness.