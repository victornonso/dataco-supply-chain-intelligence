# DataCo Supply Chain Intelligence

Victor Okeke · Power BI · DAX · Power Query · Data modelling

A commercial investigation of where revenue loses value through discounts,
product mix, customer concentration and delivery performance.

## Explore

Open `report/dataco-supply-chain-intelligence.pbix` in Power BI Desktop.
Seven report views cover executive performance, sales, customers, products,
logistics, profitability and management priorities. Market, year and customer
segment filters support investigation across the analytical pages.

Screenshots are exported from the report, not interface mockups. The portfolio
case study is https://victor-okeke-data-portfolio.victornonso44.chatgpt.site/projects/dataco-supply-chain-intelligence/

## Data and model

180,519 order lines, 65,752 distinct orders, 20,652 customers, 118 active
products and five markets. Coverage January 2015 through January 2018.
FactOrderItem connects to seven dimensions through active single-direction
many-to-one relationships. `model/` contains extracted schema, relationships
and audited measure definitions. `docs/` contains the semantic diagram and
KPI dictionary. `evidence/metrics.json` records engine-verified aggregates.

## Findings and definitions

Net sales $33.05M; recorded profit $3.97M. The highest-revenue fifth of customers
contributes 49.94% of net sales. Late-delivery rate is 57.29% of non-cancelled
order lines. Sales before discounts fell 4.03% in 2017 versus 2016.

Read the KPI dictionary before comparing measures. Orders are distinct OrderId
values rather than line counts. Margin divides recorded profit by net sales.
Late rate excludes cancellations and is line-weighted. Average delivery delay
is actual minus scheduled duration among late lines. January 2018 is partial.
Discount exposure is not a causal estimate of avoidable lost profit.

## Reproduce or extend

Use the supplied PBIX as the reproducible report artefact. Inspect the model
and DAX in Power BI Desktop; refresh or replace the source only after validating
column definitions, grain, keys and date coverage. Compare refreshed totals to
the recorded evidence. These files document the report; they do not claim to
be a complete independently automated rebuild of Power BI's semantic engine.

## Dataset attribution

DataCo SMART Supply Chain for Big Data Analysis, version 5.
Fabian Constante, Fernando Silva and António Pereira · CC BY 4.0.
https://data.mendeley.com/datasets/8gx2fvg2k6/5
Data embedded in the PBIX is from this public dataset. Preserve attribution.
Report, DAX definitions and analytical presentation are Victor Okeke's work.

## Scope

Historical descriptive analysis. No causal discount effect, future customer
value, independent cost-ledger reconstruction or currency conversion is claimed.
No proprietary ipNX material, academic administrative files, packed Python
virtual environments or raw audit-cache dumps are included.
