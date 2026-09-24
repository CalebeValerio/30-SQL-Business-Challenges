# 🎯 Challenge 24 — Comprehensive Entity Mapping via RIGHT JOIN

### 📌 Business Problem
Executive leadership and the Partner Operations team require an audit report listing **all registered companies** alongside the roles they have advertised (`job_title_short`). Even if an organization has never published a job listing, it must be retained in the output with a `NULL` job title to accurately assess partner adoption and catalog completeness.

---

### 🎙️ Executive Briefing (Leadership Summary)

1. **Right-Precedence Preservation:** Utilized a `RIGHT JOIN` with `company_dim` positioned as the right-hand table, guaranteeing that 100% of corporate entities are preserved regardless of their posting status in the fact table.
2. **Catalog Integrity & Unmatched Rows:** Postings with matching corporate IDs display their corresponding job titles, while inactive or zero-posting accounts systematically populate job attributes as `NULL`, exposing organizational gaps.
3. **Architectural Trade-Offs:** While functionally identical to an inverted `LEFT JOIN`, implementing `RIGHT JOIN` demonstrates complete command over SQL relational joins and joins directional semantics in legacy or strict-order pipeline environments.

---

### 💻 SQL Solution (DuckDB / MotherDuck)

```sql
SELECT 
    c.name AS company_name,
    j.job_title_short AS job_title
FROM data_jobs.job_postings_fact AS j 
RIGHT JOIN data_jobs.company_dim AS c       
    ON j.company_id = c.company_id
ORDER BY 
    c.name ASC
LIMIT 10;
