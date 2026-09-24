# 🎯 Challenge 23 — Inactive Account Audit via Anti-Join (LEFT JOIN)

### 📌 Business Problem
The Customer Success and Growth Marketing teams need to identify **registered companies that have never published a single job posting** on the platform. The objective is to retrieve their unique identifiers and corporate names to trigger targeted re-engagement email campaigns and diagnose potential onboarding friction.

---

### 🎙️ Executive Briefing (Leadership Summary)

1. **Baseline Entity Preservation (LEFT JOIN):** Positioned `company_dim` as the primary left table to ensure all registered organizations are retained prior to evaluating job activity.
2. **Anti-Join Filtering Logic:** Evaluated the foreign key connection against `job_postings_fact` and applied `WHERE j.job_id IS NULL`, effectively isolating zero-activity accounts by catching unmatched dimensional records.
3. **Actionable Deliverable for Retention:** Eliminated redundant aggregations to deliver a clean, deterministic list of inactive accounts sorted alphabetically, allowing direct integration into CRM re-engagement workflows.

---

### 💻 SQL Solution (DuckDB / MotherDuck)

```sql
SELECT 
    c.company_id,
    c.name AS company_name
FROM data_jobs.company_dim AS c
LEFT JOIN data_jobs.job_postings_fact AS j
    ON c.company_id = j.company_id
WHERE j.job_id IS NULL
ORDER BY 
    c.name ASC
LIMIT 10;
