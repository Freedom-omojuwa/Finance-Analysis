# DAX Measures

This document contains the DAX calculations used to build the **Finance Analytics Dashboard**. The measures support KPI calculations, time intelligence, and dynamic reporting.

---

# Calendar Table

A dedicated Calendar table was created to support time intelligence calculations such as Year-over-Year (YoY) analysis.

## Calendar Table

```DAX
Calendar Table =
CALENDAR(
    MIN(finance_transactions[transaction_date]),
    MAX(finance_transactions[transaction_date])
)
```

**Purpose**

Creates a continuous date table based on the minimum and maximum transaction dates.

---

## Month

```DAX
Month =
FORMAT('Calendar Table'[Date], "mmm")
```

**Purpose**

Returns the abbreviated month name (Jan, Feb, Mar...).

---

## Month No

```DAX
Month No =
MONTH('Calendar Table'[Date])
```

**Purpose**

Returns the month number (1–12) for chronological sorting.

---

## Year

```DAX
Year =
YEAR('Calendar Table'[Date])
```

**Purpose**

Extracts the year from the date.

---

# Dynamic Metric (Field Parameter)

A Field Parameter was created to allow users to switch between KPIs dynamically across visuals.

```DAX
Dynamic Metric =
{
    ("Total Amount", NAMEOF('finance_transactions'[Total Amount]), 0),
    ("Total Fees", NAMEOF('finance_transactions'[Total Fees]), 1),
    ("Total Tax", NAMEOF('finance_transactions'[Total Tax]), 2),
    ("Total Transaction", NAMEOF('finance_transactions'[Total Transaction]), 3)
}
```

**Purpose**

Allows users to dynamically switch between key financial metrics using a slicer.

---

# KPI Measures

## Total Amount

```DAX
Total Amount =
SUM(finance_transactions[amount])
```

**Purpose**

Calculates the total value of all financial transactions.

---

## Total Transaction

```DAX
Total Transaction =
DISTINCTCOUNT(finance_transactions[transaction_id])
```

**Purpose**

Returns the total number of unique transactions.

---

## Average Transaction Value

```DAX
Avg Transaction Value =
AVERAGE(finance_transactions[amount])
```

**Purpose**

Calculates the average transaction amount.

---

## Total Fees

```DAX
Total Fees =
SUM(finance_transactions[fee_amount])
```

**Purpose**

Calculates the total transaction fees collected.

---

## Total Tax

```DAX
Total Tax =
SUM(finance_transactions[tax_amount])
```

**Purpose**

Calculates the total tax generated from transactions.

---

# Previous Year (PY) Measures

## PY Amount

```DAX
PY Amount =
CALCULATE(
    [Total Amount],
    SAMEPERIODLASTYEAR('Calendar Table'[Date])
)
```

**Purpose**

Returns the total transaction amount for the previous year.

---

## PY Avg Transaction

```DAX
PY Avg Transaction =
CALCULATE(
    [Avg Transaction Value],
    SAMEPERIODLASTYEAR('Calendar Table'[Date])
)
```

**Purpose**

Returns the previous year's average transaction value.

---

## PY Fees

```DAX
PY Fees =
CALCULATE(
    [Total Fees],
    SAMEPERIODLASTYEAR('Calendar Table'[Date])
)
```

**Purpose**

Returns the total fees collected in the previous year.

---

## PY Tax

```DAX
PY Tax =
CALCULATE(
    [Total Tax],
    SAMEPERIODLASTYEAR('Calendar Table'[Date])
)
```

**Purpose**

Returns the total tax generated in the previous year.

---

## PY Transaction

```DAX
PY Transaction =
CALCULATE(
    [Total Transaction],
    SAMEPERIODLASTYEAR('Calendar Table'[Date])
)
```

**Purpose**

Returns the total number of transactions in the previous year.

---

# Year-over-Year (YoY) Measures

## YoY Amount %

```DAX
YoY Amount % =
DIVIDE(
    [Total Amount] - [PY Amount],
    [PY Amount],
    0
)
```

**Purpose**

Calculates the percentage growth in transaction amount compared to the previous year.

---

## YoY Avg Transaction %

```DAX
YoY Avg Transaction % =
DIVIDE(
    [Avg Transaction Value] - [PY Avg Transaction],
    [PY Avg Transaction],
    0
)
```

**Purpose**

Calculates the percentage growth in average transaction value compared to the previous year.

---

## YoY Fees %

```DAX
YoY Fees % =
DIVIDE(
    [Total Fees] - [PY Fees],
    [PY Fees],
    0
)
```

**Purpose**

Calculates the percentage growth in total fees compared to the previous year.

---

## YoY Tax %

```DAX
YoY Tax % =
DIVIDE(
    [Total Tax] - [PY Tax],
    [PY Tax],
    0
)
```

**Purpose**

Calculates the percentage growth in total tax compared to the previous year.

---

## YoY Transaction %

```DAX
YoY Transaction % =
DIVIDE(
    [Total Transaction] - [PY Transaction],
    [PY Transaction],
    0
)
```

**Purpose**

Calculates the percentage growth in total transactions compared to the previous year.

---

# Summary

The DAX measures developed for this project provide:

- Financial KPI calculations
- Time intelligence using a Calendar table
- Previous Year (PY) analysis
- Year-over-Year (YoY) growth calculations
- Dynamic KPI switching using Field Parameters

These measures power the interactive Power BI dashboard and enable stakeholders to monitor financial performance, evaluate transaction trends, and make data-driven decisions.
