# Implementation plan

## Objective

Turn the existing supply-chain Power BI project into a credible, executive-first portfolio dashboard without overstating what the supplied data can support or overwriting the existing PBIX.

## Plan and status

| Phase | Work | Status |
| --- | --- | --- |
| 1. Baseline and data audit | Review report previews, inspect CSV schemas, test fact-to-dimension key coverage, and reconcile headline sales calculations. | Complete |
| 2. KPI contract | Define sales, margin, supplier, inventory, shipment, and production metrics; distinguish delivered share from on-time/perfect-order performance. | Complete |
| 3. Visual system and page design | Establish a high-contrast neutral theme, simplify navigation, make the executive overview the entry point, and specify analytical drill-down pages. | Complete |
| 4. Report assets | Add reusable DAX patterns, a report design specification, data caveats, a Power BI theme, and a static concept preview. | Complete |
| 5. Validate and package | Recheck figures and files; clearly label the concept and disclose report limitations. | Complete |
| 6. Apply in Power BI Desktop | Repair shipment key mappings, correct/replace the stored defect rate, apply theme and measures, validate interactivity, and capture a genuine report screenshot. | Blocked: Power BI Desktop is unavailable; original PBIX has uncommitted local changes |
| 7. Publish | Create the requested public portfolio repository and publish only the confirmed project data and authored supporting assets. | Prepared locally; blocked on GitHub sign-in/repository creation |

## Decisions and guardrails

- Preserve the existing `supply chain.pbix`; it had local uncommitted changes before this work.
- Do not create customer/product/facility relationships for shipment facts until a verified key crosswalk is available.
- Do not present `perfect_order_rate` or delivered shipment share as an on-time delivery KPI.
- Do not use the stored `defect_rate_pct` field until it is reconciled with `defective_units` and `quantity_produced`.
- Do not describe the CSV metrics as real company performance.
- The SVG/PNG preview is an original static concept, not a screenshot of Power BI.

## Acceptance criteria

- Documented data grain, key integrity, KPI definitions, and limitations.
- Reusable measures use validated columns and explicit denominators.
- Theme uses consistent, legible colors and avoids reliance on color alone.
- Dashboard concept shows an executive summary and operational exceptions with clear metric names.
- Original PBIX and its existing local changes remain untouched.
- Public publishing uses the confirmed repository name/visibility and does not silently publish unrelated or unapproved files.

## Remaining work to reach production-ready report status

1. Obtain a verified source-to-dimension crosswalk for shipment `product_id`, `customer_id`, and `facility_id`.
2. Confirm the business definition/source for `fact_production[defect_rate_pct]`; replace it with a recomputed rate if appropriate.
3. Open a working copy in Power BI Desktop, apply the theme/measures, repair relationships, and validate all pages and filter behavior.
4. Capture and review actual report screenshots at the intended presentation size.
5. Publish the updated PBIX and screenshot only after the owner confirms rights to the embedded third-party marks and imagery.
