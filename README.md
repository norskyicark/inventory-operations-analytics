# Inventory Operations Analytics
### Eth Tech Business Analyst Project — Public Simulated-Data Version

> This repository presents a public, simulated-data version of an inventory operations analytics project completed under my Business Analyst work with Eth Tech. The project demonstrates SQL data modeling, KPI definition, stockout risk monitoring, replenishment prioritization, data validation, and Tableau reporting for an e-commerce operations scenario.

---

## Business Context

**ApexGear Commerce (AGC):** a simulated multi-channel e-commerce retailer selling electronic accessories across marketplace and direct-to-consumer channels.

**Pain points:**
- Frequent stockouts during demand spikes, creating estimated revenue at risk during out-of-stock periods 
- Overstock and excess coverage, tying up working capital and creating operational drags
- Fragmented visibility across inventory, sales, purchase orders, and lead times, making replenishment priorities unclear  

**My role:** Business Analyst at Eth Tech, responsible for building SQL-based operational marts, defining KPI logic, validating simulated data quality, and preparing reporting views to support purchasing and inventory review.

---

## What I Built

| Deliverable | Description |
|-------------|-------------|
| **Unified SQL data model** | Star schema: product, fulfillment center, calendar, vendor lead time, daily inventory, daily sales, purchase orders |
| **Daily ASIN–FC mart logic** | Rolling demand, inbound/outbound net flow, coverage days, in-stock status classification |
| **Stock risk monitoring** | Stockout days, in-stock rate, shortage vs overstock flags, stockout risk tiers |
| **Revenue-at-risk estimation** | Estimated gross/net revenue loss on stockout days using instock-only unit economics |
| **Replenishment priority framework** | ROP-style logic using avg / P95 lead time, coverage with and without inbound |
| **Tableau dashboards** | Executive overview + SKU/FC detail views for weekly ops review |
| **Data validation** | Consistency checks on sellable units, stockout vs sales, date coverage |

---

## Tech Stack

- **SQL / PostgreSQL** — schema, seed logic, analytical queries, window functions  
- **Tableau** — operational dashboards  
- **Excel** — stakeholder summary views (optional)  
- **GitHub** — versioned SQL and documentation  

---

## Data Model Overview

```
dim_asin              — SKU / product attributes (category, sub-category, launch date)
dim_fc                — fulfillment centers (3 regions)
dim_vendor_lead_time  — average and P95 lead time by ASIN
dim_calendar          — date spine with weekend / peak-event flags

fact_daily_inventory  — daily snapshot: on-hand, reserved, inbound, sellable units
fact_daily_sales      — daily units sold, gross/net revenue
fact_po               — purchase orders: ordered/received dates and quantities
```

**Grain:** one row per `snapshot_date × asin × fc_id` for inventory; same for daily sales.

**Scope (simulated):** ~50 ASINs × 3 FCs × ~90 days (Jun–Aug 2025).

---

## Key Metrics & Logic

| Metric | Purpose |
|--------|---------|
| **In-Stock Rate** | Share of days with sellable inventory available |
| **Stockout Days** | Count of days where sellable units = 0 |
| **Coverage Days** | Sellable units ÷ avg daily demand (with/without inbound adjustment) |
| **Lead Time P95** | Conservative replenishment horizon vs average lead time |
| **Stockout Risk Tier** | Priority flag when coverage falls below lead-time threshold |
| **Revenue Captured %** | Actual revenue vs estimated potential revenue (instock days only) |
| **Overstock Flag** | Excess coverage relative to demand/inbound profile |

---

## Repository Structure

```
├── README.md
├── sql/
│   ├── 01_schema_and_seed.sql      ← from console.sql (schema + mock data generation)
│   ├── 02_validation.sql           ← data quality checks
│   ├── 03_kpi_stockout.sql         ← stockout / in-stock metrics
│   ├── 04_revenue_impact.sql       ← revenue-at-risk logic
│   ├── 05_replenishment.sql        ← ROP / priority scoring
│   └── exploration/
│       └── Draft.sql               ← iterative metric design notes (not production layer)
├── data/
│   └── processed/                  ← CSV exports for Tableau (optional)
├── tableau/
│   └── dashboards/                 ← .twb or screenshots
└── screenshots/
    ├── executive_overview.png
    └── sku_detail.png
```

> **Note:** On first upload, `console.sql` can live as a single file; split into numbered files as you refactor.

---

## Sample Analytical Questions Answered

1. Which ASIN–FC pairs drive repeated stockouts during peak periods?  
2. Where is estimated revenue at risk due to out-of-stock days?  
3. Which SKUs need replenishment review based on coverage vs lead time P95?  
4. Where does inventory look excess relative to demand and inbound flow?  
5. How do weekend / peak-event periods differ from normal demand patterns?  

---

## Dashboards

| View | Audience | Contents |
|------|----------|----------|
| **Executive Overview** | Ops leadership | KPI cards, stockout trends, top ASINs by revenue/units, shortage vs overstock |
| **SKU / FC Detail** | Analysts / purchasing | Replenishment priority table, filters by FC, category, risk tier |

Screenshots: see `/screenshots/`  
Tableau workbook: see `/tableau/`

---

## How to Run (Local)

**Requirements:** PostgreSQL 14+

```bash
# 1. Create database
createdb apexgear_analytics

# 2. Run schema and seed (after splitting or using console.sql)
psql -d apexgear_analytics -f sql/01_schema_and_seed.sql

# 3. Run analytical queries
psql -d apexgear_analytics -f sql/03_kpi_stockout.sql
```

---

## Limitations & Disclosures

- This repository is a public demonstration version of an Eth Tech Business Analyst project and uses simulated data for portfolio and interview discussion.
- The dataset does not contain proprietary production data, confidential client records, or verified financial outcomes.
- Product names, ASINs, categories, demand patterns, and inventory behavior are simulated for an e-commerce operations scenario and may not match any public storefront listing.
- Business impact figures, including revenue-at-risk or trapped inventory assumptions, should be interpreted as modeled estimates or case assumptions rather than verified production ROI.
- Exploratory SQL files may reflect iterative metric design and should be read as analysis development work rather than a finalized production pipeline.


---

## Contact

**Shenghui Hu**  
shenghuihu08@gmail.com | [LinkedIn](your-link) | GitHub  
