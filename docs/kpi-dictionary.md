# DataCo Supply Chain Intelligence — KPI dictionary

Coverage: 1 January 2015–31 January 2018. Currency follows the dataset/report dollar convention.

Grain: one row per order item; 180,519 lines and 65,752 distinct orders.

## Net Sales

Sales after recorded line discounts. Format: $#,0.00.

```dax
SUM(FactOrderItem[OrderItemTotal])
```

## Total Orders

Distinct orders in the current filter context. Format: #,0.

```dax
DISTINCTCOUNT(FactOrderItem[OrderId])
```

## Avg Order Value

Net sales per distinct order. Format: $#,0.00.

```dax
DIVIDE([Net Sales], [Total Orders])
```

## Profit Margin %

Recorded line profit as a percentage of net sales; not an audited accounting gross margin. Format: 0.00.

```dax
DIVIDE([Total Profit], [Net Sales]) * 100
```

## Avg Delivery Delay (Days)

Average extra days among late delivered lines. Format: 0.00.

```dax
AVERAGEX(FILTER(FactOrderItem, FactOrderItem[DeliveryStatus] = "Late delivery"), FactOrderItem[DaysForShippingReal] - FactOrderItem[DaysForShipScheduled])
```

## Late Delivery Rate

Late delivered lines / non-cancelled delivered lines. Format: 0.00.

```dax
DIVIDE(CALCULATE(COUNTROWS(FactOrderItem), FactOrderItem[DeliveryStatus] = "Late delivery"), CALCULATE(COUNTROWS(FactOrderItem), FactOrderItem[DeliveryStatus] <> "Shipping canceled")) * 100
```

## Top 20% Customer Sales %

Top ceiling(20%) of customers in selected fact context ranked by net revenue; deterministic key tie-break. Format: 0.00.

```dax
VAR Customers = ADDCOLUMNS(VALUES(FactOrderItem[CustomerKey]), "Revenue", CALCULATE([Net Sales])) VAR N = ROUNDUP(COUNTROWS(Customers)*0.2,0) VAR TopCustomers = TOPN(N, Customers, [Revenue], DESC, FactOrderItem[CustomerKey], ASC) RETURN DIVIDE(SUMX(TopCustomers,[Revenue]),[Net Sales])*100
```

## Latest Full-Year Sales Growth %

Latest complete calendar year versus previous year, preserving non-date selections. Format: 0.00.

```dax
VAR LastFact = CALCULATE(MAX(FactOrderItem[OrderDate]), REMOVEFILTERS(DimDate)) VAR Y = YEAR(LastFact)-IF(LastFact<DATE(YEAR(LastFact),12,31),1,0) VAR CurrentSales=CALCULATE([Total Sales],REMOVEFILTERS(DimDate),DimDate[Year]=Y) VAR PriorSales=CALCULATE([Total Sales],REMOVEFILTERS(DimDate),DimDate[Year]=Y-1) RETURN DIVIDE(CurrentSales-PriorSales,PriorSales)*100
```

## Sales 3M Moving Avg

Average of three calendar-month gross-sales totals, retaining market/customer/product filters. Early incomplete windows are blank. Format: $#,0.00.

```dax
VAR Anchor = MAX(FactOrderItem[OrderDate]) VAR FirstMonth = EOMONTH(Anchor,-3)+1 VAR FirstData = CALCULATE(MIN(FactOrderItem[OrderDate]),REMOVEFILTERS(DimDate)) RETURN IF(FirstMonth>=FirstData, AVERAGEX(GENERATESERIES(0,2), VAR Offset=[Value] VAR EndDate=EOMONTH(Anchor,-Offset) VAR StartDate=EOMONTH(Anchor,-Offset-1)+1 RETURN CALCULATE([Total Sales], REMOVEFILTERS(DimDate), DATESBETWEEN(DimDate[FullDate],StartDate,EndDate))))
```

Delivery percentages are line-weighted and exclude cancelled lines. On-time includes early delivery. Recorded profit is the source field, not a reconstructed gross-profit measure. Net sales use recorded OrderItemTotal; rounding means pre-discount sales less recorded discounts differs by $46.13 in aggregate. The latest full-year comparison is 2017 versus 2016; January 2018 is a partial year. Historical revenue per customer is not predicted lifetime value.
