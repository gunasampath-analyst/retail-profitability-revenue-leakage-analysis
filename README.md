# Retail Profitability Analysis: Uncovering 23.5% Revenue Leakage Amid 41.6% Sales Growth

> Sales are growing — profitability isn't. This project traces where value is leaking between the top line and contribution profit, and what management should do about it.

---

## 📌 Business Background

The business represents a multi-category retailer selling Premium Furniture, Home Decor, Electronics Accessories, Office Supplies, and Outdoor & Garden products across Direct Retail, Wholesale, and Marketplace channels. Management observed a pattern that should concern any leadership team: sales were climbing steadily, but profit margin was moving in the opposite direction. The objective of this analysis was to determine whether discounting, product mix, channel mix, returns, shipping costs, or other cost pressures were contributing to that margin erosion — and to quantify each one so management could prioritize action instead of guessing.

*Note: This is a synthetic dataset built to model a realistic revenue-leakage scenario for analytical practice. The business scenario above is illustrative; the dashboard figures below are the actual output of the analysis performed on this dataset.*

---

## 🎯 Business Objective

Give management a clear, prioritized answer to: **where is profit being lost, and what should be fixed first?**

Specifically, the analysis was built to help leadership decide:

- Where is revenue being generated, and where is it leaking before it reaches contribution profit?
- Which categories, channels, products, or regions are dragging down blended profitability?
- Is discounting, returns, shipping cost, or channel commission the biggest lever to pull?
- Which specific SKUs or segments need immediate intervention vs. long-term monitoring?

---

## ❓ Business Questions

1. Is revenue growth being offset by a decline in profit margin, and by how much?
2. Where is revenue leakage concentrated — which cost category takes the biggest bite out of sales?
3. Are discounts contributing meaningfully to weaker profitability, and which category absorbs most of that discount spend?
4. Which products are structurally unprofitable once shipping and discounting are accounted for?
5. Which channel delivers the best margin, and which one is diluting blended profitability despite strong revenue?
6. Do return rates vary meaningfully by product, and is that concentrated in a specific category?
7. Which regions underperform their peers on margin despite comparable sales volume?

---

## 📊 Dataset

- **Type:** Synthetic transactional sales dataset, modeled to reflect a realistic revenue-leakage scenario
- **Grain:** One row per order line
- **Volume:** ~20,000 order lines across ~900 customers, 26 products, 5 regions, 3 sales channels, and 12 sales representatives
- **Time period:** January 2023 – December 2025 (36 months)
- **Key dimensions:** Product / Category, Customer / Segment, Region, Sales Channel, Sales Rep, Date
- **Key measures (raw fields):** Quantity, List Unit Price, Discount %, Unit Cost, Shipping Cost, Return Flag

---

## 🧹 Data Preparation

- Standardized date fields (`OrderDate`, `SignupDate`) to proper date types to support reliable time-based analysis and year-over-year comparison.
- Corrected numeric, percentage, and Boolean field types so transactional values could be reliably used in analytical calculations.
- The raw data intentionally stores granular transactional fields (price, cost, discount, quantity) with no pre-calculated financial metrics — Revenue, COGS, Gross Profit, and Margin % were built as DAX measures rather than static columns, so the calculations respond dynamically to filters such as year, region, channel, and category.

---

## 🏗️ Data Model

The model follows a **star-schema-oriented structure**:

- **Fact table:** `Fact_Sales` — one row per order line containing product, customer, channel, sales representative, date, quantity, price, discount, cost, shipping, and return information.
- **Dimension tables:** `Dim_Product`, `Dim_Customer`, `Dim_Region`, `Dim_Channel`, `Dim_SalesRep`
- **Date table:** A dedicated `DateTable`, marked as the official Date Table in the model, built specifically to support time intelligence such as year-over-year comparisons and rolling 12-month trends.
- **Relationships:** Dimension tables connect to `Fact_Sales` through one-to-many relationships, allowing sales and profitability measures to respond consistently to business filters.

This structure keeps the analytical measures filter-context aware — slicing by Year, Region, Channel, Product, or Category recalculates the KPIs and analytical measures rather than relying on hardcoded results.

---

## 🧮 DAX & Analytical Approach

The analysis is built primarily on dynamic DAX measures rather than static financial calculations, so the dashboard responds to slicers and cross-filtering:

- **Core profitability measures** (`SUMX`, `DIVIDE`) calculate Revenue, COGS, Gross Profit, and Margin % at the required analytical granularity rather than relying on pre-aggregated fields.
- **Leakage decomposition measures** isolate the impact of discounting, returns, shipping cost, and channel commission separately, allowing each leakage driver to be sized and compared rather than blended into a single cost figure.
- **Time-intelligence measures** (`SAMEPERIODLASTYEAR`, `DATESINPERIOD`) compare current performance against prior-year periods and calculate rolling 12-month trends, allowing growth and margin movement to be evaluated over time.
- **Customer segmentation logic** (`AVERAGEX`, `SWITCH`, context transition via `CALCULATE`) classifies customers based on revenue and profitability relative to the average customer and supports ranking of higher-priority customer groups.
- **A disconnected steps table** drives a waterfall/bridge visual that walks from Gross List Revenue through the identified leakage components to the resulting profit measure — turning a static KPI list into a visual argument about where value disappears.

The DAX layer therefore supports the business analysis rather than simply powering individual charts.

---

## 📈 Dashboard Story

The dashboard follows a four-stage business story:

**Executive Performance → Revenue Leakage → Product & Customer Profitability → Management Action**

---

### Page 1 — Executive Overview

**Business question:** *Are sales growing while profitability is deteriorating?*

Headline KPIs — Total Sales, Gross Profit, Margin %, Return Rate, Average Discount, and Orders — are combined with a 36-month sales-versus-profit trend and a profit-margin trend.

This page establishes the core tension that the rest of the report investigates: **revenue is growing while profitability is under pressure.**

---

### Page 2 — Revenue Leakage

**Business question:** *Where is our revenue leaking?*

This page breaks total identified leakage into four components — Discounts, Returns, Shipping, and Channel Commission — through a revenue-to-profit bridge, discount leakage by category, discount-versus-margin analysis, return rates by product, and channel profitability.

The objective is to identify both **how much value is being absorbed** and **which mechanisms are responsible**.

---

### Page 3 — Product & Customer Profitability

**Business question:** *Who is driving profit — and who is destroying it?*

This page moves from company-level leakage to the products and customers behind the result.

The analysis includes:

- Product-level profitability
- Top products by sales
- Lowest-margin products
- Customer contribution
- High-revenue / low-profit customer analysis

The purpose is to identify where the profitability problem is concentrated and distinguish high-revenue activity from genuinely profitable activity.

---

### Page 4 — Management Action Center

**Business question:** *What should management do about it?*

The final page synthesizes the findings from Pages 1–3 into four decision-oriented categories:

- Critical Leakage
- Margin Opportunities
- Growth Opportunities
- Recommended Actions

The objective is to convert analytical findings into prioritized management actions without requiring a decision-maker to interpret every underlying chart.

---

## 🔎 Key Insights

### 1. Revenue growth is masking a structural margin problem.

**Observation:** Sales increased from approximately **₹1.04M in 2023 to ₹1.48M in 2025 — a 41.6% increase** — while profit margin declined from approximately **34.1% to 17.9%**.

**Meaning:** The business is generating substantially more revenue while retaining significantly less profit from that revenue.

**Business Impact:** Management should focus on the quality of growth rather than evaluating performance through revenue growth alone.

---

### 2. Nearly a quarter of gross list revenue is absorbed by identified leakage drivers.

**Observation:** Total identified leakage across discounts, returns, shipping, and channel commission equals **₹895.26K, or 23.5% of gross list revenue**. Discounting alone accounts for **₹335.50K, or 8.8% of sales**.

**Meaning:** The margin problem is not driven by a single cost category. Four distinct leakage mechanisms are absorbing value.

**Business Impact:** Management needs a targeted response across pricing, product, operations, and channel economics rather than a single company-wide cost-cutting initiative.

---

### 3. One category absorbs the vast majority of discount spend.

**Observation:** Premium Furniture accounts for approximately **₹2.6L of the ₹3.355L total discount leakage — approximately 78% of all discount leakage**.

**Meaning:** Discounting is highly concentrated rather than evenly distributed across categories.

**Business Impact:** Premium Furniture represents the highest-leverage area for discount governance. Even a modest reduction in unnecessary discounting could materially reduce the largest identified leakage component, subject to customer demand response.

---

### 4. The Marketplace channel is diluting blended profitability despite carrying nearly half of revenue.

**Observation:** Marketplace generates approximately **43.4% of total sales** but has only a **16.36% profit margin**, compared with **33.28% for Direct Retail**.

**Meaning:** Marketplace produces significant revenue volume but converts that revenue into profit much less efficiently.

**Business Impact:** Continued growth through this channel, without changes to pricing, commission economics, or channel mix, could continue to pressure blended profitability even as top-line sales increase.

---

### 5. Product-level returns are concentrated, not random.

**Observation:** Electronics Accessories products account for **6 of the top 10 highest-return-rate products**, led by Bluetooth Earbuds Basic at **8.99%** and Laptop Stand at **8.48%**, compared with the company-wide return rate of **5.14%**.

**Meaning:** Elevated returns are concentrated within a specific category and a defined group of products.

**Business Impact:** Product-level investigation should be prioritized for these SKUs rather than assuming the return problem is evenly distributed across the business.

---

## 💸 Revenue Leakage Analysis

The Revenue Leakage page decomposes the value absorbed between Gross List Revenue and the resulting profit measure into four measurable components:

| Leakage Driver | Amount | % of Sales |
|---|---:|---:|
| Discount Leakage | ₹335.50K | 8.8% |
| Returned Sales | ₹151.29K | 4.0% |
| Shipping Leakage | ₹187.81K | 4.9% |
| Channel Commission | ₹220.65K | 5.8% |
| **Total Leakage** | **₹895.26K** | **23.5%** |

Two distinct leakage patterns emerge from this breakdown:

- **A concentration problem — discounting:** Approximately 78% of discount leakage sits in Premium Furniture, meaning the largest leakage component has a highly targeted area for investigation.
- **A structural problem — commission, shipping, and returns:** These leakage drivers are linked to how and where products are sold, making the solution more dependent on channel economics, product economics, and operational performance.

The purpose of this analysis is not to assume that every identified leakage dollar can be eliminated. Instead, it identifies where management should investigate controllable or potentially avoidable value loss.

---

## 🚀 Actionable Recommendations

| Priority | Finding | Recommended Action | Business Owner | Potential Impact | Caveat |
|---|---|---|---|---|---|
| 1 | Premium Furniture drives approximately 78% of discount leakage (₹2.6L) | Introduce capped discount thresholds for high-value Premium Furniture SKUs | Sales / Commercial | Potential reduction in the largest identified leakage component | Assumes demand holds at a lower average discount; price elasticity was not tested |
| 2 | Marketplace carries 43.4% of sales but only 16.36% margin | Renegotiate commission terms with the Marketplace partner or rebalance growth toward higher-margin channels | Channel / Commercial | Potential improvement in blended margin without necessarily reducing total revenue | Commission terms are contractual; renegotiation feasibility was not assessed |
| 3 | Eight products carry negative contribution margin, led by Screen Protector 2-Pack, Sticky Notes Bulk Pack, Ergonomic Mouse Pad, and Phone Case - Clear | Review pricing, cost structure, and product-level economics; consider repricing or discontinuing persistently negative-margin SKUs | Product / Pricing | Potential reduction in exposure to actively loss-making products | No minimum viable margin threshold was defined in this analysis |
| 4 | Electronics Accessories account for 6 of the top 10 highest-return products | Investigate product quality, listing accuracy, and return drivers for the highest-return SKUs | Product / Quality | Potential reduction in returned-sales leakage concentrated in one category | Root cause — defect, expectation mismatch, or shipping damage — cannot be distinguished from this dataset |
| 5 | Central region has a margin of 24.37%, approximately 1.8 percentage points below East, the highest-margin region | Audit Central region's discount approval and return-handling practices | Regional Management | Potential reduction in the regional margin gap | The underlying regional root cause was not established in this analysis |

> **Note:** Potential impact is intentionally described as potential rather than guaranteed financial impact. The dataset does not establish causality or quantify the financial benefit of implementing each recommendation.

---

## 🧭 Management Takeaway

> **Revenue growth alone is not evidence of a healthy business — this analysis shows that 23.5% of gross list revenue is absorbed by identified leakage drivers, concentrated in discounting, channel economics, returns, and a small number of loss-making products.**

---

## ⚠️ Limitations

- **No marketing or acquisition cost data.** Revenue growth and channel mix shifts cannot be evaluated against the cost required to generate that growth.
- **No cost breakdown beyond the available transactional cost fields.** Overhead, labor, and fixed operating costs are outside the scope of the profitability analysis.
- **No causal information.** The analysis identifies where leakage is concentrated — category, channel, product, and region — but does not establish why the leakage occurs.
- **No competitor pricing data.** The analysis cannot determine whether current discount levels are competitive or how competitor pricing influences customer demand.
- **No operational root-cause information.** Returns can be quantified by product and category, but the dataset does not identify whether they are caused by product quality, listing accuracy, customer expectations, or logistics.
- **No inventory or availability data.** The analysis cannot determine whether stockouts or inventory constraints influenced sales performance.
- **Synthetic dataset.** The leakage patterns are designed to represent a realistic business scenario, but the underlying data does not represent an actual operating company.
- **Historical analysis.** The 2023–2025 trends describe historical performance and cannot guarantee future profitability or the outcome of recommended actions.

---

## 🛠️ Tools & Skills

### Tools

- Excel 
- Power BI Desktop
- DAX
- Power Query

### Power BI

- Star-schema data modeling
- Dedicated Date table with time-intelligence functions
- DAX measures
- KPI development
- Profitability analysis
- Revenue leakage analysis
- Interactive dashboard design
- Cross-filtering and slicer-driven analysis
- Conditional formatting
- Executive data visualization
- Business storytelling
- Management decision support

### DAX / Analytics

- `CALCULATE`
- `SUMX`
- `DIVIDE`
- `FILTER`
- `RANKX`
- `SWITCH`
- `AVERAGEX`
- `SAMEPERIODLASTYEAR`
- `DATESINPERIOD`
- Time Intelligence
- Year-over-Year analysis
- Rolling 12-month analysis
- Ranking
- Contribution analysis
- Customer segmentation
- Conditional business logic
- Dynamic measures

### Project Highlights

- Business-focused Power BI dashboard rather than a chart gallery
- **₹895.26K identified revenue leakage**
- **23.5% of gross list revenue represented by identified leakage drivers**
- Executive KPI reporting with year-over-year context
- Product and customer profitability analysis
- Regional and channel profitability analysis
- Discount and return analysis
- Time-based performance analysis
- DAX-driven business metrics
- Revenue leakage decomposition
- Decision-oriented storytelling
- Dedicated Management Action Center
- Prioritized recommendations with analytical caveats

---

## 👤 Project Role

**Data Analyst / BI Analyst — End-to-End Project**

Responsible for:

- Translating the business problem into analytical questions
- Structuring the Power BI data model
- Preparing and validating business metrics
- Developing DAX measures
- Building time-intelligence and profitability calculations
- Designing multi-page Power BI dashboards
- Identifying profitability and revenue leakage drivers
- Analyzing product, customer, regional, and channel performance
- Converting analytical findings into management recommendations
- Developing an executive-level business narrative

---

## 📌 Recruiter Snapshot

| Hiring Area | Evidence Demonstrated |
|---|---|
| Business Thinking | Revenue growth vs. profitability analysis |
| Analytical Thinking | Revenue leakage and profitability diagnosis |
| DAX | KPI, time intelligence, ranking, segmentation, and analytical measures |
| Power BI | Multi-page interactive dashboard |
| Data Modeling | Fact and dimension structure with dedicated Date table |
| Data Visualization | Executive, diagnostic, and decision-oriented storytelling |
| Business Analysis | Findings translated into management actions |
| Commercial Understanding | Discounts, returns, commissions, shipping, and margin analysis |
| Decision Making | Prioritized recommendations with caveats |
| Communication | Executive-level narrative and concise recommendations |
| Quantified Findings | ₹895.26K identified leakage / 23.5% of gross list revenue |
| Analytical Maturity | Explicit limitations, assumptions, and next-analysis recommendations |
