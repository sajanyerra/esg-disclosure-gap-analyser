# ESG Risk Analyser — US Energy Sector

![ESG Risk Analysis](assets/dashboard_preview.png)

**An end-to-end data pipeline that collects, cleans, stores and visualises 
ESG risk scores for S&P 500 Energy sector companies — identifying which 
companies carry the highest unmanaged environmental, social and governance risk.**

> **Disclaimer:** This project is built purely for educational and portfolio 
> purposes. All scores and classifications are sourced from publicly available 
> data provided by Sustainalytics via the S&P 500 ESG Risk Ratings dataset. 
> This analysis should not be used for investment, financial, or business 
> decisions. All findings are presented for analytical and learning purposes only.

---

## Live Dashboard

[![Tableau](https://img.shields.io/badge/Tableau-Dashboard-blue)](https://public.tableau.com/views/esg_energy_dashboard/ESGEnergyDashboard)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-black)](https://github.com/sajanyerra/esg-disclosure-gap-analyser)

🔗 [View Interactive Tableau Dashboard](https://public.tableau.com/views/esg_energy_dashboard/ESGEnergyDashboard?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

🌐 [View GitHub Pages Showcase](https://sajanyerra.github.io/esg-disclosure-gap-analyser/)

---

## Project Overview

ESG risk analysis is a core function at firms like MSCI, Sustainalytics, 
and Bloomberg ESG. Analysts assess not just what companies claim about 
their sustainability practices, but what the data actually shows about 
their unmanaged risk exposure.

This project builds that process as a complete data pipeline — from raw 
data ingestion through to an interactive Tableau dashboard — focused 
entirely on the US Energy sector within the S&P 500.

| Detail | Info |
|--------|------|
| Sector | Energy (Oil & Gas) |
| Companies | 20 S&P 500 Energy companies |
| Data Source | S&P 500 ESG Risk Ratings — Sustainalytics methodology |
| Score Type | ESG Risk Score — higher means more unmanaged risk |
| Tech Stack | Python · PostgreSQL · Tableau Desktop |

---

## Key Findings

| Finding | Detail |
|---------|--------|
| Highest ESG Risk | Occidental Petroleum — 41.7 (Severe) |
| Lowest ESG Risk | Kinder Morgan — 19.0 (Low) |
| Dominant Risk Driver | Environment drives risk in 17 of 20 companies |
| Most at risk sub-sector | Oil & Gas Integrated — avg score 39.10 |
| Least at risk sub-sector | Oil & Gas Equipment & Services — avg score 22.95 |
| Companies at High or Severe risk | 15 out of 20 (75%) |

---

## How ESG Risk Scores Work

These scores use the **Sustainalytics ESG Risk Rating methodology**:

ESG Risk Score = Exposure to ESG Risk − What the Company Manages Well
= Unmanaged / Residual Risk


This means a **higher score is worse** — it represents risk that the 
company is not adequately managing.

| Score Range | Risk Level |
|-------------|------------|
| 0 — 10 | Negligible |
| 10 — 20 | Low |
| 20 — 30 | Medium |
| 30 — 40 | High |
| 40+ | Severe |

### The Three Components

| Component | What it Measures |
|-----------|-----------------|
| Environment Risk Score | Carbon emissions, resource use, pollution |
| Social Risk Score | Labour practices, supply chain, community |
| Governance Risk Score | Board structure, transparency, executive pay |

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python (Pandas, NumPy) | Data ingestion, cleaning, analysis |
| PostgreSQL + pgAdmin 4 | Relational database storage and querying |
| Tableau Desktop | Interactive dashboard and visualisation |
| Jupyter Notebooks | Reproducible analysis workflow |

---

## Data Source

| Field | Detail |
|-------|--------|
| Dataset | S&P 500 ESG Risk Ratings |
| Provider | Sustainalytics (Morningstar) |
| Coverage | S&P 500 companies |
| Scores | Total ESG, Environment, Social, Governance |
| Availability | Kaggle — Public Domain (CC0) |

---

## Repository Structure
esg-disclosure-gap-analyser/
│
├── README.md
├── index.html ← GitHub Pages showcase
│
├── data/
│ ├── raw/ ← Original downloaded data
│ └── processed/ ← Cleaned and analysis-ready CSVs
│
├── notebooks/
│ ├── 01_data_exploration.ipynb
│ ├── 02_data_cleaning.ipynb
│ └── 03_analysis.ipynb
│
├── sql/
│ ├── schema.sql ← Table creation scripts
│ └── queries.sql ← All analysis queries
│
├── dashboard/
│ └── esg_energy_dashboard.twbx ← Tableau workbook
│
└── assets/
├── dashboard_preview.png ← Dashboard screenshot
└── report.pdf ← Exported PDF report


---

## Database Schema

Three tables in PostgreSQL:

```sql
companies         — company reference data (symbol, name, sector, industry)
esg_scores        — all ESG risk scores and classifications per company
industry_summary  — aggregated average scores by Oil & Gas sub-industry

Key SQL Queries
Companies ranked by ESG risk:

sql
SELECT c.name, s.total_esg_risk_score, s.esg_risk_level
FROM esg_scores s
JOIN companies c ON c.id = s.company_id
ORDER BY s.total_esg_risk_score DESC;
Average scores by industry:

sql
SELECT industry, avg_total_esg_risk_score, company_count
FROM industry_summary
ORDER BY avg_total_esg_risk_score DESC;
Companies at Severe or High risk:

sql
SELECT c.name, s.total_esg_risk_score, s.esg_risk_level
FROM esg_scores s
JOIN companies c ON c.id = s.company_id
WHERE s.esg_risk_level IN ('Severe', 'High')
ORDER BY s.total_esg_risk_score DESC;

Analytical Insights
Environment Dominates ESG Risk in Energy
17 of 20 companies have Environment as their dominant risk driver.
This reflects the sector's inherent exposure to carbon transition risk,
pollution liability and resource depletion — risks that many companies
are only partially managing.

Midstream Companies Carry Lower Risk
All four Oil & Gas Midstream companies rank in the bottom half of the
risk table. Pipeline and infrastructure operators have lower direct
emissions exposure compared to Exploration and Production companies
that extract fossil fuels directly.

Social Risk Dominates in Midstream Specifically
The three companies where Social is the dominant risk driver —
Targa Resources, Oneok and Williams Companies — are all Midstream
operators. Community relations, worker safety and supply chain
practices drive their risk profile rather than environmental exposure.

Integrated Oil Carries the Highest Average Risk
ExxonMobil and Occidental Petroleum both score in the Severe category.
Integrated companies operate across the full value chain — extraction,
refining, retail — accumulating ESG risk at every stage.

Limitations
Single point in time — this dataset represents one snapshot.
Trend analysis over multiple years would strengthen conclusions.
Sustainalytics methodology — scores reflect one rating agency's
view. MSCI, Bloomberg and CDP may score these companies differently.
US listed companies only — Shell, BP and TotalEnergies are absent
as they are not S&P 500 constituents.
Two companies excluded — Diamondback Energy and Baker Hughes
were removed due to missing ESG scores in the source dataset.
Self-reported inputs — some data feeding into Sustainalytics
scores originates from company disclosures which may have limitations.
Regulatory Context
This project is relevant to current ESG regulation and hiring:

EU CSRD — Corporate Sustainability Reporting Directive mandates
detailed ESG disclosures for large companies from 2024
SEC Climate Disclosure Rules — US listed companies must now
disclose Scope 1 and 2 emissions in annual filings
TCFD Framework — Task Force on Climate-related Financial
Disclosures is now mandatory in the UK and referenced globally
Sustainalytics methodology — used by institutional investors
managing trillions in assets for ESG risk screening
Related Projects
Project	Description	Stack
ireland-esg-tracker	Tracks Ireland's carbon emissions against 2030 Climate Action Plan targets	Python, SQL, Power BI
climate-change	How data can be selectively framed to build biased narratives	Python, SQL, Power BI
This project completes a three-part ESG portfolio — moving from national
emissions tracking to data framing analysis to corporate ESG risk analysis.

Author
Sai Sajan Yerra
GitHub: github.com/sajanyerra
