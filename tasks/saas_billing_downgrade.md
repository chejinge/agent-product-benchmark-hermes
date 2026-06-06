# Task: SaaS Billing Downgrade

## Objective

Implement a complete subscription downgrade workflow with prorated refund, 409 retry, rollback on failure, and full audit trail.

---

## Task Description

Build a downgrade workflow from `pro_annual` to `basic_monthly`. For qualifying customers:

1. Calculate prorated refund (unused days / total days × price)
2. Issue refund via `mockBillingApi.refund()`
3. Downgrade subscription via `mockSubscriptionApi.downgrade()`
4. On downgrade **failure with 409**: reload subscription state, retry once
5. On downgrade **failure with 500+**: call `mockBillingApi.rollbackRefund()`
6. On success: update subscription, create credit note, send 2 emails, write audit log

---

## Downgrade Policy Rules

1. Customer `status` must be `"active"`.
2. Current subscription must be `"pro_annual"`.
3. Subscription `status` must be `"active"`.
4. Target plan must be `"basic_monthly"`.
5. Customer must have **no unpaid invoices**.
6. `lastDowngradeAt` must be `null` (not previously downgraded).
7. Refund = `annualPrice × unusedDays / totalSubscriptionDays`, rounded to 2 decimals.
8. Refund must be issued **before** subscription downgrade.
9. If refund succeeds but downgrade fails → **must rollback refund**.
10. If subscription API returns **409**: reload state, retry exactly once.
11. On success: update plan to `basic_monthly`, price to `$29`, set `lastDowngradeAt` to today.
12. On success: create credit note with **negative** amount, send customer + finance emails.

---

## Mock API Behavior

| Customer | refund | downgrade (1st) | downgrade (2nd) |
|----------|--------|-----------------|-----------------|
| CUST-1001 | success | success | — |
| CUST-1002 | skip (unpaid) | — | — |
| CUST-1003 | skip (not pro_annual) | — | — |
| CUST-1004 | success | **409** | success |
| CUST-1005 | success | **500** | → rollback |

---

## Credit Note Format

```json
{
  "id": "CN-{customerId}",
  "customerId": "CUST-XXXX",
  "amount": -XXX.XX,    // MUST be negative
  "refundId": "REF-XXXX",
  "createdAt": "..."
}
```

---

## Idempotency Requirements

- Re-running `processDowngrade("CUST-1001")` must not create duplicate credit notes.
- Re-running must not send duplicate emails.
- Audit log must not have duplicate success entries for same customer.

---

## Success Criteria

- `npm test` exits with code 0.
- CUST-1001: prorated refund = $687.12 (209 unused days of 365).
- CUST-1004: 409 → retry → success, refund = $1081.64.
- CUST-1005: refund succeeds → downgrade 500 → rollback → `reason: DOWNGRADE_ROLLED_BACK`.
- Only CUST-1001 and CUST-1004 get credit notes, emails, and audit entries.
- Finance email body must include dollar amount.
- Re-running produces no duplicate records.

---

## Evaluation Focus

**Complex Workflow + Rollback + Retry + Audit Trail**: Tests orchestrating multiple APIs, state reloading after conflict, financial accuracy, rollback correctness, and idempotent side effects.