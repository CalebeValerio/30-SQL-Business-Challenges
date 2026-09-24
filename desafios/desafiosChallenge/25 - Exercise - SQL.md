# 🎯 Challenge 25 — Two-Way Integrity Audit via FULL OUTER JOIN

### 📌 Business Problem
The Data Engineering and Platform Governance teams need to perform a full relational integrity audit across the corporate directory (`company_dim`) and job postings fact table (`job_postings_fact`). The goal is to detect orphaned records in both directions: registered companies with zero published roles, and job postings associated with missing or unmapped enterprise entities.

---

### 🎙️ Executive Briefing (Leadership Summary)

1. **Bidirectional Data Retention (FULL OUTER JOIN):** Executed a `FULL OUTER JOIN` to merge the dimensional and fact models without dropping unmatched records from either entity.
2. **Discrepancy Isolation Filter:** Implemented a targeted conditional filter (`WHERE j.job_id IS NULL OR c.company_id IS NULL`) to exclude balanced relationships and strictly surface relational gaps.
3. **Data Quality & Pipeline Governance:** Provided an actionable diagnostic query for data pipeline monitoring, helping identify upstream ingestion errors, unlinked postings, and inactive enterprise accounts in a single pass.

---

### 💻 SQL Solution (DuckDB / MotherDuck)

```sql
SELECT 
    c.company_id,
    c.name AS company_name,
    j.job_id
FROM data_jobs.company_dim AS c
FULL OUTER JOIN data_jobs.job_postings_fact AS j
    ON c.company_id = j.company_id
WHERE 
    j.job_id IS NULL 
    OR c.company_id IS NULL
LIMIT 10;
