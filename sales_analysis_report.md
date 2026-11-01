# AAL Australia – Q4 2020 Apparel Sales Analysis Report

**Project:** Applied Data Science with Python – Course-End Project  
**Dataset:** `AusApparalSales4thQrt2020.csv` (7 561 transaction records)  
**Period:** October – December 2020  
**Prepared by:** Data Science Team  

---

## Executive Summary

AAL (Australian Apparel Limited), established in 2000, operates across multiple Australian states serving four demographic groups: Kids, Women, Men, and Seniors. This report summarises a full end-to-end analysis of Q4 2020 sales data — covering data wrangling, descriptive statistics, temporal trend analysis, and visualisation — with the goal of identifying high-revenue states and recommending sales programmes for underperforming regions.

---

## 1. Data Overview

| Attribute | Value |
|---|---|
| Total Rows | 7 561 |
| Columns | 6 (`Date`, `Time`, `State`, `Group`, `Unit`, `Sales`) |
| Date Range | 1 Oct 2020 – 31 Dec 2020 (Q4) |
| States Covered | WA, NSW, VIC, QLD, SA, TAS (and others present in data) |
| Demographic Groups | Kids, Women, Men, Seniors |
| Time Periods | Morning, Afternoon, Evening, Night |

---

## 2. Data Wrangling

### 2.1 Missing Value Treatment

Both `isna()` and `notna()` were applied to systematically inspect each column for null entries.

**Findings and recommendations:**

| Column Type | Approach | Rationale |
|---|---|---|
| Numeric (`Unit`, `Sales`) with < 5 % nulls | Fill with **column median** | Median is robust to outliers; preserves distribution shape |
| Categorical (`State`, `Group`, `Time`) with any nulls | Fill with **mode** (most frequent) | Preserves the natural frequency distribution |
| Rows with > 50 % fields missing | **Drop** the row | Imputation on sparse rows introduces excessive noise |

Dropping rows was treated as a last resort. The dataset is at the individual-transaction level, so imputation is strongly preferred to maintain temporal and group-level distributions.

### 2.2 Normalisation

**Method chosen: Min-Max Normalisation**

$$x_{\text{norm}} = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$

This scales all numeric values to the **[0, 1]** range, producing `Unit_Norm` and `Sales_Norm` columns alongside the originals.

**Why Min-Max over Z-score Standardisation?**

| Criterion | Min-Max | Z-score |
|---|---|---|
| Values remain non-negative | Yes | No (can produce negatives) |
| Business interpretability | Yes – "0 % – 100 % of range" is intuitive | Less intuitive |
| Explicit project preference | Yes – required by specification | — |
| Sensitivity to outliers | Moderate | More robust |

Min-Max is appropriate here because Sales values are naturally bounded, non-negative monetary figures, and business stakeholders benefit from a 0–1 scale when comparing across groups.

### 2.3 GroupBy Insights

Two primary strategies were applied:

| Strategy | Use case | Applied to |
|---|---|---|
| **Chunking** (split-apply-combine) | Aggregate metrics per segment | State totals, group totals, time-of-day totals |
| **Merging / Pivot** | Compare segments side-by-side | State × Group sales matrix, Time × Group heatmap |

**Recommendation:** Use **chunking** (`groupby().agg()`) as the primary strategy for ranking and segment comparison. Use **pivot tables** (`pivot_table()` / `unstack()`) only when a cross-tabular view is required for heatmaps or multi-dimensional dashboards.

---

## 3. Descriptive Statistical Analysis

### 3.1 Sales Column

The Sales column represents individual transaction revenue in AUD. After executing the notebook the following statistics were computed directly from the 7 561 transaction records:

| Statistic | Value |
|---|---|
| Mean | ~$44,900 per transaction |
| Median | ~$37,500 per transaction |
| Mode | $2,500 (most common single-transaction value) |
| Standard Deviation | ~$38,000 |
| Min | $0 |
| Max | $160,000 |

> **Interpretation:** The mean ($44,900) exceeds the median ($37,500), confirming the Sales distribution is right-skewed. A small proportion of high-value transactions (bulk orders, premium lines) pull the average upward. The standard deviation is large relative to the mean (coefficient of variation ~85 %), meaning transaction sizes are highly variable — typical for a multi-demographic retail business with both low-unit and bulk purchases.

### 3.2 Units Column

The Unit column records the number of items in each transaction. Statistics from the executed notebook:

| Statistic | Value |
|---|---|
| Mean | ~18 units per transaction |
| Median | ~15 units per transaction |
| Mode | 1 (single-item purchases are most common) |
| Standard Deviation | ~14 units |
| Min | 1 |
| Max | 65 |

> **Interpretation:** The low mode (1 unit) combined with a mean of 18 indicates a bimodal purchase pattern — many small single-item purchases coexist with occasional large bulk orders (up to 65 units). This is relevant for inventory planning: the distribution is right-skewed, so safety stock calculations should account for the tail of large-volume transactions.

### 3.3 Box Plot Analysis

Box plots were generated for both `Sales` and `Unit` columns segmented by `Group` and by `State`. Key takeaways:

- **Outliers in Sales:** Certain states or groups show extreme high-value transactions (whisker outliers), suggesting either bulk corporate orders or seasonal promotional spikes.
- **Spread comparison across groups:** Groups with wider IQR (interquartile range) have more variable transaction sizes, making demand forecasting harder.
- **Symmetry:** Near-symmetric boxes indicate balanced distributions; skewed boxes suggest concentration at either low or high transaction values.

---

## 4. Temporal Aggregations

### 4.1 Weekly Report

Sales were aggregated by ISO week number. The executed notebook revealed the following pattern across weeks 40–53:

| Week Range | Total Sales | Pattern |
|---|---|---|
| Week 40 (partial) | $15.0M | Low — only a partial week of October data |
| Weeks 41–43 | $27.0M – $26.8M | Stable early-October baseline |
| Weeks 44–48 | $20.9M – $21.8M | November trough — lowest sustained period |
| Weeks 49–52 | $29.6M – $31.8M | December surge — Christmas shopping uplift |
| Week 53 (partial) | $13.8M | Low — partial final week of December |

Key finding: Weeks 49–52 (late November through late December) represent the peak trading period, with sales ~50 % higher than the November trough. Promotional investment should be front-loaded into week 49 to capture early Christmas shoppers.

### 4.2 Monthly Report

| Month | Total Sales | vs. Q4 Average | Observation |
|---|---|---|---|
| October | ~$115M | +10 % | Strong open to Q4; stable daily run-rate ~$3.7M–$4.0M |
| November | ~$90M | -13 % | Mid-quarter trough; daily sales dipped to ~$2.8M–$3.2M |
| December | ~$135M | +30 % | Peak month; Christmas surge lifted daily sales to ~$4.0M–$4.8M |

December is the single largest revenue month at ~$135M — 50 % more than November. The November dip is notable and suggests an opportunity to introduce a mid-quarter promotional event (e.g., a November sales campaign) to smooth the revenue curve.

### 4.3 Quarterly Summary (Q4 2020)

| Metric | Value |
|---|---|
| Total Q4 Sales | ~$340M |
| Total Units Sold | ~136,000 |
| Average Daily Sales | ~$3.7M |
| Peak Day Sales | ~$4.8M (mid-December) |
| Trough Day Sales | ~$2.8M (early November) |
| States Active | 7 (VIC, NSW, SA, QLD, TAS, NT, WA) |
| Total Transactions | 7,561 |

The Q4 2020 quarterly baseline of ~$340M provides the benchmark for 2021 planning. December alone (~$135M) contributes approximately 40 % of the quarter's total revenue.

---

## 5. State-Wise Analysis

### 5.1 State Revenue Rankings (from executed notebook)

| Rank | State | Total Q4 Sales | Share of Q4 Revenue |
|---|---|---|---|
| 1 | VIC | $105.6M | 31 % |
| 2 | NSW | $75.0M | 22 % |
| 3 | SA | $58.9M | 17 % |
| 4 | QLD | $33.4M | 10 % |
| 5 | TAS | $22.8M | 7 % |
| 6 | NT | $22.6M | 7 % |
| 7 | WA | $22.2M | 7 % |

VIC alone generates nearly one-third of all Q4 revenue. The top-3 states (VIC, NSW, SA) account for 70 % of total revenue despite representing only 43 % of states.

### 5.2 High-Revenue States

States at the top of the sales ranking (VIC, NSW, SA) represent AAL's most mature, high-density markets. These states should receive:

- **Investment in inventory depth** – ensure stock availability for the highest-demand products.
- **Premium loyalty programmes** – to retain the existing high-value customer base.
- **Expansion of store footprint** within metropolitan areas.

### 5.3 Low-Revenue States

The bottom-4 states (QLD, TAS, NT, WA) each generate $22–33M — roughly one-fifth of VIC's revenue. These states represent growth opportunities. Recommended programmes:

| Programme | Description |
|---|---|
| **Hyper-personalisation** | Use transaction-level data to personalise promotions for each demographic group in low-revenue states. |
| **Next Best Offer (NBO)** | ML-driven offer engine to surface the most relevant product at the right time-of-day. |
| **Seasonal flash sales** | Time-limited promotions aligned to off-peak periods to build awareness and drive trial. |
| **Demographic targeting** | If a specific group (e.g., Kids) underperforms in a state, run school-season campaigns targeting parents. |

---

## 6. Group-Wise Analysis

### 6.1 Highest and Lowest Sales Groups

From the executed notebook output:

- **Highest-sales group: Women** — $83.9M total Q4 sales, driven by strong performance in VIC and NSW.
- **Lowest-sales group: Seniors** — $82.7M total Q4 sales, narrowly behind Kids at $83.0M.

Note: All four groups are remarkably close in total revenue (~$82–84M each), as confirmed by the box plots showing near-identical IQR and median ranges. This indicates AAL has successfully achieved balanced demographic coverage across its product range.

**Group-specific recommendations:**

| Group | Strategy |
|---|---|
| **Women** | Maintain stock depth; invest in seasonal collections aligned with fashion cycles. |
| **Men** | Focus on workwear and activewear as growth categories. |
| **Kids** | Tie promotions to school year calendar (back-to-school, winter/summer uniform periods). |
| **Seniors** | Comfort-focused and value-for-money messaging; loyalty scheme with senior discounts. |

---

## 7. Time-of-Day Analysis

### 7.1 Peak and Off-Peak Periods

| Period | Typical Behaviour | Recommended Action |
|---|---|---|
| **Morning** | Early commuter purchases, convenience-driven | Promote easy-to-wear basics; click-and-collect campaigns |
| **Afternoon** | Lunchtime shopping spike | Flash deals and "today only" offers; in-store foot traffic peaks |
| **Evening** | Post-work browsing; online channel dominant | Email/push notification campaigns; premium upsell opportunities |
| **Night** | Low foot traffic; online only | Automated personalised retargeting; abandoned-cart nudges |

The S&M team should schedule hyper-personalisation campaigns and Next Best Offer (NBO) triggers to align with the **peak time-of-day** identified in the data to maximise conversion rates.




---

##  Conclusions

1. **Revenue concentration:** A small number of states likely account for the majority of Q4 2020 revenue. These should be protected and grown with premium investment.

2. **Under-performing states represent the highest growth potential.** Targeted sales programmes — hyper-personalisation, NBO, and demographic-specific campaigns — should be rolled out starting Q1 2021.

3. **Timing matters:** Peak selling periods (time-of-day and day-of-week) should drive campaign scheduling. Off-peak periods should be leveraged with awareness-building rather than conversion-focused activity.

4. **Demographic diversification:** If any group is significantly under-represented in certain states, tailored regional-demographic programmes should be designed.

5. **Data quality is strong:** After wrangling, no material data quality issues were identified. Future quarters should adopt the same data collection schema to enable consistent year-over-year comparison.

