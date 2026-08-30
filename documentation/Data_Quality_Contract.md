# Data Quality Contract

## 1. Purpose

This Data Quality Contract defines the minimum data-quality standards
required for the retail orders dataset to be considered reliable for
KPI reporting and business decision-making.

The contract covers five data-quality dimensions:

- Completeness
- Uniqueness
- Validity
- Consistency
- Freshness

---

## 2. Dataset Information

| Attribute | Definition |
|---|---|
| Dataset | Retail Orders |
| Data Grain | One row represents one order |
| Primary Key | order_id |
| Data Owner | Retail Operations Team |
| Decision Owner | Retail Operations Manager |
| Refresh Frequency | Daily |

---

## 3. Data Quality Rules

| Dimension | Field | Rule | Severity | Action |
|---|---|---|---|---|
| Completeness | order_id | Must not be missing | Critical | Reject record |
| Completeness | order_date | Must not be missing | Critical | Reject record |
| Completeness | city | Must not be missing | High | Reject record |
| Completeness | category | Must not be missing | High | Reject record |
| Completeness | quantity | Must not be missing | Critical | Reject record |
| Completeness | unit_price | Must not be missing | Critical | Reject record |
| Completeness | payment_status | Must not be missing | High | Reject record |
| Uniqueness | order_id | Must be unique | Critical | Remove duplicate and investigate |
| Validity | order_date | Must be a valid date from 2025-01-01 through today | Critical | Reject record |
| Validity | customer_segment | Must be Student, Fresher, or Professional | High | Reject or standardize |
| Validity | category | Must be Learning Kit, Course Access, or Mentor Session | High | Reject record |
| Validity | quantity | Must be a whole number greater than 0 | Critical | Reject record |
| Validity | unit_price | Must be greater than or equal to 0 | Critical | Reject record |
| Validity | discount_pct | Must be between 0 and 100 | High | Reject record |
| Validity | payment_status | Must be Paid, Pending, Failed, or Refunded | High | Reject record |
| Consistency | customer_segment | Values must use standardized capitalization | Medium | Normalize value |
| Consistency | payment_status | Values must use standardized capitalization | Medium | Normalize value |
| Consistency | order_date | Dates must use YYYY-MM-DD format | Medium | Normalize value |
| Freshness | order_date | Latest valid order date must be monitored | Medium | Report freshness status |

---

## 4. Allowed Values

### Customer Segment

- Student
- Fresher
- Professional

### Category

- Learning Kit
- Course Access
- Mentor Session

### Payment Status

- Paid
- Pending
- Failed
- Refunded

---

## 5. Cleaning Rules and Assumptions

### Missing Discount

If `discount_pct` is missing, it is treated as 0%.

This assumption means that a missing discount is interpreted as no
discount being applied.

This assumption should be reviewed if the business confirms that
missing discounts may represent unknown or unrecorded discounts.

### Textual Quantity

Unambiguous textual quantities such as `two` may be converted to
their numeric equivalent.

### Standardization

Differences in capitalization and date formatting are standardized
where the intended value can be determined reliably.

### Unrecoverable Values

Values that cannot be reliably corrected from the available data are
rejected rather than guessed or artificially replaced.

---

## 6. Severity and Escalation

### Critical

A critical failure prevents the affected record from being used for
KPI calculations.

Examples:

- Missing order_id
- Duplicate order_id
- Missing or invalid order_date
- Invalid quantity
- Missing quantity
- Invalid unit_price

**Action:**

1. Reject or quarantine the affected record.
2. Record the reason for rejection.
3. Investigate recurring issues with the data owner.

### High

A high-severity failure may affect KPI accuracy.

Examples:

- Missing city
- Invalid discount
- Invalid category
- Invalid customer segment
- Invalid payment status

**Action:**

1. Correct the value when it can be reliably determined.
2. Otherwise reject the affected record.
3. Record the issue and investigate the source.

### Medium

A medium-severity issue does not necessarily invalidate the record
but requires standardization or monitoring.

Examples:

- Capitalization differences
- Date-format differences
- Freshness monitoring

**Action:**

Normalize the value or monitor the issue.

---

## 7. KPI Dependencies

| KPI | Critical Data Dependencies |
|---|---|
| Total Orders | order_id |
| Total Revenue | quantity, unit_price |
| Net Revenue | quantity, unit_price, discount_pct |
| Average Order Value | order_id, quantity, unit_price, discount_pct |
| Total Units Sold | quantity |
| Average Discount % | discount_pct |
| Paid Order Rate | order_id, payment_status |
| Pending Order Rate | order_id, payment_status |

A KPI must not be considered trustworthy when a critical
data-quality rule affecting its required fields fails.

---

## 8. Freshness Monitoring

The latest valid `order_date` will be monitored as an indicator of
dataset freshness.

The current dataset does not specify a formal business freshness SLA.
Therefore, freshness is initially treated as a monitoring metric
rather than an automatic failure condition.

A formal freshness threshold should be established with the business
owner before production implementation.

---

## 9. Data Quality Status

The dataset will be classified using the following statuses:

| Status | Meaning |
|---|---|
| PASS | No critical data-quality issues detected |
| WARNING | Non-critical issues require monitoring |
| FAIL | One or more critical data-quality rules failed |

A dataset must achieve **PASS** before being used for final KPI
reporting.