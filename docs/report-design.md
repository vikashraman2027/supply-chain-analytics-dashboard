# Report design specification

## Visual system

- **Canvas:** 16:9 widescreen; light slate background and white visual surfaces.
- **Palette:** deep navy for structure, teal for primary series, blue for comparison, amber for attention, and red for adverse exceptions. Use labels/icons as well as color.
- **Typography:** Segoe UI; use a clear title/subtitle hierarchy, restrained chart labels, and consistent currency/percentage formats.
- **Navigation:** persistent page navigation with short, readable names; remove decorative clutter and avoid a large overlay filter panel.
- **Filtering:** compact report-level date range and a small set of relevant page slicers; show active filter context and provide a reset action.
- **Accessibility:** maintain text/background contrast, use descriptive titles and alt text, avoid red/green as the sole status cue, and keep keyboard focus order meaningful.

## Pages and questions answered

### 1. Executive overview

**Question:** Are commercial results healthy, and where should leadership act?

- KPI cards: net revenue, profit, profit margin, delivered shipment share (explicitly labeled as a shipment-record share).
- Monthly net revenue and profit trend; channel mix; top product/category contribution.
- Compact exception callouts for shipment-key quality and production-rate reconciliation.
- Default time window and latest refresh date displayed; do not show `perfect_order_rate` until the source definition is validated.

### 2. Commercial performance

**Question:** Which periods, channels, customers, and products drive revenue and profit?

- Monthly net revenue and profit with a prior-year comparison.
- Channel mix and customer contribution, using the validated sales-to-customer relationship.
- Product/category revenue and profit margin; discount amount/rate with a clear net/gross basis.
- Avoid rankings that hide negative or low-volume results; support cross-filtering and detail drill-through.

### 3. Inventory and production

**Question:** Which stock positions need replenishment, and where are production defects concentrated?

- Inventory value/units from one selected snapshot, with date visible; do not sum snapshot values over time.
- Current stock vs safety stock and reorder point, with below-safety and below-reorder exceptions.
- Production output and a defect rate computed from validated counts after its definition is confirmed.
- Exclude or visibly flag the stored `defect_rate_pct` until reconciled.

### 4. Supplier performance

**Question:** Which suppliers balance quality, cost, and lead time?

- Average procurement lead time and average observed procurement quality score by supplier.
- Unit cost/trend with currency and aggregation stated.
- Supplier scorecard with sample/record counts to avoid over-interpreting small samples.
- Use the valid procurement supplier relationship; separate supplier master quality from transaction quality.

### 5. Shipment operations

**Question:** What is the status of shipments and where are delays occurring?

- Shipment count by status, delayed count, shipping cost trend, carrier comparisons, and delay reasons.
- Label 75.29% as "Delivered shipment share" with shipment records as its denominator.
- Show reason coverage (573 / 7,500) or delayed-only coverage; never treat blank reason as a reason category.
- Keep product/customer/facility drilldowns disabled until all three shipment key relationships are fixed.

### 6. Data quality

**Question:** Can users trust this slice of the report?

- Unmatched-key counts, missing-field coverage, and production defect-rate reconciliation status.
- Explain impact and remediation owner/source; distinguish data-quality indicators from operational KPIs.

## Interaction and delivery checklist

- Sync date filters across pages only where the date role is semantically the same.
- Use tooltip pages for definitions, numerator/denominator, units, and source fields.
- Add drill-through only where relationships are validated; include a back button.
- Test no-selection, multi-selection, empty-result, and latest-snapshot behavior.
- Use descriptive visual titles (avoid raw field names such as `Total_Delivered_Ship`).
- Confirm the report is readable at 1366 x 768 and 1920 x 1080; capture screenshots from Power BI Desktop after validating the PBIX.
