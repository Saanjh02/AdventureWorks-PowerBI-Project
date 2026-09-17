# AdventureWorks Power BI – DAX Measures

This document contains the key DAX measures developed for the AdventureWorks Power BI dashboard. These measures support sales analysis, profitability analysis, customer insights, returns analysis, time intelligence, target tracking, and What-If analysis.

## Core KPI Measures

### Total Revenue

```DAX
Total Revenue =
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] *
    RELATED('Product Lookup'[ProductPrice])
)
```

### Total Cost

```DAX
Total Cost =
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] *
    RELATED('Product Lookup'[ProductCost])
)
```

### Total Profit

```DAX
Total Profit =
[Total Revenue] - [Total Cost]
```

### Total Orders

```DAX
Total Orders =
DISTINCTCOUNT(
    'Sales Data'[OrderNumber]
)
```

### Total Customers

```DAX
Total Customers =
DISTINCTCOUNT(
    'Sales Data'[CustomerKey]
)
```

### Quantity Sold

```DAX
Quantity Sold =
SUM(
    'Sales Data'[OrderQuantity]
)
```

### Total Returns

```DAX
Total Returns =
COUNT(
    'Returns Data'[ReturnQuantity]
)
```

### Quantity Returned

```DAX
Quantity Returned =
SUM(
    'Returns Data'[ReturnQuantity]
)
```

### Return Rate

```DAX
Return Rate =
DIVIDE(
    [Quantity Returned],
    [Quantity Sold],
    "NO Sales"
)
```

### Average Retail Price

```DAX
Average Retail Price =
AVERAGE(
    'Product Lookup'[ProductPrice]
)
```

### Average Revenue Per Customer

```DAX
Average Revenue Per Customer =
DIVIDE(
    [Total Revenue],
    [Total Customers]
)
```

---

## Time Intelligence Measures

### Previous Month Revenue

```DAX
Previous Month Revenue =
CALCULATE(
    [Total Revenue],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```

### Previous Month Orders

```DAX
Previous Month Orders =
CALCULATE(
    [Total Orders],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```

### Previous Month Profit

```DAX
Previous Month Profit =
CALCULATE(
    [Total Profit],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```

### Previous Month Returns

```DAX
Previous Month Returns =
CALCULATE(
    [Total Returns],
    DATEADD(
        'Calendar Lookup'[Date],
        -1,
        MONTH
    )
)
```

### YTD Revenue

```DAX
YTD Revenue =
CALCULATE(
    [Total Revenue],
    DATESYTD(
        'Calendar Lookup'[Date]
    )
)
```

---

## Rolling Performance Measures

### 10-Day Rolling Revenue

```DAX
10-day Rolling Revenue =
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(
        'Calendar Lookup'[Date],
        MAX('Calendar Lookup'[Date]),
        -10,
        DAY
    )
)
```

### 90-Day Rolling Profit

```DAX
90-day Rolling Profit =
CALCULATE(
    [Total Profit],
    DATESINPERIOD(
        'Calendar Lookup'[Date],
        MAX('Calendar Lookup'[Date]),
        -90,
        DAY
    )
)
```

---

## Target & Performance Measures

### Revenue Target

```DAX
Revenue Target =
[Previous Month Revenue] * 1.1
```

### Revenue Target Gap

```DAX
Revenue Target Gap =
[Total Revenue] - [Revenue Target]
```

### Order Target

```DAX
Order Target =
[Previous Month Orders] * 1.1
```

### Order Target Gap

```DAX
Order Target Gap =
[Total Orders] - [Order Target]
```

### Profit Target

```DAX
Profit Target =
[Previous Month Profit] * 1.1
```

### Profit Target Gap

```DAX
Profit Target Gap =
[Total Profit] - [Profit Target]
```

---

## Product & Order Analysis

### Overall Average Price

```DAX
Overall Average Price =
CALCULATE(
    [Average Retail Price],
    ALL('Product Lookup')
)
```

### High Ticket Orders

```DAX
High Ticket Orders =
CALCULATE(
    [Total Orders],
    FILTER(
        'Product Lookup',
        'Product Lookup'[ProductPrice] > [Overall Average Price]
    )
)
```

### Bulk Orders

```DAX
Bulk Orders =
CALCULATE(
    [Total Orders],
    'Sales Data'[OrderQuantity] > 1
)
```

### Weekend Orders

```DAX
Weekend Orders =
CALCULATE(
    [Total Orders],
    'Calendar Lookup'[Weekend] = "Weekend"
)
```

---

## Bike Performance Measures

### Bike Sales

```DAX
Bike Sales =
CALCULATE(
    [Quantity Sold],
    'Product Categories Lookup'[CategoryName] = "Bikes"
)
```

### Bike Returns

```DAX
Bike Returns =
CALCULATE(
    [Total Returns],
    'Product Categories Lookup'[CategoryName] = "Bikes"
)
```

### Bike Return Rate

```DAX
Bike Return Rate =
CALCULATE(
    [Return Rate],
    'Product Categories Lookup'[CategoryName] = "Bikes"
)
```

---

## Percentage of Overall Activity

### All Orders

```DAX
All Orders =
CALCULATE(
    [Total Orders],
    ALL('Sales Data')
)
```

### % Of All Orders

```DAX
% Of All Orders =
DIVIDE(
    [Total Orders],
    [All Orders]
)
```

### All Returns

```DAX
All Returns =
CALCULATE(
    [Total Returns],
    ALL('Returns Data')
)
```

### % Of All Returns

```DAX
% Of All Returns =
DIVIDE(
    [Total Returns],
    [All Returns]
)
```

---

## What-If Price Adjustment Measures

### Adjusted Price

```DAX
Adjusted Price =
[Average Retail Price] *
(1 + 'Price Adjustment (%)'[Price Adjustment (%) Value])
```

### Adjusted Revenue

```DAX
Adjusted Revenue =
SUMX(
    'Sales Data',
    'Sales Data'[OrderQuantity] *
    [Adjusted Price]
)
```

### Adjusted Profit

```DAX
Adjusted Profit =
[Adjusted Revenue] - [Total Cost]
```

---

## DAX Techniques Demonstrated

This Power BI project demonstrates several DAX concepts, including:

- Iterator functions using `SUMX`
- Filter context modification using `CALCULATE`
- Relationship-based calculations using `RELATED`
- Safe division using `DIVIDE`
- Time intelligence using `DATEADD`, `DATESYTD`, and `DATESINPERIOD`
- Context removal using `ALL`
- Table filtering using `FILTER`
- Rolling-period calculations
- Previous-month comparisons
- KPI target and variance calculations
- Dynamic What-If parameter analysis
- Product and category-level analysis
- Customer-level revenue analysis
