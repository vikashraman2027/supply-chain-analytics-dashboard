# Supply Chain Analytics | Power BI Portfolio Project

An executive-oriented Power BI portfolio project for exploring sales, procurement, inventory, production, and shipment operations. The original local workspace contains a PBIX report; this public-ready package intentionally excludes that file and focuses on the approved CSV data, a refreshed Power BI theme, validated DAX measure patterns, and a model/data-quality review.

> **Important:** This is an illustrative portfolio dataset, not verified company reporting. The presence of Samsung product and customer names does not imply Samsung affiliation or endorsement. Figures must not be represented as actual company performance.

## Project at a glance

| Area | Contents |
| --- | --- |
| Sales | Revenue, profit, margin, customer/channel and product analysis |
| Procurement | Supplier quality, purchase cost, and lead-time analysis |
| Inventory | Stock, safety-stock and reorder-point snapshots |
| Production | Output and defective-unit analysis |
| Logistics | Shipment status, carrier, shipping cost, and delay reasons |

The data covers **2023-01-01 through 2024-12-31**. See [the data-quality review](docs/data-quality.md) before interpreting results: shipment dimension keys are not aligned with the supplied dimensions, and the stored production defect-rate field is inconsistent with its underlying counts.

## Verified headline figures

These are dataset aggregates, not claims about a real company:

| Metric | Value | Definition |
| --- | ---: | --- |
| Net revenue | $176.95M | Sum of `fact_sales[net_revenue]` |
| Gross revenue | $186.86M | Sum of `fact_sales[gross_revenue]` |
| Profit | $48.56M | Sum of `fact_sales[profit]`; reconciles to net revenue less total cost |
| Profit margin | 27.44% | Profit divided by net revenue |
| Shipment delivered share | 75.29% | Delivered shipment records divided by all shipment records; not an on-time or perfect-order rate |

## Report preview

[View the proposed executive dashboard concept](preview%20image/Dashboard-Concept.png) or its [scalable SVG source](preview%20image/Dashboard-Concept.svg). The PNG is a browser-rendered image of an original static design concept, **not a screenshot of a rendered or updated Power BI report**. The existing PBIX is preserved as-is; the refreshed theme and measure patterns are ready to apply when editing in Power BI Desktop.

Existing report-page previews remain in the local working folder only; they show the prior report and are not part of the public-ready package.

## Repository contents

```text
Data/                         CSV dimensions and facts
docs/
  data-quality.md             Audited row counts, key mismatches, and metric caveats
  implementation-plan.md      Scope, phases, deliverables, and acceptance criteria
  report-design.md            Executive-first page and interaction specification
dax/
  Measures.dax                Reusable, explicitly defined measure patterns
preview image/
  Dashboard-Concept.svg       Static, non-branded report design concept
Power BI Executive Theme.json Accessible, neutral Power BI theme
supply chain.pbix              Existing local report; not in the public-ready package
```

## Recommended report structure

1. **Executive overview** - net revenue, profit, margin, delivered shipment share, and prioritized exceptions.
2. **Commercial performance** - sales trend, channel/customer mix, product/category profitability, and discount impact.
3. **Inventory & production** - latest-snapshot stock, safety stock, reorder exceptions, production volume, and a recomputed defect rate.
4. **Supplier performance** - procurement lead time, quality score, cost, and supplier comparisons.
5. **Shipment operations** - shipment status, shipping cost, carrier and delay-reason trends. Keep customer/product/facility shipment drill-downs disabled until keys are reconciled.
6. **Data quality** - unmatched keys, incomplete fields, and metric caveats visible to report consumers.

## Applying the theme and measures

1. Open the original PBIX in Power BI Desktop (it is available only in the local working folder) and save a working copy before making changes.
2. Import `Power BI Executive Theme.json` from **View > Themes > Browse for themes**.
3. In Power Query, confirm data types, convert empty text in `delay_reason` to null, and profile keys before creating relationships.
4. Use the relationship and caveat guidance in [the data-quality review](docs/data-quality.md). Do not create shipment dimension relationships from coincidental numeric IDs.
5. Add measures from [`dax/Measures.dax`](dax/Measures.dax), set their numeric formats in the model, and build pages following [the design specification](docs/report-design.md).
6. Validate totals against the CSVs and capture screenshots from the rendered report after the key and defect-rate issues are resolved.

## Data and usage

The CSV files are included for reproducibility. No data license or rights to third-party marks/product images are asserted here. Do not reuse company names, product imagery, logos, or this data in a way that implies affiliation or endorsement; obtain any required permissions for reuse.

## Current status

- Data integrity and headline sales aggregates reviewed.
- New theme, measure patterns, report specification, implementation plan, and static concept preview prepared.
- Original PBIX intentionally left untouched because it had pre-existing local changes and Power BI Desktop was unavailable in the authoring environment; it is not included in the public-ready package.
- A true Power BI report screenshot and verification inside Power BI Desktop remain outstanding.
