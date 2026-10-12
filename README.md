<div align="center">

# Patient Readmission Analytics

### Healthcare Data Analytics · SQL · Power BI · 30-Day Readmission Risk

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![SQL](https://img.shields.io/badge/SQL-Analytics-00599C?style=for-the-badge&logo=sqlite&logoColor=white)](https://en.wikipedia.org/wiki/SQL)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/kalisettikiran920-afk)

**An end-to-end hospital readmission analytics project — from raw diabetic encounter data to a 3-page interactive Power BI dashboard surfacing actionable clinical and operational insights.**

[Overview](#-project-overview) •
[Objectives](#-objectives) •
[Dataset](#-dataset) •
[Repository Structure](#-repository-structure) •
[SQL Pipeline](#-sql-analysis-pipeline) •
[Dashboard](#-power-bi-dashboard) •
[Key Findings](#-key-findings) •
[Tech Stack](#-tech-stack)

</div>

---

## 📌 Project Overview

This project delivers a **Patient Readmission Analytics** solution built on the UCI Diabetes 130-US Hospitals dataset. It addresses a critical operational challenge in modern healthcare:

> **"Which patient groups are most at risk of being readmitted within 30 days — and what clinical and discharge factors drive that risk?"**

The project covers the complete analytics pipeline — from raw data ingestion and cleaning in PostgreSQL, through structured business-question queries, to a fully interactive 3-page Power BI dashboard with **Executive Overview**, **Clinical Deep Dive**, and **Discharge Analysis** views.

The analysis is grounded in a rigorous eligibility framework: encounters ending in death or hospice discharge (IDs 11, 13, 14, 19, 20, 21, 26) are excluded from the readmission denominator, leaving **99,343 eligible encounters** as the analytical base.

---

## 🎯 Objectives

1. **Build a Clean Analytical Dataset:** Ingest raw hospital encounter data, cast all fields to appropriate types, replace invalid markers (`?`, `Unknown/Invalid`) with NULL, and document every cleaning decision.
2. **Define Readmission Eligibility:** Apply clinically sound exclusion criteria to remove patients who could not logically be readmitted within 30 days.
3. **Answer Seven Business Questions via SQL:** Examine how readmission rates vary by age group, gender, length of stay, previous inpatient visits, admission type, discharge destination, and diabetes medication changes.
4. **Build a 3-Page Interactive Power BI Dashboard:** Translate SQL findings into an executive-ready visual product with slicers, KPI cards, and clinical/discharge deep-dive pages.
5. **Surface Actionable Insights:** Identify the highest-risk patient segments and discharge pathways to support clinical intervention planning.

---

## 📊 Dataset

| Attribute | Detail |
|---|---|
| **Source** | [UCI ML Repository — Diabetes 130-US Hospitals (1999–2008)](https://archive.ics.uci.edu/ml/datasets/Diabetes+130-US+hospitals+for+years+1999-2008) |
| **Raw Rows** | 101,766 hospital encounters |
| **Eligible Encounters (post-exclusion)** | 99,343 |
| **Features** | 50 columns — demographics, clinical measures, medication flags, diagnosis codes, and readmission label |
| **Readmission Label** | `<30` (within 30 days), `>30` (after 30 days), `NO` (not recorded) |
| **Supporting Lookup Files** | `Admission Type.xlsx`, `Admission Source.xlsx`, `Discharge Disposition.xlsx`, `IDS_mapping.csv` |

---

## 📁 Repository Structure

```
Healthcare Analytics/
│
├── README.md                         # Project documentation (this file)
│
├── diabetic_data.csv                 # Raw source dataset (101,766 encounters)
├── IDS_mapping.csv                   # ID-to-label mapping for codes
├── Admission Type.xlsx               # Admission type code lookup
├── Admission Source.xlsx             # Admission source code lookup
├── Discharge Disposition.xlsx        # Discharge disposition code lookup
│
├── healthcare_analytics.sql          # Full PostgreSQL pipeline (7 sections, 447 lines)
│
└── Readmission Analysis.pbix         # Power BI dashboard (3 pages)
```

---

## 🗄️ SQL Analysis Pipeline

The [`healthcare_analytics.sql`](healthcare_analytics.sql) file is organized into **7 structured sections**:

| Section | Purpose |
|---|---|
| **1 — Schema Setup** | Creates the `healthcare` schema |
| **2 — Raw Table** | Defines `diabetic_data_raw` with all columns as TEXT for safe import |
| **3 — Import Validation** | Confirms row count (expected: 101,766) and inspects sample records |
| **4 — Data Cleaning** | Casts numeric fields, replaces `?` and `Unknown/Invalid` with NULL, outputs `diabetic_data_clean` |
| **5 — Data-Quality Validation** | Checks row count preservation, unique encounter IDs, and missing-value distribution |
| **6 — Readmission Eligibility** | Excludes death/hospice discharge codes (11, 13, 14, 19, 20, 21, 26); outputs `readmission_analysis` (99,343 rows) |
| **7 — Business Analysis** | Seven SELECT queries answering clinical and operational questions |

### Business Questions Answered

```sql
-- Q1: Readmission rate by age group
-- Q2: Readmission rate by gender (NULL/Unknown excluded)
-- Q3: Readmission rate by length of hospital stay
-- Q4: Readmission rate by previous inpatient visit count (HAVING >= 50 for reliability)
-- Q5: Readmission rate by admission type
-- Q6: Readmission rate by discharge destination (HAVING >= 100)
-- Q7: Effect of diabetes medication changes on 30-day readmission rate
```

---

## 📈 Power BI Dashboard

The dashboard is organized into **three pages**, each targeting a different analytical audience:

---

### Page 1 — Executive Overview

High-level KPIs and population-level breakdown of readmission patterns.

| KPI Card | Value |
|---|---|
| Eligible Encounters | 99,343 |
| 30-Day Readmissions | 11,314 |
| **30-Day Readmission Rate** | **11.39%** |
| Average Length of Stay | 4.38 days |
| Average Medications | 15.98 |

**Visuals included:**
- 30-Day Readmission Rate by Age Group (horizontal bar)
- Readmission Status Distribution — NO / >30 / <30 (pie chart)
- 30-Day Readmission Rate by Gender (clustered bar)
- 30-Day Readmission Rate by Admission Type (clustered bar)
- 30-Day Readmission Rate by Length of Stay in Days (area chart)

**Filters:** Age Group · Gender · Admission Type · Diabetes Medication Changed · Discharge Disposition

---

### Page 2 — Clinical Deep Dive

Clinical factor analysis — lab procedures, medications, and inpatient history.

**Visuals included:**
- Lab Procedures vs. Medications by Readmission Status (scatter plot — `<30` / `>30` / NO)
- 30-Day Readmissions and Readmission Rate by Previous Inpatient Visits (combo bar + line)
- 30-Day Readmissions by Discharge Disposition (treemap)

**Filters:** Race · Medication Count · Diabetes Medication Changed · Previous Emergency Visits · Lab Procedures Count

---

### Page 3 — Discharge Analysis

Discharge destination risk analysis with KPI cards per discharge type and an embedded key insights panel.

| Discharge Destination | Encounters | 30-Day Readmission Rate |
|---|---|---|
| Discharged to Home | 60,234 | 9.30% |
| **Skilled Nursing Facility (SNF)** | 13,954 | **14.66%** |
| Home Health Service | 12,902 | 12.70% |
| Left Against Medical Advice (AMA) | 623 | 14.45% |

**Filters:** Admission Source · Admission Type · Readmission Status · Race · Length of Stay

---

## 🔑 Key Findings

### 1. Overall Readmission Rate: 11.39%
Out of 99,343 eligible encounters, **11,314 were recorded as readmitted within 30 days** — consistent with published benchmarks for diabetic patient populations (11–12%).

### 2. Young Adults (20–30) Show the Highest Readmission Rate
At **14.31%**, the [20–30) group has the highest readmission rate of any age band — higher than any elderly group. This may reflect lifestyle non-adherence, lack of chronic disease management experience, or social support gaps, and warrants targeted post-discharge intervention.

### 3. Readmission Risk Rises With Length of Stay
Readmission rates climb steadily with length of stay, peaking around **8–10 days (14.59%–14.81%)**. Longer stays signal clinical complexity that persists post-discharge.

### 4. Prior Inpatient History is the Strongest Single Predictor
Patients with **no prior inpatient visits** have a readmission rate of just **8.59%**. This rises sharply to **34.81%** for patients with 6 prior visits and approaches **100%** at extreme visit counts — confirming that inpatient history is the most powerful readmission risk signal in this dataset.

### 5. SNF and AMA Discharges Carry Significantly Elevated Risk
Patients discharged to **Skilled Nursing Facilities (14.66%)** and those who **Left Against Medical Advice (14.45%)** show readmission rates well above the 11.39% average. AMA patients in particular may benefit from proactive follow-up outreach programs.

### 6. Emergency Admissions Lead Among Clinical Admission Types
**Emergency admissions (11.83%)** have a higher readmission rate than Urgent (11.35%), Elective (10.49%), or Newborn (10.00%) admissions, reflecting the acute, unplanned nature of the original encounter.

### 7. Medication Changes Correlate With Higher Readmission
Encounters where a diabetes medication was changed during the visit show elevated readmission rates compared to no-change encounters — supporting the clinical hypothesis that medication adjustments signal unstable glucose control.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **PostgreSQL** | Data ingestion, type casting, cleaning, eligibility filtering, and all 7 business-question queries |
| **Power BI Desktop** | 3-page interactive dashboard — KPI cards, scatter plots, treemaps, combo charts, slicers |
| **Microsoft Excel** | Lookup tables for admission type, admission source, and discharge disposition code mappings |
| **CSV** | Primary source data — 101,766 diabetic hospital encounters, 50 columns |

---

## ⚙️ How to Reproduce

### SQL (PostgreSQL)

1. Create the database:
   ```sql
   CREATE DATABASE healthcare_analytics;
   ```
2. Connect to `healthcare_analytics` in pgAdmin or psql.
3. Import `diabetic_data.csv` into `healthcare.diabetic_data_raw` using pgAdmin's import tool or `\COPY`.
4. Run `healthcare_analytics.sql` section by section — each section has validation queries to confirm output before proceeding.

### Power BI

1. Open `Readmission Analysis.pbix` in Power BI Desktop.
2. If prompted, update the data source path to point to your local `diabetic_data.csv`.
3. Refresh the dataset — all three dashboard pages populate automatically.

---

## 👤 Author

**Kiran Kalisetti**  
*Aspiring Data Analyst*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/kiran-kalisetti)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/kalisettikiran920-afk)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:kalisettikiran920@gmail.com)
