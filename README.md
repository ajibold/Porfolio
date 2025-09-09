# 🏅Athletes Health & Injury Analysis
---

## [Dashboard link](https://app.powerbi.com/view?r=eyJrIjoiODFmZjM3ZjMtOGEzYi00M2UyLTkxZmYtZDg5ODk0ZmY0YmI2IiwidCI6IjQ2NTRiNmYxLTBlNDctNDU3OS1hOGExLTAyZmU5ZDk0M2M3YiIsImMiOjl9)

---
## Table of Contents

- [Project Overview](#Project-Overview)
- [Data Sources](#data-Sources)
- [Tools & Technologies](#Tools-&-Technologies)
- [Data Cleaning & Preparation](#Data-cleaning-&-Preparation)
- [Outcome](#Outcome)
- [Result & Key Findings](#Result-&-KeyFindings)
- [Recommendations](#recommendations)

---  
### 📌 Project Overview

This project focuses on Athletes’ Health & Injury Analysis, stepping into the role of a Sports Performance Analyst.
The goal is to uncover injury trends, provide data-driven recommendations, and gain a deeper understanding of the factors influencing player health and performance.

The dataset contains thousands of injury events across players, clubs, and locations.

---

<img width="593" height="333" alt="Overview" src="https://github.com/user-attachments/assets/05a4451c-5837-4a2f-aa79-447cb2c15267" />
<img width="593" height="332" alt="Analysis" src="https://github.com/user-attachments/assets/638d1372-64ee-402d-ab0d-ab905083f496" />
<img width="586" height="334" alt="Profile" src="https://github.com/user-attachments/assets/480b4049-083f-446d-9e85-8547b1f95c9a" />
<img width="595" height="334" alt="overview info" src="https://github.com/user-attachments/assets/1b737c7c-e310-4827-a1c6-dab1e47fb58c" />
<img width="592" height="338" alt="Analysis info" src="https://github.com/user-attachments/assets/e27e3a9a-82a5-4054-8bfa-17cc5d76438a" />



### 📂 Data Sources

Challenge_29_Athlete Health & Injury Dataset.xlsx – Raw injury data including events, treatment, recovery, and costs.

### 🛠 Tools & Technologies

- Excel → Data cleaning & exploration

- SQL Server Import & Export Wizard → Data import into SQL Server

- SQL Server → Data analysis & preprocessing

- DAX → Calculated measures

- Power BI → Visualization, dashboards & reports

- ZoomCharts → Interactive custom visuals with drill-through

### 🧹 Data Cleaning & Preparation
Key Steps

1. Initial Inspection (Excel)

2. Modified Data Types (Excel)

3. Loaded into SQL Server

### 🔹 Issue Encountered

After importing into SQL Server, an error occurred:
The primary key column (InjuryID) contained values like INJ-00004, causing data type conflicts.

### 🔹 Resolutions

1. Adjusting the Primary Key

-- Drop existing PK
```sql
ALTER TABLE FactInjuries  
ALTER COLUMN InjuryID VARCHAR(20) NOT NULL;
```

-- Re-apply PK
```sql
ALTER TABLE FactInjuries  
ADD CONSTRAINT PK_FactInjuries PRIMARY KEY (InjuryID);
```

2. Using a Staging Table

Imported data into Staging_FactInjuries

Cleaned data (duplicates, nulls, trims)

Moved to FactInjuries after validation

Examples:

-- Remove duplicates
```sql
WITH CTE_Duplicates AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY InjuryID ORDER BY InjuryID) AS rn
    FROM Staging_FactInjuries
)
DELETE FROM CTE_Duplicates WHERE rn > 1;
```

-- Remove NULLs
```sql
DELETE FROM Staging_FactInjuries WHERE InjuryID IS NULL;
```

-- Trim spaces
```sql
UPDATE Staging_FactInjuries
SET InjuryID = LTRIM(RTRIM(InjuryID));
```


### ✅ Outcome:

Ensured data integrity

Built a robust ETL pipeline (Staging → Fact)

Maintained InjuryID as a reliable primary key

### 📊 Results & Key Findings

Recovery Days: Avg = 49 days; Quick recoveries (≤14 days) = 49.95%

Treatment Costs: Total = 27,702,121M; significant variance across treatment methods

High-Cost Injuries: 19.04% of injuries drive most expenses

Sports & Regions: Injury frequency & severity vary significantly by sport type, competition level, and region

### ✅ Recommendations
- 🎯 Targeted Prevention Programs

Injury-prevention training for high-risk sports & age groups

Focus on recurring injury patterns

- 💰 Cost Management

Monitor & negotiate high-cost treatment methods

Encourage cost-effective treatments without reducing quality

- ⏱ Faster Recovery Support

Invest in rehabilitation programs for shorter recovery times

Personalized recovery plans for athletes exceeding 14 days

-  🏟 Club-Level Monitoring

Clubs with high injury-to-cost ratios → strength & conditioning programs

Encourage best practice sharing across clubs

-  📑 Data-Driven Policy

Regularly review dashboards to inform coaches & medical staff

Use insights to refine training schedules and health protocols

📈 Dashboard Preview

Interactive Dashboard Link 👉 View Here

### 📌 Author

👤 Rofiat Ajibola

⚡ This project was created as part of the FP20 Analytics Challenge 29.
