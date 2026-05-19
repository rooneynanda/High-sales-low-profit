# OmniStore Retail: The Profitability Paradox Case Study
> **An End-to-End Data Analytics & Strategy Project diagnosing structural profit leakage using SQL, Advanced Excel, and Power BI.**

---

##  Executive Summary
OmniStore Retail is experiencing strong top-line revenue growth across global sales channels, yet bottom-line net profit margins are aggressively shrinking. This project functions as an **Operations & Commercial Diagnostic Tool** to identify why high-volume, flagship products are failing to generate expected cash returns. 

By executing a structured, cross-tool analytics pipeline, this study uncovers three multi-variable structural bottlenecks:
1. **Supply Chain Failure (Smart Home Appliances):** Breaking inventory demand forecasting models leading to chronic stockouts, forcing heavy appliances onto premium Express Air freight.
2. **The Commercial Discount Trap (Fast-Fashion Apparel):** Deep markdown structures ($30\% - 50\%$) driving artificial customer transaction velocity but triggering a toxic $28\%$ reverse-logistics return rate.
3. **Flagship Customer Acquisition Burn (Premium Electronics):** Disproportionately high marketing capital allocation (CAC) required to maintain market penetration, aggressively eroding core gross margins.

---

##  Data Architecture & Pipeline Overview

The project mirrors a production-ready corporate analytics pipeline, processing over 5,200 transactions across four sequential phases:
###  Phase 1: Database Engineering & Querying (SQL)
Raw data was ingested into a structured PostgreSQL database to filter high-sales operational segments, calculate baseline net metrics, and rank cost centers.

* **High-Discount Event Segmentation:** Isolating transactions where markdowns exceeded $30\%$ to assess shifts in average order size.
* **Logistics Overhead Audit:** Utilizing window ranking functions (`RANK() OVER`) to isolate premium freight spend by category, identifying *Smart Home Appliances* as the primary cost offender.

###  Phase 2: Advanced Financial Modeling (Excel)
A dynamic Management Decision-Support System (DSS) was engineered in Excel to run automated cell audits, compute granular risk triggers, and model line item economics.
* **Feature Engineering:** Modeled a custom `True_Net_Profit` and a `Return_Deficit` field factoring in full revenue reversals alongside a physical **$15 corporate restocking/handling penalty** per returned unit.
* **SKU Sourcing Desk:** Built an interactive, error-proof lookup interface using `XLOOKUP`, `SUMIFS`, and dynamic array filters (`INDEX/MODE/MATCH`) to auto-populate product attributes without user data-entry risk.

###  Phase 3: Star Schema Data Modeling (Power BI)
To optimize analytical performance and dynamic filtering behavior, the flat transaction file was broken into a centralized dimensional model.

* **Dimensional Modeling:** Established a $1:\text{Many}$ Star Schema linking unique product dimensions (`Dim_Product`) to granular transactional tables (`Fact_Sales`).
* **Explicit DAX Measures:** Avoided default implicit sums by writing robust calculation expressions:
    ```DAX
    Total_Net_Revenue = SUM(Fact_Sales[Net_Revenue])
    ```
    ```DAX
    Total_OpEx = SUM(Fact_Sales[Total_COGS]) + SUM(Fact_Sales[Shipping_Cost]) + SUM(Fact_Sales[Marketing_Spend_Allocated])
    ```
    ```DAX
    True_Net_Profit = [Total_Net_Revenue] - [Total_OpEx]
    ```
    ```DAX
    Return_Rate_Pct = DIVIDE(CALCULATE(COUNT(Fact_Sales[Transaction_ID]), Fact_Sales[Return_Status] = "Yes"), COUNT(Fact_Sales[Transaction_ID]), 0)
    ```

---

##  Strategic Business Insights

### 1. The Supply Chain Volumetric Penalty
* **The Diagnostic:** The *Smart Home Appliances* category (e.g., *HydroClean Dishwasher*) accounts for **481 Express Air transactions**, generating **$193,238.50 in premium shipping costs**—surpassing all other categories combined.
* **The Root Cause:** High order frequency proves stockouts are chronic due to poor forecasting. Because these appliances average an $85\text{ lbs}$ deadweight mass profile, emergency air-freight updates introduce severe volumetric airline penalties, devouring contribution margins down to a weak $21.3\%$.

### 2. The Promotional Markdown Illusion
* **The Diagnostic:** The *ComfortFit Denim Jacket* shows massive transaction spikes but fails to translate volume into scalable cash.
* **The Root Cause:** Marketing's hyper-promotional strategies ($40\% - 50\%$ off) attract opportunistic, low-intent buyers. This pricing strategy triggers an unsustainable **$28\%$ customer return rate** (compared to a baseline $5\%$ on full-price lines), completely erasing initial revenue gains via processing and reverse logistics overhead.

### 3. Flagship Customer Acquisition Burn
* **The Diagnostic:** The *Quantum X Smartphone* represents the premier top-line channel with **$2.3 Million in gross revenue**, yet generates a compressed net cash flow yield.
* **The Root Cause:** Intensive industry market saturation forces a heavy reliance on digital acquisition budgets (`Marketing_Spend_Allocated`), meaning the business is essentially "buying" sales volume through unoptimized, premium customer acquisition cost (CAC) buffers.

---

##  Prescriptive Recommendations

###  Short-Term (0-30 Days)
* **Enforce System Freight Blocks:** Program a hard operational rule into the ERP framework preventing heavy appliances ($>30\text{ lbs}$) from using Express Air channels unless premium shipping charges are passed entirely to the customer at checkout.
* **Ad Budget Realignment:** Pivot $15\%$ of paid ad budgets away from the saturated *Quantum X Smartphone* into the healthy margin *Aura Soundbar 360* ($38\%$ margin) to maximize global Return on Ad Spend (ROAS).

###  Medium-Term (1-6 Months)
* **Establish Gross Margin Floor Caps:** Institute a strict $20\%$ discount ceiling on the apparel line to wash out high-risk impulse buyers and bring down the toxic $28\%$ return deficit.
* **Restrictive Return Architecture:** Introduce automated restocking fees on deep-sale items to alter purchasing friction and protect margin lines.

###  Long-Term (6+ Months)
* **Pre-Position Inventory via Regional Hubs:** Decentralize appliance fulfillment. Leverage rolling 30-day regional demand forecasts to position heavy appliances via standard, low-cost ground freight near high-density zip codes *before* purchase orders happen, eliminating stockout panic air shipping entirely.

---

