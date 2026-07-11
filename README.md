# Paid-Media-Performance-Dashboard
An end to end data analytics project using SQL and Excel to optimizze multi-channel advertising performance 
## Key Performance Indicators (KPIs) Analyzed
* **CPC (Cost Per Click):** Ad Spend / Clicks
* **CAC (Customer Acquisition Cost):** Ad Spend / Total Conversions
* **ROAS (Return on Ad Spend):** Total Revenue / Ad Spend

## The Tech Stack & Process
1. **Excel:** Generated mock transactional and daily campaign spend datasets utilizing `RANDBETWEEN` and advanced conditional data arrays.
2. **SQL (PostgreSQL):** Designed relational schemas and engineered an aggregated subquery join to eliminate many-to-many fan-out data duplication across shared date and channel keys.
3. **Data Visualization:** Imported query summary outputs back into Excel to build a dual-axis combo chart analyzing volume (Spend) vs. efficiency (Revenue).

## The SQL Script
```sql
SELECT 
    s.channel,
    SUM(s.spend) AS total_spend,
    SUM(s.clicks) AS total_clicks,
    COALESCE(c.total_conversions, 0) AS total_conversions,
    COALESCE(c.total_revenue, 0) AS total_revenue,
    ROUND(SUM(s.spend) / SUM(s.clicks), 2) AS cpc,
    ROUND(SUM(s.spend) / COALESCE(c.total_conversions, 1), 2) AS cac,
    ROUND(COALESCE(c.total_revenue, 0) / SUM(s.spend), 2) AS roas
FROM ad_spend s
LEFT JOIN (
    SELECT 
        date, 
        channel_attribution, 
        COUNT(purchase_id) AS total_conversions, 
        SUM(revenue) AS total_revenue
    FROM conversions
    GROUP BY date, channel_attribution
) c ON s.date = c.date AND s.channel = c.channel_attribution
GROUP BY s.channel, c.total_conversions, c.total_revenue;

Based on the dashboard results generated from the data analysis, the following structural adjustments are recommended to optimize performance:

Google Search Optimization: Google Search demonstrated steady conversion capability, yielding a solid revenue stream relative to volume. Recommend protecting this base spend to scale high-intent consumer keyword targets.

Meta Ads Creative Strategy: Meta Ads captured the highest volume of total conversions ($205.00 in revenue), proving it as a primary acquisition channel. Recommendation is to reallocate top-of-funnel budget here to maximize scale.

TikTok Ads Re-evaluation: TikTok Ads achieved a competitive Cost Per Click ($0.82) and drove high initial traffic, but completely failed to convert any downstream revenue. Recommendation is to halt broad-awareness spending on TikTok and pivot budget toward conversion-focused retargeting, or reallocate 15% of this budget directly into Meta Ads.
