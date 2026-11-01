# 🛍️ AAL Australia – Q4 2020 Apparel Sales Analysis

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?logo=pandas)
![Seaborn](https://img.shields.io/badge/Seaborn-Matplotlib-brightgreen)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

> **Course-End Project** · Applied Data Science with Python  
> End-to-end EDA and visualisation of Australian Apparel Limited (AAL) retail sales for Q4 2020.

---

## 📋 Table of Contents

1. [Project Overview](#-project-overview)
2. [Business Problem](#-business-problem)
3. [Dataset](#-dataset)
4. [Project Structure](#-project-structure)
5. [Methodology](#-methodology)
6. [Key Findings](#-key-findings)
7. [Visualisations](#-visualisations)
8. [Getting Started](#-getting-started)
9. [Requirements](#-requirements)
10. [Results & Recommendations](#-results--recommendations)

---

## 🎯 Project Overview

AAL (Australian Apparel Limited), founded in 2000, is a multi-state fashion retailer serving four demographic groups — **Kids, Women, Men, and Seniors** — across all major Australian states and territories.

This project delivers a complete **Exploratory Data Analysis (EDA)** of AAL's Q4 2020 (October–December) sales transactions. The goal is to surface actionable insights that support data-driven investment and marketing decisions for 2021.

---

## 💼 Business Problem

The sales director needs to understand:

- **Which states** generate the most (and least) revenue?
- **Which demographic groups** drive sales, and where?
- **When** do customers buy — what time of day, week, and month peaks?
- **What targeted programmes** should be designed for underperforming states?

---

## 📂 Dataset

| Attribute | Detail |
|---|---|
| **File** | `AusApparalSales4thQrt2020.csv` |
| **Records** | 7,560 transactions |
| **Period** | 1 Oct 2020 – 31 Dec 2020 |
| **Columns** | `Date`, `Time`, `State`, `Group`, `Unit`, `Sales` |
| **States** | VIC, NSW, SA, QLD, TAS, NT, WA |
| **Groups** | Kids, Women, Men, Seniors |
| **Time Periods** | Morning, Afternoon, Evening, Night |

---

## 🗂️ Project Structure

```
Sales_Analysis_Project/
│
├── sales_analysis.ipynb               # Main analysis notebook
├── AusApparalSales4thQrt2020.csv      # Raw dataset
├── sales_analysis_report.md           # Full written report with findings
│
├── fig_dashboard.png                  # Summary dashboard
├── fig_state_ranking.png              # State-wise revenue ranking
├── fig_groupwise_by_state.png         # Group sales broken down by state
├── fig_state_by_group.png             # State sales broken down by group
├── fig_heatmap_state_group.png        # State × Group sales heatmap
├── fig_boxplot_group.png              # Sales distribution by group
├── fig_boxplot_state.png              # Sales distribution by state
├── fig_monthly_trend.png              # Monthly sales trend
├── fig_weekly_trend.png               # Weekly sales trend
├── fig_daily_trend.png                # Daily sales trend
├── fig_time_of_day.png                # Sales by time of day
├── fig_tod_group_heatmap.png          # Time-of-day × Group heatmap
├── fig_kde_group.png                  # KDE plot – sales density by group
│
└── README.md                          # This file
```

---

## 🔬 Methodology

The notebook follows a structured data science workflow:

### 1. Environment Setup & Data Loading
- Import `numpy`, `pandas`, `scipy`, `matplotlib`, `seaborn`
- Load and inspect the CSV; confirm shape and dtypes

### 2. Data Wrangling
- **Missing-value treatment:** numeric nulls filled with column median; categorical nulls filled with mode; rows with >50 % missing values dropped
- **Normalisation:** Min-Max scaling applied to `Unit` and `Sales` → produces `Unit_Norm` and `Sales_Norm` columns (range [0, 1])
- **GroupBy insights:** split-apply-combine aggregations (`groupby().agg()`) for segment-level summaries; pivot tables for cross-tabular heatmap views

### 3. Descriptive Statistics
- Central tendency (mean, median, mode) and spread (std dev, IQR, min/max)
- Distribution analysis via box plots — by `Group` and by `State`

### 4. Temporal Aggregations
- **Weekly** aggregation (ISO week numbers 40–53)
- **Monthly** aggregation (Oct / Nov / Dec)
- **Quarterly** summary KPIs

### 5. Visualisation
- State-wise revenue rankings (bar chart)
- Group-wise sales by state (stacked/grouped bars)
- State × Group heatmap
- Time-of-day sales patterns
- KDE plots for sales density

---

## 📊 Key Findings

### State Revenue Rankings

| Rank | State | Q4 Sales | Share |
|---|---|---|---|
| 1 | **VIC** | ~$105.6M | 31 % |
| 2 | **NSW** | ~$75.0M | 22 % |
| 3 | **SA** | ~$58.9M | 17 % |
| 4 | QLD | ~$33.4M | 10 % |
| 5 | TAS | ~$22.8M | 7 % |
| 6 | NT | ~$22.6M | 7 % |
| 7 | WA | ~$22.2M | 7 % |

> VIC alone contributes **~31 %** of total Q4 revenue. The top-3 states account for **70 %** of revenue.

### Temporal Trends
- **December** is the peak month (~$135M) — 50 % higher than November (~$90M)
- **Weeks 49–52** show the Christmas shopping surge, up ~50 % vs. the November baseline
- **November** is a consistent trough — an opportunity for a mid-quarter promotional event

### Demographic Groups
- All four groups are remarkably balanced (~$82–84M each), confirming broad demographic coverage
- **Women** edges out as the top group; **Seniors** is the narrowest segment

---

## 🖼️ Visualisations

| Chart | Description |
|---|---|
| `fig_dashboard.png` | One-page executive summary dashboard |
| `fig_state_ranking.png` | Bar chart of total Q4 sales per state |
| `fig_heatmap_state_group.png` | Heat-map of sales intensity across State × Group |
| `fig_monthly_trend.png` | Month-over-month line chart |
| `fig_time_of_day.png` | Sales volume by time-of-day period |
| `fig_boxplot_group.png` | Distribution spread per demographic group |

---

## 🚀 Getting Started

### Clone the repository

```bash
git clone https://github.com/Abdurrahman-Sabbahi/Sales_Analysis_Project.git
cd Sales_Analysis_Project
```

### Install dependencies

```bash
pip install numpy pandas scipy matplotlib seaborn notebook
```

### Launch the notebook

```bash
jupyter notebook sales_analysis.ipynb
```

Run all cells top-to-bottom (`Kernel → Restart & Run All`).

---

## 📦 Requirements

| Library | Version |
|---|---|
| Python | ≥ 3.10 |
| numpy | ≥ 1.24 |
| pandas | ≥ 2.0 |
| scipy | ≥ 1.11 |
| matplotlib | ≥ 3.7 |
| seaborn | ≥ 0.12 |
| jupyter | ≥ 1.0 |

---

## ✅ Results & Recommendations

| Priority | Recommendation |
|---|---|
| **Protect** VIC / NSW / SA | Invest in inventory depth and premium loyalty programmes |
| **Grow** QLD / TAS / NT / WA | Deploy hyper-personalisation and Next Best Offer (NBO) campaigns |
| **November campaign** | Introduce a mid-quarter promotional event to lift the seasonal trough |
| **Demographic targeting** | Align school-season promotions for Kids; comfort/value messaging for Seniors |
| **Peak-hour scheduling** | Front-load digital campaigns to afternoon and evening peak windows |

For the full written analysis see [`sales_analysis_report.md`](sales_analysis_report.md).

---

*Applied Data Science with Python – Course-End Project*
