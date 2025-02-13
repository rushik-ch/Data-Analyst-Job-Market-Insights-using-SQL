# 📊 Data Analyst Job Market Insights using SQL

## 📌 Project Overview
This project analyzes the **job market for Data Analysts** using SQL. It extracts insights from job postings, focusing on **in-demand skills, top-paying jobs, and salary trends**. The dataset includes job postings, company details, and skill requirements, allowing us to explore key factors that influence salaries and demand.

## 🗄 Data Source & Schema
The analysis is based on a relational database with the following tables:
- **`job_postings_fact`** – Contains job details (title, location, salary, posting date, etc.).
- **`company_dim`** – Stores company names and details.
- **`skills_dim`** – Lists all available skills.
- **`skills_job_dim`** – Maps skills to job postings.

## 🔍 Key Analysis & SQL Queries
### 1️⃣ **Top 10 In-Demand Skills for Data Analysts**
```sql
SELECT skills, COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 10;
```
📌 **Insight:** Identifies the most frequently required skills for Data Analyst roles.

---
### 2️⃣ **Top 10 Highest-Paying Data Analyst Jobs**
```sql
SELECT job_id, job_title, name AS company_name, job_location, job_schedule_type, salary_year_avg, job_posted_date
FROM job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE job_title_short = 'Data Analyst' AND job_location = 'Anywhere' AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```
📌 **Insight:** Lists the best-paying Data Analyst jobs with company details and job type.

---
### 3️⃣ **Skills with Highest Average Salaries**
```sql
SELECT skills, ROUND(AVG(salary_year_avg),0) AS salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst' AND salary_year_avg IS NOT NULL
GROUP BY skills
ORDER BY salary DESC
LIMIT 10;
```
📌 **Insight:** Determines which skills are associated with higher salaries.

---
### 4️⃣ **Top-Paying Jobs with Required Skills**
```sql
WITH top_paying_jobs_skills AS (
    SELECT job_id, job_title, name AS company_name, salary_year_avg
    FROM job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE job_title_short = 'Data Analyst' AND job_location = 'Anywhere' AND salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10
)
SELECT * 
FROM top_paying_jobs_skills
INNER JOIN skills_job_dim ON top_paying_jobs_skills.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
```
📌 **Insight:** Maps the highest-paying jobs to the skills they require.

## 🚀 Key Takeaways
✅ **Data Cleaning & Transformation** – Loaded and structured CSV data into a relational database.
✅ **SQL Query Optimization** – Used efficient `JOINs`, `GROUP BY`, `ORDER BY`, and `CTEs`.
✅ **Market Insights Extraction** – Identified trends in salaries, skills, and job demand.
✅ **Business Intelligence Application** – Findings can be used by job seekers and hiring managers to make informed decisions.
