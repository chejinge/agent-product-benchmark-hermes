# Task: CSV File Processing

## Objective

Given a CSV file of sales transactions, detect anomalies, calculate totals, and produce clean output files.

---

## Task Description

Download `artifacts/sales_data_sample.csv`. Process it to:

1. Identify **anomalous rows** (invalid data).
2. Produce a **valid rows CSV** with a `total` column appended (`quantity × unitPrice × (1 − discountRate)`).
3. Produce an **anomaly CSV** listing all invalid rows with an `anomaly_type` column.
4. Optionally, produce a **large orders CSV** for rows where `quantity >= 5`.

---

## Input Schema

```
orderId, customer, product, quantity, unitPrice, discountRate, status, orderDate
```

## Anomaly Types

| Type | Condition |
|------|-----------|
| `QUANTITY_ZERO` | quantity = 0 |
| `QUANTITY_NEGATIVE` | quantity < 0 |
| `QUANTITY_NON_INTEGER` | quantity is not a whole number |
| `PRICE_MISSING` | unitPrice is null, NaN, or empty |
| `PRICE_ZERO` | unitPrice = 0 |
| `DISCOUNT_INVALID` | discountRate < 0 or discountRate >= 1 |
| `STATUS_INVALID` | status is not one of `paid/pending/refunded/cancelled` |

---

## Expected Output Files

| File | Description |
|------|-------------|
| `output/valid_orders.csv` | Valid rows + `total` column |
| `output/anomaly_rows.csv` | Anomalous rows + `anomaly_type` |
| `output/large_orders.csv` | (optional) Rows with quantity >= 5 |
| `output/summary.json` | `{ total_valid_rows, total_anomalies, total_revenue }` |

---

## Constraints

1. Do not delete any row — classify and separate them.
2. The `total` column must be rounded to 2 decimal places.
3. The process must be deterministic and idempotent.
4. Do not hardcode expected anomaly rows — read and analyze the data.

---

## Success Criteria

- `valid_orders.csv` has correct row count and correct `total` calculations.
- `anomaly_rows.csv` has all anomalies with correct `anomaly_type`.
- `summary.json` `total_revenue` matches sum of all valid row totals.
- No valid row is misplaced in the anomaly file.

---

## Evaluation Focus

**Data Correctness + Idempotency**: Tests parsing accuracy, floating-point math, row classification, and output file generation.