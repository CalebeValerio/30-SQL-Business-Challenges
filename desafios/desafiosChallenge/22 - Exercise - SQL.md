# 🎯 Challenge 22 — Volume Analysis by Company & Geographic Hub

### 📌 Business Problem
The Talent Acquisition and Market Intelligence teams need to identify the **top 10 companies actively hiring** across specific geographic locations. By analyzing the distribution of open roles, the business aims to benchmark recruiting competitors and identify key regional hiring hubs.

---

### 🎙️ Executive Briefing (Leadership Summary)

1. **Dimensional Star Schema Join:** Connected the central fact table (`job_postings_fact`) with the organization dimension (`company_dim`) using `company_id`, ensuring accurate corporate entity attribution for every posting.
2. **Data Sanitation & Completeness:** Applied strict filtering on `job_location IS NOT NULL` to eliminate incomplete records and safeguard regional reporting accuracy.
3. **Market Concentration & Decision-Making:** Grouped and aggregated total postings by company and location to highlight where market competitors are focusing their hiring efforts, providing immediate strategic context for regional talent sourcing and salary benchmarking.

---

### 💻 SQL Solution (DuckDB / MotherDuck)

```sql
SELECT 
    c.name AS company_name,
    j.job_location,
    COUNT(j.job_id) AS total_jobs
FROM data_jobs.job_postings_fact AS j
INNER JOIN data_jobs.company_dim AS c
    ON j.company_id = c.company_id
WHERE j.job_location IS NOT NULL
GROUP BY 
    c.name,
    j.job_location
ORDER BY 
    total_jobs DESC
LIMIT 10;
