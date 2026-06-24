# Business Insight Report — RetailMart BD

**Analysis Period:** January 2023 – December 2024

**Tools Used:** Excel, SQL Server (SSMS), Power BI

**Prepared by:** Suborno

---

## 1. Executive Summary

A 24-month (January 2023 – December 2024) analysis of RetailMart BD's inventory transaction data shows that while the company's overall inventory health is broadly satisfactory, one clear and actionable problem stands out: **৳75,980 worth of dead stock concentrated in just 5 products**, sitting idle for an average of 538 days. Although this represents only 1.25% of total inventory value, **100% of this dead stock capital originated from just two import suppliers (Singapore and India)** — pointing to a clear procurement risk pattern.

The analysis also reveals that the company's overall inventory turnover ratio (~1.3) is well below the healthy industry benchmark (4–8), reflecting a systemic, buffer-heavy procurement policy. At present, no product faces a stockout risk (Reorder Alert Count = 0), confirming that the company's core problem is not understocking — it is **selective overstocking and capital inefficiency**.

This report provides specific recommendations for dead stock liquidation, supplier risk mitigation, and a reassessment of procurement policy.

---

## 2. Project Background & Objective

RetailMart BD is a mid-sized FMCG retail company operating 3 warehouses (Dhaka Central, Chittagong Port, Sylhet Hub) and managing 35 SKUs, sourced from multiple suppliers and distributed nationwide.

The company's inventory management has been entirely reactive and intuition-driven — there was no data-backed system to identify which products were tying up capital, which products carried stockout risk, or when and how much to reorder.

### Objectives

1. **Inventory Segmentation** — Classify products by revenue contribution and demand predictability using ABC-XYZ analysis
2. **Dead & Slow Stock Identification** — Identify which products have been inactive, for how long, and how much capital is tied up
3. **Proactive Reorder Intelligence** — Build an early-warning system that flags risk before stockouts occur

---

## 3. Methodology

| Phase | Tool | Work Done |
|---|---|---|
| Data Cleaning | Excel | Fixed date formats, transaction-type inconsistencies, missing values, sign errors |
| Data Modeling | SQL Server (SSMS) | Designed a Star Schema (1 Fact + 4 Dimension tables) |
| Analysis | SQL Server | Built 8 analytical views (ABC, XYZ, Aging, Reorder, Turnover, Supplier Performance) |
| Visualization | Power BI | Built a 5-page interactive dashboard |

**Data Quality Approach:** No problematic rows were deleted. Every issue was root-cause analyzed and flagged via a `data_issue` column, then excluded from downstream analysis — preserving a full audit trail. A total of 138 transactions (~1.5%) were flagged this way.

---

## 4. Key Findings

### Finding 1 — Inventory Segmentation Reveals a Concentrated Risk Pool

**Observation:** ABC-XYZ analysis placed 5 of the 35 SKUs into the "CZ" segment — meaning they have the lowest revenue contribution (just 0.87% of total revenue) and the most volatile demand pattern (Coefficient of Variation 160–170%, compared to 14–31% for most other products).

**So What:** These 5 products aren't merely "low-selling" — they are effectively inactive. ABC analysis alone wasn't sufficient to flag them, since the C-category also contained 6 other products that were low-revenue but stable, not dead.

**Root Cause:** All 5 products belong to Lifestyle or niche categories (Premium Candle Set, Ceramic Mug Set, Aromatherapy Diffuser, Herbal Bath Salts, Imported Olive Oil) — non-essential, discretionary items where demand is inherently volatile and unpredictable.

---

### Finding 2 — ৳75,980 in Capital Tied Up, Idle for an Average of 538 Days

**Observation:** Stock Aging Analysis confirms that these 5 dead products (across 15 product-warehouse combinations) hold a combined ৳75,980 in stock, with the longest idle period — Herbal Bath Salts — reaching 657 days.

**So What:** Although small as a percentage (1.25%), this loss stemmed from a fully preventable decision, and the locked-up capital remains partially recoverable through liquidation.

**Root Cause:** These products were purchased in bulk based on early sales signals (pre-July 2023), but demand dropped sharply afterward, leaving no opportunity to sell through the excess stock.

---

### Finding 3 — 100% of Dead Stock Traces Back to Just Two Import Suppliers

**Observation:** Pacific Imports Ltd (Singapore) alone accounts for 81% of total dead stock capital (৳61,630), and Globe Traders (India) accounts for the remaining 19% (৳14,350). None of the company's 8 local Bangladeshi suppliers have any dead stock.

**So What:** This is not random — it's a systematic supplier-risk pattern. The supplier master data's `reliability_score` independently rated these two suppliers the lowest (72 and 78), which aligns directly with the actual outcome — two independent metrics confirming the same signal.

**Root Cause:** These two suppliers have the longest lead times (21–28 days, versus 4–12 days for local suppliers). Long lead times force procurement to order well in advance and in larger quantities to avoid stockouts — but for niche, low-demand products, that same buffer becomes dead stock.

---

### Finding 4 — Company-wide Inventory Turnover Is Low (a Systemic Issue, Not an Isolated One)

**Observation:** Even active, normal-demand products have a turnover ratio of at most 2.90 (Maggi Noodles), whereas a healthy FMCG business typically operates in the 4–8 range.

**So What:** Dead stock isn't an isolated problem confined to 5 products — it's the most visible symptom of a broader procurement pattern.

**Root Cause:** Reorder Point Analysis confirms that no product currently needs reordering (Reorder Alert Count = 0), proving that the company's procurement approach is risk-averse and buffer-heavy across the board.

---

### Finding 5 — No Stockout Risk, But This "Safety" Has a Hidden Cost

**Observation:** All 35 products show a reorder status of "Sufficient Stock" — meaning no product currently faces imminent stockout risk.

**So What:** Read alongside Finding 4, this reveals that the company maintains so much buffer to avoid stockouts that this very buffer is the root cause of dead stock and low turnover. Stockout risk and dead stock — two seemingly opposite symptoms — actually stem from the same root cause.

**Root Cause:** The company lacks a data-driven, demand-volatility-aware procurement strategy — the same "buy more to be safe" approach is applied uniformly across all products, regardless of their actual demand pattern.

---

## 5. Recommendations

### 🔴 Immediate Action (0–30 days)

**R1 — Liquidate the 5 CZ-Segment Products**
Launch a discount/liquidation campaign (30–50% discount, or bundling with fast-moving products) to recover at least part of the tied-up capital and free up warehouse space. *(Finding 1, 2)*

**R2 — Halt Future Reordering for These 5 Products**
Add a manual hold flag in the procurement system until a new demand signal emerges. *(Finding 2)*

### 🟡 Short-term Action (1–3 months)

**R3 — Revisit Order Policy with Pacific Imports Ltd and Globe Traders**
Shift to smaller, more frequent orders instead of large bulk orders. *(Finding 3)*

**R4 — Establish a Demand-Validation Process for Niche/Lifestyle Categories**
Validate demand with a small pilot batch (10–20% of the full order) before committing to a full order on new niche products. *(Finding 1, 3)*

### 🟢 Long-term Action (3–6+ months)

**R5 — Build a Demand-Volatility-Based Procurement Policy (SOP)**
Formally incorporate XYZ classification into procurement decisions — large bulk orders for X-category products, small and conservative orders for Z-category products. *(Finding 4, 5)*

**R6 — Explore Expanding Local Supplier Partnerships**
Seek out lower-lead-time alternative suppliers for niche product categories. *(Finding 3)*

**R7 — Implement a Quarterly Inventory Health Review**
Treat this dashboard as a living tool, not a one-time analysis — regular review will catch emerging dead stock early, before it accumulates for 500+ days.

---

## 6. Expected Impact

| Timeframe | Expected Outcome |
|---|---|
| **Immediate** | Liquidating the CZ segment could recover an estimated 40–60% of tied-up capital (~৳30,000–45,000) |
| **Medium-term** | Revised order policy should meaningfully reduce import-supplier-driven dead-stock risk |
| **Long-term** | A demand-volatility-based policy could gradually move the turnover ratio (~1.3) closer to the industry-standard range (4–8) |

> **Caveat:** These estimates are reasoned projections based on dataset patterns, not the result of a financial audit or pilot test. A small-scale pilot is recommended before full implementation to validate actual impact.

---

## 7. Data Quality & Limitations

- 138 transactions (~1.5%) were flagged and excluded due to data quality issues (not deleted — full audit trail preserved)
- Inventory Turnover Ratio uses period-end inventory value as an approximation of "Average Inventory"
- ~0.4% of transactions showed negative balances due to opening-stock timing overlap; these were flagged and excluded from analysis
- Some products in the "Slow Moving" bucket are not genuinely at risk — they were classified that way simply because their most recent sale fell close to the dataset's cutoff date (2024-12-31)

---

## Appendix — Dashboard Overview

The full analysis is presented through an interactive 5-page Power BI dashboard:

1. **Executive Overview** — KPI summary, aging distribution, category-wise revenue
2. **ABC-XYZ Segmentation** — 9-segment matrix, product-level detail
3. **Dead & Slow Stock Report** — aging breakdown, financial impact
4. **Reorder Alert Panel** — reorder status, supplier lead time
5. **Trend Analysis** — monthly patterns, seasonal insights

*(See dashboard screenshots in the [/04_screenshots](./04_screenshots) folder)*

---

**Tools:** Excel · SQL Server (SSMS) · Power BI
**Project Type:** Self-directed portfolio project
