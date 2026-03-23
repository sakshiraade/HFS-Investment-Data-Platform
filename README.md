# HFS Investment Data Management Platform

An end-to-end data management platform replicating the infrastructure used by hedge fund solutions data teams — built to demonstrate data collection, quality oversight, process automation, and cross-divisional reporting workflows.

---

## What This Does

This platform simulates a vendor data feed of hedge fund performance data and runs it through a full data management lifecycle:

1. **Ingestion** — loads raw fund performance data (AUM, returns, net exposure) from a simulated vendor feed
2. **Quality Oversight** — runs 5 automated checks: anomaly detection, timeliness validation, completeness checks, AUM validation, and exposure limit monitoring
3. **Clean/Flagged Split** — separates clean records from flagged ones with full audit trail
4. **Performance Reporting** — generates strategy-level KPIs on clean data only
5. **Dashboard** — Tableau dashboard for cross-divisional reporting

---

## Repository Structure

```
HFS-Investment-Data-Platform/
│
├── hfs_fund_data.csv              ← Raw vendor feed (20 funds, 6 strategies)
├── hfs_clean_data.csv             ← Post-validation clean records (14 funds)
├── hfs_flagged_data.csv           ← Flagged records with issues (6 funds)
├── hfs_validated_data.csv         ← Full dataset with quality_flag column
├── hfs_strategy_summary.csv       ← Aggregated KPIs by strategy
├── hfs_quality_pipeline.ipynb     ← Python pipeline (ingestion → audit trail)
├── hfs_queries.sql                ← 5 SQL queries for performance reporting
└── audit_log.json                 ← Full lineage and run documentation
```

---

## Data Quality Checks Implemented

| Check | Description | Threshold |
|-------|-------------|-----------|
| Missing fund name | Flags blank or null fund identifiers | Any null |
| Invalid AUM | Flags non-positive or missing AUM values | AUM ≤ 0 |
| Return anomaly | Flags extreme monthly return values | > ±50% |
| Stale data | Flags records not updated recently | > 60 days |
| Exposure breach | Flags net exposure outside safe limits | > ±150% |

**Sample run results:**
- Total records: 20
- Clean records: 14 (70.0% quality score)
- Flagged records: 6 (30.0%)

---

## SQL Queries

Five business queries covering core HFS reporting needs:

1. AUM and average returns by strategy
2. Top performing funds — current month
3. Exposure monitoring with risk flags
4. Data timeliness — days since last report
5. Quality summary — clean vs flagged breakdown

---

## Tech Stack

| Tool | Usage |
|------|-------|
| Python (Pandas, NumPy) | Data ingestion, quality checks, pipeline automation |
| SQL (SQLite) | Performance reporting queries |
| Tableau Public | Cross-divisional dashboard |
| Excel VBA | Automated data quality checker (separate workbook) |
| JSON | Audit trail and lineage documentation |

---

## Key Design Decisions

- **Audit trail first** — every pipeline run generates a timestamped JSON log documenting source file, checks applied, quality score, and output files. In financial data environments, knowing *where* a number came from and *when* it was validated is as important as the number itself.
- **Clean/flagged separation** — downstream reporting only uses validated clean data, preventing quality issues from flowing silently into dashboards.
- **Synthesized data** — the dataset was deliberately designed with realistic hedge fund strategies, AUM ranges, and return profiles, with quality issues intentionally planted to test each validation check.

---

## Tableau Dashboard

[HFS Fund Performance & Data Quality — Monthly Report](https://public.tableau.com/views/HFSFundPerformanceDataQualityPlatform/HFSMonthlyReport?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

---

*Built as a portfolio project to demonstrate hedge fund data management workflows — March 2026*
