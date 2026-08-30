# KPI-Data-Quality

# Retail KPI & Data Quality Management System

## Overview

This project aims to take a retail orders dataset and turn it into a clean, reliable and ready-to-use dataset for business reporting. Of building KPIs directly from raw data the process starts by examining the quality of the data. It identifies issues applies cleaning rules and separates records that cannot be trusted for further review.

Once the data is cleaned and validated business KPIs are defined through a KPI Dictionary. Documented with a Data Quality Contract. The overall workflow follows this path:

**Raw Data → Data Profiling → Data Cleaning → Data Quality Validation → KPI Definition → Business Reporting**

---

## Problem Statement

Retail data often has problems like entries missing values, inconsistent formats, invalid numbers and incorrect categories. If these problems are ignored, the resulting KPIs and reports can give insights.

For this project the goal is to create a structured data quality process. This ensures only trustworthy records are used when calculating KPIs and generating business reports.

---

## Objectives

- Understand the structure and business meaning of the retail dataset

- Find data quality issues before any analysis begins

- Clean and standardize data that can be fixed

- records that cannot be reliably corrected

- Define clear data quality rules that can be measured

- Create a business-focused KPI Dictionary

- Build a Data Quality Contract

- Validate the cleaned dataset against the rules

- Prepare the data for accurate KPI reporting

---

## Dataset

The dataset contains retail order information with the following fields:

| Column | Description |

|---|---|

order_id` | Unique identifier for each order |

| `order_date` | Date on which the order was placed |

| `customer_segment` | Customer category such as Student, Fresher or Professional |

| `city` | Customer/order city |

| `category` | Product or service category |

| `quantity` | Number of units ordered |

| `unit_price` | Price per unit in INR |

| `discount_pct` | Discount percentage applied |

| `payment_status` | Payment state of the order |

---

## Data Quality Dimensions

The dataset was evaluated using five key data quality dimensions.

### 1. Completeness

Checks whether required fields have missing values.

Examples:

- Missing order ID

- Missing order date

- Missing city

- Missing quantity

### 2. Uniqueness

Checks whether `order_id` is unique. Finds duplicate records.

### 3. Validity

Checks whether values follow the defined business rules.

Examples:

- Quantity must be a number greater than zero

- Unit price cannot be negative

- Discount must be between 0 and 100

- Payment status must belong to the approved list

### 4. Consistency

Checks whether values follow a format.

Examples:

- `student` → `Student`

- `paid` → `Paid`

- date formats → standardized date format

### 5. Freshness

Checks whether the available order dates are recent enough to represent the expected reporting period.

No formal freshness SLA was specified so freshness is treated as a monitoring rather than an automatic failure condition.

---

## Data Cleaning

The raw dataset had types of quality issues. Some could be corrected confidently while others could not.

### Examples of corrections

- Standardized inconsistent date formats

- Standardized capitalization (e.g. `student` to `Student`)

- Converted unambiguous text quantities like `two` to numeric values

- Treated missing discount as 0% based on project assumptions

### Examples of rejected records

Records that could not be reliably corrected were moved to a rejection dataset.

Examples include:

- Invalid dates

- Invalid quantities

- Missing required fields

- Discount values outside the range

The project avoids guessing values unless there is reasonable confidence in the correction.

---

## KPI Framework

After setting up the data quality rules eight business KPIs were defined.

| KPI | Purpose |

|---|---|

| Total Orders | Measures the number of orders |

| Total Revenue Measures gross revenue before discounts |

| Net Revenue | Measures revenue after discounts |

| Average Order Value | Measures average net revenue per order |

| Total Units Sold | Measures the total quantity sold |

| Average Discount % | Measures the average discount applied |

Paid Order Rate | Measures the percentage of orders successfully paid

| Pending Order Rate | Measures the percentage of orders awaiting payment |

The KPI Dictionary documents the business meaning, calculation logic, data grain, filters, ownership, refresh frequency and data quality dependencies for each KPI.

---

## KPI Dashboard

The Excel workbook includes a KPI dashboard based on the cleaned dataset. The dashboard shows metrics such as:

- Total Orders

- Total Revenue

- Net Revenue

- Average Order Value

- Total Units Sold

- Average Discount

- Paid Order Rate

- Pending Order Rate

All KPI values are calculated from the cleaned data. They are not manually entered.

---

## Data Quality Contract

The `Data_Quality_Contract.md` file defines the rules that the dataset must meet before being considered suitable for KPI reporting.

The contract includes:

- Data quality dimensions

- Field-level validation rules

- Severity levels

- Allowed values

- Cleaning assumptions

- Escalation actions

- KPI dependencies

- Freshness monitoring

- Final data quality status definitions

This creates a link, between **data quality and KPI reliability**.

---

## Project Workflow

```text

Raw Retail Data

│

▼

Data Profiling

│

▼

Identify Issues

│

▼

Data Cleaning

/           \

/             \

▼               ▼

Cleaned Records    Rejected Records

│

▼

Data Quality Checks

│

▼

KPI Definition

│

▼

KPI Dictionary + Dashboard

│

▼

Business Reporting

```
