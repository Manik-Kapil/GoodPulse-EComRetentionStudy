<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 120" width="400" height="120">
  <defs>
    <linearGradient id="pulseGrad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color:#5b9bd5;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#a8c8e8;stop-opacity:1" />
    </linearGradient>
    <linearGradient id="bgGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#1e2130;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#262b3d;stop-opacity:1" />
    </linearGradient>
    <filter id="glow">
      <feGaussianBlur stdDeviation="2" result="coloredBlur"/>
      <feMerge>
        <feMergeNode in="coloredBlur"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <!-- Background pill -->
  <rect x="0" y="0" width="400" height="120" rx="16" ry="16" fill="url(#bgGrad)"/>

  <!-- Subtle border -->
  <rect x="1" y="1" width="398" height="118" rx="15" ry="15" fill="none" stroke="#5b9bd5" stroke-width="0.8" stroke-opacity="0.3"/>

  <!-- Icon: pulse / heartbeat line inside a circle -->
  <!-- Circle ring -->
  <circle cx="62" cy="60" r="34" fill="none" stroke="#5b9bd5" stroke-width="2" stroke-opacity="0.25"/>
  <circle cx="62" cy="60" r="26" fill="#5b9bd5" fill-opacity="0.08"/>

  <!-- Pulse line — flat, spike, flat -->
  <polyline
    points="30,60 44,60 50,42 56,78 62,54 68,66 74,60 94,60"
    fill="none"
    stroke="url(#pulseGrad)"
    stroke-width="2.8"
    stroke-linecap="round"
    stroke-linejoin="round"
    filter="url(#glow)"
  />

  <!-- Dot at peak of pulse -->
  <circle cx="56" cy="78" r="3" fill="#a8c8e8" filter="url(#glow)"/>
  <circle cx="62" cy="54" r="2.2" fill="#5b9bd5" filter="url(#glow)"/>

  <!-- Wordmark: GoodPulse on one line -->
  <text
    x="112"
    y="58"
    font-family="'Segoe UI', 'Helvetica Neue', Arial, sans-serif"
    font-size="34"
    font-weight="700"
    letter-spacing="-0.5"
    fill="#a8c8e8"
  >Good<tspan fill="url(#pulseGrad)" font-weight="300" letter-spacing="1">Pulse</tspan></text>

  <!-- Thin divider line -->
  <line x1="112" y1="68" x2="385" y2="68" stroke="#5b9bd5" stroke-width="0.6" stroke-opacity="0.35"/>

  <!-- Tagline -->
  <text
    x="113"
    y="88"
    font-family="'Segoe UI', 'Helvetica Neue', Arial, sans-serif"
    font-size="11"
    font-weight="400"
    letter-spacing="2.2"
    fill="#5b9bd5"
    fill-opacity="0.65"
  >E-Commerce Analytics made easy.</text>

</svg>
# GoodPulse — Executive Performance Report

> **Data Stack:** Amazon S3 · Databricks (Medallion Architecture) · Looker
> **Dataset:** Olist E-Commerce + Olist Marketing (Kaggle)
> **Classification:** Internal Use Only

---

## Table of Contents

1. [Organization Overview & Executive Summary](#1-organization-overview-and-summary)
2. [OKR Framework](#2-okr-framework)
3. [Executive Overview — Northstar Metrics](#3-executive-overview--northstar-metrics)
   - 3.1 [Customer Distribution by Frequency](#31-customer-distribution-by-frequency)
   - 3.2 [Revenue per Customer — Monetization Imbalance](#32-revenue-per-customer--monetization-imbalance)
   - 3.3 [Recency × Value Pivot Tables](#33-recency--value-pivot-tables)
   - 3.4 [Monthly Revenue Trend](#34-monthly-revenue-trend)
   - 3.5 [Impact of Delivery Days on Review Score](#35-impact-of-delivery-days-on-review-score)
4. [Customer Segmentation](#4-customer-segmentation)
   - 4.1 [Segment Distribution](#41-segment-distribution)
   - 4.2 [Revenue by Segment & Acquisition Channel](#42-revenue-by-segment--acquisition-channel)
5. [Cohort Retention & Acquisition Analysis](#5-cohort-retention--acquisition-analysis)
   - 5.1 [Cohort Retention Heatmap](#51-cohort-retention-heatmap)
   - 5.2 [Revenue by Acquisition Channel](#52-revenue-by-acquisition-channel)
   - 5.3 [Lifecycle Revenue Distribution](#53-lifecycle-revenue-distribution)
6. [KPI Deep-Dive](#6-kpi-deep-dive)
   - [KPI 1 — Month 1 Retention](#kpi-1--month-1-retention)
   - [KPI 2 — Funnel Analysis: Repeat Purchase Rate](#kpi-2--funnel-analysis-repeat-purchase-rate)
   - [KPI 3 — Revenue from Active Customers](#kpi-3--revenue-from-active-customers)
   - [KPI 4 — Revenue Concentration by Value Segment](#kpi-4--revenue-concentration-by-value-segment)
7. [Strategic Recommendations](#7-strategic-recommendations)
8. [Metric Definitions](#8-metric-definitions)

---

## 1. Organization Overview and Summary 

GoodPulse is a data-driven analytical simulation of a rapidly scaling e-commerce marketplace modeled using the Olist Brazilian e-commerce dataset enriched with marketing data. Operating in a competitive digital commerce landscape, GoodPulse serves a diverse customer base across multiple regions and product categories. 

The company operates across multiple acquisition channels and product categories. While revenue volume appears strong, preliminary analysis reveals structural challenges in customer retention and lifecycle sustainability. 

The data pipeline follows a **Medallion Architecture** in Databricks, progressively refining raw transactional and marketing data through three layers before surfacing insights in Looker.

**Executive Summary**
Core Structural Issue: Extremely Low Repeat Rate

The most critical finding is a repeat purchase rate of approximately 0.03%.

Out of 93,358 customers:
- ~90K are one-time buyers
- Fewer than 3% return for additional purchases
- Only 19 customers qualify as power users

This indicates that GoodPulse operates almost entirely on first-time transactional revenue.

**Revenue Sustainability Risk**
Despite generating $15.5M in total revenue, the majority of revenue is sourced from one-time buyers.
Revenue concentration analysis shows:

- High Value customers generate the majority of total revenue
- However, most high-value revenue is classified as Dormant
- High Value Dormant Revenue: $4.9M
- High Value Recent Revenue: $1.7M

This indicates significant leakage of previously valuable customers.

**Cohort Retention Analysis**
Cohort analysis reveals immediate retention decay:
- Month 0 retention = 100%
- Month 1 retention drops sharply toward zero
- Retention remains negligible beyond Month 1

This confirms the repeat rate issue at a structural level. The business lacks customer lifecycle stickiness.

**Lifecycle Distribution**
Revenue distribution by lifecycle segment:
*Dormant > Active > At Risk*
This means more revenue is sitting in inactive customers than in actively engaged ones. Without retention improvement, revenue will depend entirely on constant acquisition.

**Key Risks Identified**

1. Revenue is acquisition-dependent
2. Customer lifetime value is structurally constrained
3. High-value customers are not retained
4. Cohorts fail to mature beyond initial purchase
5. Long-term growth is unstable without retention improvement


**Pipeline Layers:**

- **Bronze** — Raw ingestion from Amazon S3. Includes Olist orders, customers, reviews, geolocations, and marketing datasets. Profiling covers schema validation and null analysis.
- **Silver** — Data cleaning, deduplication, column standardisation, and merging of order, customer, and review records into a `unified_user_lifecycle` base table.
- **Gold** — Business logic applied. Produces three analytical tables:
  - `goodpulse.gold.customer_segmentation` — RFM-based behavioural segmentation
  - `goodpulse.gold.cohort_analysis` — Quarterly cohort retention tracking from Q3 2016 to Q3 2018
  - `goodpulse.gold.customer_unified_lifecycle` — Full 360° customer view combining recency, value, and frequency signals
- **Looker** — Semantic layer over Gold tables. The executive dashboard is the primary output.

The reporting period spans **Q3 2016 through Q2 2018**, covering **93,358 unique customers**, **$15.49M in total revenue**, and **96,500+ orders**.

---

## 2. OKR Framework

The analysis is structured around the **OKR → KPI → Metric pyramid**. Every data point in this report connects upward to a strategic objective.

> - **Metrics** are any quantitative value we can track. *(e.g. "Total orders were 96.5K")*
> - **KPIs** are the metrics that genuinely reflect performance and are influenceable by the team. *(e.g. "Repeat rate is 3%")*
> - **OKRs** are the change we want to execute — the goals KPIs are in service of.

**Objectives for this reporting period:**

| # | Objective | Primary KPI |
|---|-----------|-------------|
| O1 | Increase customer lifetime value and revenue per customer | Revenue Concentration by Value Segment |
| O2 | Improve customer retention and reduce churn | Month 1 Retention Rate |
| O3 | Convert first-time buyers into repeat customers | Repeat Purchase Funnel Rate |
| O4 | Grow the active revenue base faster than churn | Revenue from Active Customers |

---

## 3. Executive Overview — Northstar Metrics

![North Star Metrics](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/b666bc9562480c75cdc9511c1d43c1ce354f4348/img_northstar_metrics.png)

| Metric | Value |
|--------|-------|
| Total Customers | 93,358 |
| Total Revenue | $15.49M |
| Average Order Value | $160.32 |
| Total Orders | 96.5K |
| Repeat Rate | 3.00% |

At first glance, the platform is generating meaningful scale. But two numbers demand immediate attention: a **3% repeat rate** and only **19 Power Users** out of 93,358 customers. These signal a platform that is very good at acquiring customers and very poor at retaining them — a structural problem, not a cyclical one.

---

### 3.1 Customer Distribution by Frequency

![Frequency Distribution](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/78bd3a1d2078d8dcf2c7e6145ee10163f2e4fad7/img_freq_distribution.png)

The frequency breakdown confirms how skewed the customer base is:

| Segment | Count | Revenue Share |
|---------|-------|---------------|
| One-time | 90,600 | 94.03% |
| Regular (2+ purchases) | 2,800 | 5.87% |
| Power User | 19 | 0.10% |

97% of all customers have purchased exactly once. While one-time buyers dominate revenue in absolute terms simply because of their volume, the **per-customer revenue gap** tells the real story — Regular customers spend nearly 3x more than one-time buyers, and Power Users spend nearly 11x more. The platform is leaving substantial revenue on the table by not converting even a small fraction of one-time buyers into returning customers.

---

### 3.2 Revenue per Customer — Monetization Imbalance

![Monetization Distribution](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/067de9a44dd9ffb329e4c326f519b64370961850/monetization_imbalance.png)

The bar chart surfaces a stark monetization gap across value tiers:
- **High Value:** $436.30 per customer
- **Mid Value:** $113.90 per customer
- **Low Value:** $39.80 per customer

High Value customers generate **10.9x more revenue per head** than Low Value customers. This isn't unusual in e-commerce, but the degree of concentration means that any churn among High Value customers has an outsized negative effect on total revenue. Protecting and growing this tier is the highest-leverage action available to the business.

---

### 3.3 Recency × Value Pivot Tables

The two pivot tables cross-reference **Recency** (Dormant / Aging / Recent) against **Value Segment**, revealing where revenue and customers are actually sitting.

**Revenue by Recency Bucket:**

| Value Segment | Dormant | Aging | Recent |
|---------------|---------|-------|--------|
| High Value | $4.9M | $1.8M | $1.7M |
| Mid Value | $3.7M | $1.4M | $1.3M |
| Low Value | $447K | $154K | $147K |

<!-- PLACEHOLDER: img_pivot_customers.jpg -->

**Customer Count by Recency Bucket:**

| Value Segment | Dormant | Aging | Recent |
|---------------|---------|-------|--------|
| Mid Value | 32.6K | 11.7K | 10.9K |
| High Value | 11.3K | 4.2K | 3.9K |
| Low Value | 11.2K | 3.8K | 3.7K |

**Key insight — the Dormant High Value problem:** $4.9M in revenue belongs to Dormant High Value customers. This is the single largest revenue block in the entire dataset, and it sits in lapsed customers. These 11,300 people have already demonstrated willingness to make high-value purchases — they simply haven't returned. A targeted win-back campaign for this cohort is the highest-ROI opportunity on the board.

A second structural signal: across all value segments, Dormant customers **outnumber** Recent customers roughly 3:1. The platform is accumulating lapsed customers faster than it is developing active ones.

---

### 3.4 Monthly Revenue Trend

![Monthly Trend](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/d0318120af86bb9c8fbc2b6039dc65195b37f301/monthly%20revenue%20trend.png)

Revenue grew steadily from the platform's launch in Q3 2016, accelerating through 2017 and peaking around Q4 2017 before softening in early 2018. The **average monthly revenue of $1.93M** (shown as dashed line) is skewed by the peak period — roughly half of all months fall below this average, indicating the platform has not normalised at its peak run rate.

The Q4 2017 peak is consistent with seasonal shopping trends seen across e-commerce broadly. The softening in Q1–Q2 2018 warrants monitoring: it could reflect seasonal normalisation, or it could be the beginning of organic growth deceleration as the addressable customer base matures.

---

### 3.5 Impact of Delivery Days on Review Score

![Rating](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/be7dafdb8aefbfa63e3f375b54b653c14bf890c3/rating.png)

The scatter plot reveals a clear inverse relationship between delivery speed and customer satisfaction. As delivery days increase, average review scores decline. The platform's **average review score of 3.29 / 5** suggests that delivery performance is meaningfully dragging satisfaction below what product quality alone would warrant.

The practical implication: improving last-mile logistics is not just an operational goal, it is a **retention lever**. Customers who receive slow deliveries are less likely to return, and their lower review scores reduce organic trust signals for new customers.

---

## 4. Customer Segmentation

The `goodpulse.gold.customer_segmentation` table applies RFM logic combined with lifecycle flags to classify every customer into an actionable segment. These segments directly inform CRM targeting, marketing spend allocation, and retention strategy.

<!-- PLACEHOLDER: img_seg_dist_revenue.jpg -->

---

### 4.1 Segment Distribution

The ten largest segments by customer count:

![Segment Distribution](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/2b0a8fb7e4632029391ba39e1f52d865bbae4f96/Segment%20Distribution.png)

The landscape is overwhelmingly one-time buyers at various lifecycle stages. The **6,300 Dormant High Value one-time buyers** are a critical win-back target — they have demonstrated purchase intent and high spend capacity, they just haven't come back. The **593 Active High Value Regular customers** are the platform's most valuable recurring buyers and should be treated as a VIP tier.

---

### 4.2 Revenue by Segment & Acquisition Channel

The revenue distribution, broken out by acquisition channel, reveals both the value hierarchy and channel efficiency:

![Segment Revenue Distribution](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/2b0a8fb7e4632029391ba39e1f52d865bbae4f96/Seg%20Rev%20Dist.png)

**Organic search** dominates acquisition across all high-value segments. This is a positive signal: customers are finding the platform through genuine discovery, not paid dependency. Paid search and social channels are present but contribute a small fraction of revenue — before investing further in these channels, attribution analysis should validate whether they are attracting lower-quality customers or simply underleveraged.

> ⚠️ **Urgent flag:** $2.2M sitting in *At Risk \| High Value \| One-time* is the highest-urgency retention priority. These customers have a purchase history; targeted re-engagement can convert them back to Active. The cost of retention is always lower than the cost of new acquisition.

---

## 5. Cohort Retention & Acquisition Analysis

---

### 5.1 Cohort Retention Heatmap

![HeatMap](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/f3fa0872540889edfba54f794b54a7d2d63d6d5c/Cohort%20HeatMap.png)

The heatmap, derived from `goodpulse.gold.cohort_retention`, tracks what percentage of customers from each quarterly acquisition cohort return to purchase in subsequent periods. The data spans Q1 2017 through Q3 2018 and tells a consistent story:

**Reading the heatmap correctly:**
- `cohort_index 0` = the acquisition period itself (always 100%)
- `cohort_index 1` = the first period *after* acquisition
- The values shown are retention rates as percentages

**What the data shows:**

| Cohort | Index 1 Retention | Index 2 Retention | Index 3–6 Range |
|--------|-------------------|-------------------|-----------------|
| Q3 2016 | — | — | — |
| Q4 2016 | 100* | 0.8 | 0.4 |
| Q1 2017 | 0.3 | 0.3 | 0.2–0.4 |
| Q2 2017 | 0.5 | 0.4 | 0.3–0.4 |
| Q3 2017 | 0.6 | 0.4 | 0.2–0.4 |
| Q4 2017 | 0.5 | 0.3 | 0.2 |
| Q1 2018 | 0.4 | 0.4 | 0.2–0.3 |
| Q2 2018 | 0.5 | 0.3 | 0.1–0.2 |
| Q3 2018 | 0.5 | — | — |

*Q4 2016 index 1 = 100 is an anomaly, likely due to the very small cohort size for that early period.*

**Key findings:**

- Across all mature cohorts, Period 1 retention sits between **0.3% and 0.6%** — meaning fewer than 1 in 150 customers makes a second purchase in the following period
- Retention does not meaningfully improve across cohort generations — the Q3 2017 cohort (0.6% at index 1) performs similarly to Q1 2018 (0.4%) — there is no evidence of structural improvement over time
- The retention problem is concentrated at the **point of second purchase conversion**, not at later stages. Customers who do make a second purchase tend to show some, if low, continued activity at indices 3–6
- This pattern is consistent with the **3% platform-wide repeat rate** — the cohort view confirms it is structural

The absence of improvement across cohort generations is the **critical signal** here. It rules out "early platform growing pains" as an explanation,   ` the retention problem persists regardless of when customers were acquired.

---

### 5.2 Revenue by Acquisition Channel

![Acquisition Distribution](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/16eeac8c8d19a8600ec52930d8373e34ac939a30/GoodPulse%20-%20Executive%20Dashboard_Untitled%20Page_Bar%20chart%20(1).png)


`online_medium` dominates volume entirely, 83K customers and $13.8M represents **89% of total customers and 89% of total revenue**. The platform is near-entirely dependent on a single acquisition channel.

The **revenue-per-customer figures are remarkably similar across all channels** (~$159–$176), which means there is no obvious "premium channel" delivering higher-quality customers. The concentration risk is therefore a **diversification problem, not a quality problem**, the other channels could be scaled up without sacrificing customer quality.

---

### 5.3 Lifecycle Revenue Distribution

![Acquisition Revenue Distribution](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/7c45d73cdd3c61aed41edc3d5f65d210c7243f58/Acq%20Channel%20Rev.png)

| Lifecycle State | Revenue | Share |
|-----------------|---------|-------|
| Dormant | $5.4M | 35.1% |
| Active | $5.3M | 34.0% |
| At Risk | $4.8M | 30.8% |

The near-equal split between **Dormant ($5.4M)** and **Active ($5.3M)** is the single most important finding in the lifecycle analysis. Almost as much historical revenue value sits in lapsed customers as in currently active ones. When you add the At Risk pool ($4.8M), **over 65% of all revenue-generating customers are either already gone or trending that way.**

A healthy platform would have Active customers representing 50–60%+ of total lifecycle revenue. At 34%, GoodPulse's engaged base is not growing fast enough to offset natural churn. This is the defining strategic challenge.

---

## 6. KPI Deep-Dive

The four KPIs below were selected because they are: (1) measurable from the Gold layer tables, (2) directly influenceable by operational decisions, and (3) tied to the OKRs established in Section 2. Each connects upward to a specific objective.

---

### KPI 1 — Month 1 Retention

![Retention](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/aa1dc1cea8414746fc6911e0555953356b6deadc/KPI%201.png)

**Definition:** The percentage of customers from a given acquisition cohort who make a second purchase within the first period after their initial order.

**Current State:** The retention curve drops from 100% to near-zero within a single cohort index. Across all cohorts, Period 1 retention averages between **0.3% and 0.6%**. The curve shape is steep and immediate — there is no "slow decline"; customers simply do not come back.

**Why this is a KPI, not just a metric:** Month 1 retention is the most leverageable point in the entire customer lifecycle. Research consistently shows that customers who purchase twice are significantly more likely to purchase a third, fourth, and fifth time. The second purchase is the gateway to loyalty — and the platform is almost universally failing to open it.

**Root cause:** The steepness of the drop from index 0 to index 1 — with no gradual decline — strongly suggests an **absence of structured post-purchase engagement**. There is no evidence of a nurture sequence, loyalty incentive, or re-engagement mechanism triggering between a customer's first and potential second purchase.

**Recommendation:**
- Implement a 7/14/30-day post-purchase email sequence with personalised product recommendations based on first purchase category
- The addressable pool is 90,600 one-time buyers — even a 1% conversion improvement translates to ~906 additional repeat customers
- At the Regular customer AOV of $113.90, that's approximately **$103K in incremental revenue per percentage point improvement**

**OKR Link → O2 (Improve retention) and O3 (Convert first-time buyers)**

---

### KPI 2 — Funnel Analysis: Repeat Purchase Rate

![Funnel](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/aa1dc1cea8414746fc6911e0555953356b6deadc/KPI%202.png)

**Definition:** The percentage of customers who progress from One-time (single purchase) to Regular (2+ purchases) to Power User status.

**Current State:**
- One-time → Regular conversion: **3.07%**
- Regular → Power User conversion: **0.68%** of Regulars (0.02% of total base)
- 19 total Power Users from a base of 93,358 customers

**Why this is a KPI, not just a metric:** The repeat purchase funnel is the most direct expression of the platform's ability to create loyal customers. At 3%, GoodPulse is operating well below what healthy e-commerce platforms achieve — industry benchmarks for repeat rates typically sit in the 20–40% range for established platforms.

**The funnel collapse in context:** The 97% drop-off from One-time to no-return is not a product quality problem — organic search dominates acquisition, meaning customers are finding and trusting the platform enough to purchase. It is a **post-purchase experience problem**. The platform makes the sale but does not make the relationship.

The Power User funnel is almost non-existent: 0.68% of Regulars become Power Users, producing just 19 people who purchase frequently. This means there is no natural "graduation" path from casual to loyal — customers either buy once and leave, or occasionally return but rarely deepen their relationship with the platform.

**Recommendation:**
- Introduce a points-based loyalty programme tied to repeat purchases to create explicit incentive for second and third orders
- Moving from 3.07% to 5% repeat rate adds ~1,870 Regular customers; at $113.90 AOV that's **~$213K in incremental revenue**
- Design a clear upgrade pathway from Regular to Power User status (spend milestones, exclusive perks, early access)

**OKR Link → O3 (Convert first-time buyers) and O1 (Increase LTV)**

---

### KPI 3 — Revenue from Active Customers

![Revenue Distribution](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/aa1dc1cea8414746fc6911e0555953356b6deadc/KPI%203.png)

**Definition:** The share of total platform revenue generated by customers currently classified as Active (recently purchased, showing engagement signals).

**Current State:**
- **Active: 34.03%** ($5.3M)
- **Dormant: 35.13%** ($5.4M)
- **At Risk: 30.84%** ($4.8M)

**Why this is a KPI, not just a metric:** Active revenue share is a leading indicator of platform health. It tells you whether the engaged customer base is growing or shrinking relative to total historical value. When Dormant revenue exceeds Active revenue — as it does here — the platform is effectively losing ground: more value is sitting in lapsed customers than in currently engaged ones.

**The 34% Active share problem:** A healthy platform should have Active customers representing 50–60% or more of lifecycle revenue. At 34%, with Dormant marginally ahead at 35%, GoodPulse's active base is not growing fast enough to offset churn. The platform is running on the back of historical purchasing rather than current momentum.

**The At Risk $4.8M urgency:** The At Risk segment (30.84%) represents revenue that is in transition — these customers are declining in engagement and, without intervention, will migrate to Dormant. Automated early-warning triggers tied to recency signals can identify At Risk customers before they fully lapse, enabling proactive re-engagement.

**Recommendation:**
- Set a 12-month target to shift Active revenue share from 34% to 45%
- Achieve through a combination of: (a) improved Month 1 retention reducing new-customer churn, and (b) win-back campaigns converting Dormant High Value customers back to Active
- Deploy automated lifecycle monitoring to flag At Risk customers at the point of declining engagement, not after they have already lapsed

**OKR Link → O4 (Grow active revenue base)**

---

### KPI 4 — Revenue Concentration by Value Segment

![Revenue Concentration](https://github.com/Manik-Kapil/GoodPulse-EComRetentionFiasco/blob/aa1dc1cea8414746fc6911e0555953356b6deadc/KPI%204.png)

**Definition:** The percentage of total revenue contributed by each customer value segment (High, Mid, Low), measuring how concentrated the revenue base is across tiers.

**Current State:**
- **High Value: 54.6%**
- **Mid Value: 40.6%**
- **Low Value: 4.8%**

**Why this is a KPI, not just a metric:** Revenue concentration is a risk and opportunity metric simultaneously. High concentration in the top tier validates strong monetisation from the best customers — but it also means the revenue base is fragile. If High Value customers churn in volume, there is limited Mid or Low Value revenue to absorb the impact.

**The dual risk in High Value concentration:** 54.6% of all revenue flows from a relatively small pool of High Value customers. The Recency pivot from Section 3.3 reveals that **11,300 of these customers are already Dormant** — holding $4.9M in lapsed value. The Active High Value pool ($1.7M Recent) is smaller than the Dormant pool ($4.9M). This means the top revenue tier is net-declining: more High Value customers are going dormant than are being developed.

**The Low Value floor at 4.8%:** The near-absence of Low Value revenue contribution is not inherently a problem — it reflects the natural distribution of an RFM model. However, it does indicate that the upgrade pathway from Low to Mid to High is not functioning. Customers are not graduating up the value tiers; they are either arriving as High/Mid buyers or staying permanently at Low.

**Recommendation:**
- Build a tiered retention programme specifically for High Value customers: priority support, early access to new products, exclusive offers. Retaining 10% of the $4.9M Dormant High Value pool = **~$490K in recovered revenue**
- Design spend-threshold upgrade incentives to move Mid Value customers toward High Value behaviour
- Monitor the High Value Active vs. Dormant ratio monthly — if Dormant continues to exceed Active, the top revenue tier is in structural decline

**OKR Link → O1 (Increase LTV and revenue per customer)**

---

## 7. Strategic Recommendations

| Priority | Action | Owner | Timeline | Expected Impact |
|----------|--------|-------|----------|-----------------|
| 🔴 Critical | Post-purchase email nurture (Day 7/14/30 sequence) | CRM / Marketing | Month 1–2 | +1–2% Month 1 retention |
| 🔴 Critical | Win-back campaign — Dormant High Value (11.3K customers) | CRM | Month 1–3 | ~$490K potential recovered revenue |
| 🔴 Critical | At Risk automated monitoring & early-warning triggers | Analytics / CRM | Month 1 | Protect $4.8M at-risk revenue pool |
| 🟡 High | Loyalty programme launch (points-based, spend milestones) | Product / Marketing | Month 2–4 | +3% → 5% repeat rate (~$213K incremental) |
| 🟡 High | Delivery SLA improvements — target sub-20 day standard | Operations / Logistics | Month 3–6 | Review score improvement → retention lift |
| 🟢 Medium | Acquisition channel diversification testing | Marketing / Growth | Month 2–5 | Reduce 89% single-channel dependency |
| 🟢 Medium | High Value tier retention programme (VIP perks, priority access) | CRM / Product | Month 3–6 | Protect 54.6% revenue concentration |

**12-Month OKR Targets (suggested):**

- Repeat rate: **3% → 6%**
- Month 1 retention: **0.5% → 2%**
- Active revenue share: **34% → 45%**
- Average review score: **3.29 → 4.0**

---

## 8. Metric Definitions

| Metric | Technical Formula | Plain English | Relevant Teams |
|--------|-------------------|---------------|----------------|
| AOV (Average Order Value) | Total Revenue ÷ Total Orders | Average spend per transaction | Finance, Marketing |
| Repeat Rate | Customers with 2+ orders ÷ Total Customers | % of customers who return to buy again | CRM, Marketing |
| Cohort Retention Rate | Active customers at index N ÷ Cohort size at index 0 | % of an acquisition cohort still purchasing in a given period | Product, CRM |
| Lifecycle State | Recency-based classification (Active / At Risk / Dormant) | How recently a customer last purchased | CRM, Analytics |
| Revenue Concentration | Segment revenue ÷ Total revenue | Share of revenue held by each customer tier | Finance, Strategy |
| Revenue per Customer | Total revenue ÷ Unique customers in segment | Expected revenue from one customer in a given segment | Finance, Marketing |
| Delivery Days | Avg calendar days from order placement to delivery | How fast customers receive their orders | Operations, Logistics |
| Review Score | Avg rating (1–5) on delivered orders | Customer satisfaction with their purchase and delivery experience | Operations, Product |

---

*End of Report — GoodPulse Executive Data Analytics Performance Report*
*Data Stack: Amazon S3 · Databricks · Looker | Dataset: Olist (Kaggle)*
