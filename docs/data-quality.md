# Data quality and metric contract

## Scope

The supplied CSVs contain five dimensions and five facts. The date dimension is continuous from **2023-01-01 to 2024-12-31** (731 dates). Record counts below were measured from the checked-in CSVs.

| Table | Rows | Grain / key observations |
| --- | ---: | --- |
| `dim_date` | 731 | One row per calendar date; `date_key` |
| `dim_customer` | 5 | `customer_id` |
| `dim_product` | 16 | `product_id` |
| `dim_supplier` | 7 | `supplier_id` |
| `dim_facility` | 6 | `facility_id` |
| `fact_sales` | 8,500 | Sales line; `sales_id` |
| `fact_inventory` | 1,152 | Product/facility/date snapshot; no duplicate combinations found |
| `fact_procurement` | 2,200 | Purchase order line; `procurement_id` |
| `fact_production` | 4,500 | Production record; `production_id` |
| `fact_shipment` | 7,500 | Shipment record; `shipment_id` |

## Confirmed strengths

- All checked sales, inventory, procurement, and production foreign keys matched the supplied dimensions and date table.
- Gross revenue equals `quantity_sold x unit_price` for every sales row (to cent precision under the audit tolerance).
- Discount amount equals `gross_revenue x discount_pct` for every sales row (to cent precision under the audit tolerance).
- Total profit reconciles exactly: **$48,559,549.18 = $176,946,645.88 net revenue - $128,387,096.70 total cost**.
- No duplicate product/facility/date inventory combinations were found.
- Shipment delivery dates were not earlier than ship dates in the supplied date dimension.

## Issues that block trustworthy analysis

### Shipment dimension keys do not match

| Relationship candidate | Unmatched shipment rows | Coverage |
| --- | ---: | ---: |
| Shipment `product_id` to `dim_product[product_id]` | 4,142 / 7,500 | 44.77% |
| Shipment `customer_id` to `dim_customer[customer_id]` | 5,295 / 7,500 | 29.40% |
| Shipment `facility_id` to `dim_facility[facility_id]` | 7,500 / 7,500 | 0.00% |

The supplied facility IDs are 1-6 while shipment facility IDs are 10-16. Product and customer shipment IDs also exceed the supplied dimension ranges. Do not join these facts by row order, truncate values, or infer mappings. Obtain an authoritative crosswalk or correct the source extract; until then, keep shipment analysis at shipment-fact grain and do not attribute shipment results to product, customer, or facility dimensions.

### Stored production defect rate is inconsistent

For **4,277 of 4,500** production rows, `defect_rate_pct` does not equal `100 x defective_units / quantity_produced` (rounded to two decimals). The inputs contain no zero-production rows and no rows where defects exceed units produced. Use the recomputed ratio only if the business confirms that is the intended definition; otherwise investigate the upstream definition and correct the source.

### Delay-reason field is sparse

`delay_reason` is populated on **573 of 7,500** shipment rows (**7.64%**). The 573 populated rows equal the number of shipments whose status is `Delayed` in this extract. Convert empty CSV values to null in Power Query and state the denominator when showing reason coverage.

## Verified headline aggregates

| Metric | Audited result | Calculation |
| --- | ---: | --- |
| Gross revenue | $186,861,918.70 | `SUM(fact_sales[gross_revenue])` |
| Net revenue | $176,946,645.88 | `SUM(fact_sales[net_revenue])` |
| Discount amount | $9,915,272.82 | `SUM(fact_sales[discount_amount])` |
| Total cost | $128,387,096.70 | `SUM(fact_sales[total_cost])` |
| Profit | $48,559,549.18 | `SUM(fact_sales[profit])` |
| Profit margin | 27.44% | Profit / net revenue |
| Delivered shipment share | 75.29% | 5,647 delivered records / 7,500 shipment records |

Delivered shipment share is **not** an on-time delivery rate, an in-full rate, or a perfect-order rate. The facts do not provide a promised delivery date, order-to-shipment key, complete order quantity, or damage/documentation evidence needed to establish those measures.

## Recommended model and Power Query actions

1. Set key columns to whole number (or a consistent text key) on both sides of each relationship; set monetary fields to fixed decimal and calendar dates to date.
2. Mark `dim_date` as the date table. Keep one-to-many, single-direction dimension-to-fact relationships; use role-playing date relationships for ship, delivery, and procurement dates where needed.
3. Reconcile shipment keys against their source. Until then, do not activate shipment-to-product/customer/facility relationships.
4. Convert empty text to null in `delay_reason`; distinguish missing/unknown values from "no delay".
5. Treat inventory as periodic snapshots: calculate stock as of a selected/latest snapshot, not as a sum across snapshot dates.
6. Replace ambiguous source labels with business-readable measures and show their denominator/definition in tooltips or a metric glossary.
7. Validate report totals against the aggregates above and test slicers, cross-filtering, drill-through, and date boundaries after applying any model changes.

## Interpretation note

These figures describe the supplied portfolio dataset only. They are not verified financial or operational statements about any named company.
