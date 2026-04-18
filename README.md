# 📊 Breast Cancer Survival & Treatment Analytics Dashboard
Excel + SQL + Power BI Project | Breast Cancer Survival & Treatment Analytics | From Raw Data to Clincal and Non Clincal Insights

## 📑 Table of Contents
* [Project Overview](#project-overview)
* [Tools & Technologies](#Tools--technologies)
* [Dataset Overview](#dataset-overview)
* [Data Cleaning](#data-cleaning)
* [Exploratory Analysis](#exploratory-analysis)
* [Power BI Dashboard](#power-bi-dashboard)
* [Key Metrics](#key-metrics)
* [Key Insights](#key-insights)
* [Recommendations](#recommendations)
* [Dataset Source](#dataset-source)
  
###  Project Overview
This project analyses breast cancer patient data to uncover insights on survival rates, mortality, treatment effectiveness, and recurrence patterns.
> The goal is to:
> - Identify patterns across different cancer stages
> - Evaluate treatment performance
> - Understand risk factors affecting survival
> - Support data-driven healthcare decisions

###  Tools & Technologies
- SQL – Data extraction, transformation, and analysis
- Power BI – Data visualization and dashboard creation

###  Dataset Overview
 - The dataset contains information on:
> -  Patient demographics (age groups)
> - Cancer stages (Stage 0 – Stage 4)
> - Treatment types (Chemotherapy, Surgery, Radiation, etc.)
> - Survival, mortality, and recurrence indicators
 - Total Records: 5,000 patient

Sample Overview
Column
Patient_ID,Age, Gender, Cancer_Type, Cancer_Stage, Tumor_Size_cm, Affected_Breast, Receptor_Status, Histology_Comfirmation, Treatment_Type, Diagnosis_Date, Survuval_Status, Survival_Months, Recurrence_Status, Recurrence_Months

Sample Preview
| Patient_ID |Age |Gender | Cancer_Type | Cancer_Stage | Tumor_Size_cm | Affected_Breast | Receptor_Status |Histology_Comfirmation | Treatment_Type | Diagnosis_Date | Survuval_Status | Survival_Months | Recurrence_Status |Recurrence_Months |
|-----------|-----|-------|--------------|--------------|---------------|-----------------|-----------------|-----------------------|----------------|----------------|-----------------|-----------------|-------------------|------------------|
| BC000001 | 65 | Female | Inflammatory Breast Cancer | Stage 1 |1.5 | Left | ER+/PR+ | Confirmed | Targeted Therapy | 2025-08-05 | Survived | 29 | Yes | 22.0 |
| BC000002 | 63 | Female | Lobular Carcinoma | Stage 3 | 5.8 | Right|ER+/PR- | Confirmed | Hormone Therapy | 2025-10-29 | Survived | 3 | No | o |

###  Data Cleaning
- Key cleaning steps performed:
> - Removed duplicate records
> - Handled missing/null values
> - Standardized categorical fields (e.g., treatment types, stages)
> - Created calculated columns (e.g., survival rate %, mortality rate %)
> - Ensured consistent date and numeric formats 

###  Exploratory Analysis
- During analysis, the following were explored:
1. Patient Per Stage
```SQL
  SELECT 
    Cancer_Stage,
    COUNT(*) AS total_patients
FROM datasta.breast_cancer
GROUP BY Cancer_Stage
ORDER BY total_patients DESC;
```
2. Survival vs mortality distribution across age groups
```SQL
SELECT 
    Cancer_Type,
    COUNT(*) AS total_patients,
    SUM(Survival_Status = 'Survived') AS survived,
    SUM(Survival_Status = 'Deceased') AS deceased,
    ROUND(SUM(Survival_Status = 'Survived') / COUNT(*) * 100, 2)
        AS survival_rate_percentage,
    ROUND(SUM(Survival_Status = 'Deceased') / COUNT(*) * 100, 2)
        AS mortality_rate_percentage
FROM datasta.breast_cancer
GROUP BY Cancer_Type
ORDER BY mortality_rate_percentage DESC;
```
3. Impact of cancer stage on survival and Mortality outcomes
```SQL
SELECT 
    Cancer_Type,
    COUNT(*) AS total_patients,
    SUM(Survival_Status = 'Survived') AS survived,
    SUM(Survival_Status = 'Deceased') AS deceased,
    ROUND(SUM(Survival_Status = 'Survived') / COUNT(*) * 100, 2)
        AS survival_rate_percentage,
    ROUND(SUM(Survival_Status = 'Deceased') / COUNT(*) * 100, 2)
        AS mortality_rate_percentage
FROM datasta.breast_cancer
GROUP BY Cancer_Type
ORDER BY mortality_rate_percentage DESC;
```
4. Treatment Type by Survival & Mortality Rate
```SQL
SELECT 
    Treatment_Type,
    COUNT(*) AS total_patients,
    SUM(Survival_Status = 'Survived') AS survived,
    SUM(Survival_Status = 'Deceased') AS deceased,
    ROUND(SUM(Survival_Status = 'Survived') / COUNT(*) * 100, 2)
        AS survival_rate_percentage,
    ROUND(SUM(Survival_Status = 'Deceased') / COUNT(*) * 100, 2)
        AS mortality_rate_percentage
FROM datasta.breast_cancer
GROUP BY Treatment_Type
ORDER BY survival_rate_percentage DESC;
```
5. Recurrence patterns by treatment type
```SQL
SELECT 
    Treatment_Type,
    COUNT(*) AS total_patients,
    SUM(Recurrence_Status = 'Yes') AS recurrence_cases,
    ROUND(SUM(Recurrence_Status = 'Yes') / COUNT(*) * 100, 2)
        AS recurrence_rate_percentage
FROM datasta.breast_cancer
GROUP BY Treatment_Type
ORDER BY recurrence_rate_percentage DESC;
```
6. Recurrence Rate by Percentage
```SQL
SELECT 
    Cancer_Type,
    ROUND(SUM(Recurrence_Status = 'Yes') / COUNT(*) * 100, 2)
        AS recurrence_rate_percentage
FROM datasta.breast_cancer
GROUP BY Cancer_Type
ORDER BY recurrence_rate_percentage DESC;
```
7. Recuurence rate of Survival and Deceased Rate
```SQL
SELECT 
    Recurrence_Status,
    COUNT(*) AS total_patients,
    ROUND(SUM(Survival_Status = 'Survived') / COUNT(*) * 100, 2)
        AS survival_rate_percentage,
    ROUND(SUM(Survival_Status = 'Deceased') / COUNT(*) * 100, 2)
        AS mortality_rate_percentage
FROM datasta.breast_cancer
GROUP BY Recurrence_Status;
```
8. Age Most Affected (Highest Cancer Cases)
```SQL
SELECT 
    Age,
    COUNT(*) AS total_cases
FROM datasta.breast_cancer
GROUP BY Age
ORDER BY total_cases DESC;
```
9. Age Groups of both Survival and Deceased
```SQL
SELECT 
    CASE 
        WHEN Age < 30 THEN 'Under 30'
        WHEN Age BETWEEN 30 AND 39 THEN '30-39'
        WHEN Age BETWEEN 40 AND 49 THEN '40-49'
        WHEN Age BETWEEN 50 AND 59 THEN '50-59'
        WHEN Age BETWEEN 60 AND 69 THEN '60-69'
        ELSE '70+'
    END AS Age_Group,
    COUNT(*) AS total_patients,
    ROUND(SUM(Survival_Status = 'Survived') / COUNT(*) * 100, 2)
        AS survival_rate_percentage,
    ROUND(SUM(Survival_Status = 'Deceased') / COUNT(*) * 100, 2)
        AS mortality_rate_percentage
FROM datasta.breast_cancer
GROUP BY Age_Group
ORDER BY total_patients DESC;
```
10. Average Tumor Size
```SQL
SELECT 
    Cancer_Stage,
    ROUND(AVG(Tumor_Size_cm),2) AS avg_tumor_size
FROM datasta.breast_cancer
GROUP BY Cancer_Stage
ORDER BY avg_tumor_size DESC;
```
11. Check Mortality Distribution
```SQL
SELECT Survival_Status, COUNT(*)
FROM datasta.breast_cancer
GROUP BY Survival_Status;
```
12. Distribution of cancer type
```SQL
SELECT Cancer_Type, COUNT(*) 
FROM datasta.breast_cancer
GROUP BY Cancer_Type
ORDER BY COUNT(*) DESC;
```
###  Power BI Dashboard
-The dashboard provides an interactive view of:

- KPIs (Top Section):
> - Total Patients
> - Survival Rate %
> - Mortality Rate %
> - Recurrence Rate %
> - Average Survival per Month
- Filters:
> - Cancer Stage
> - Age Group
> - Treatment Type
> - Recurrence Status
> - Affected Breast
- Visuals:
1. Survival vs Mortality by Age Group
2. Survival Rate by Cancer Stage
3. Mortality Rate by Cancer Stage
4. Recurrence by Treatment Type
5. Cancer Type Distribution
   
![Dashboard](Screenshot 2024-03-21 125517...)

###  Key Metrics
- Total Patients: 5,000
- Survival Rate: 75%
- Mortality Rate: 25%
- Recurrence Rate: 30.12%
- Average Survival per Month: 29.53
<img width="1209" height="675" alt="Screenshot 2026-03-21 125517" src="https://github.com/user-attachments/assets/8bacf335-eeb3-46cb-9c3b-450f1c40935f" />

###  Key Insights
Early detection is critical:
1. Survival rates are highest in Stage 0 (96.4%) and Stage 1 (95.4%), but drop significantly in later stages.
2.  Late-stage risk is high:
> - Stage 4 has the highest mortality rate (45.5%), highlighting the danger of late diagnosis.
3. Age impact is moderate:
> - Survival rates remain relatively stable across age groups, but slightly decline in middle-aged groups.
4. Treatment effectiveness varies:
> - Surgery shows the lowest recurrence rate (~27.6%), making it one of the most effective treatments.
5. Recurrence is a major concern:
> - Around 30% of patients experience recurrence, indicating the need for better follow-up care.

###  Recommendations
1. Promote early screening programs
Focus on detecting cancer at Stage 0–1 to significantly improve survival rates.
2. Enhance treatment strategies for late stages
Invest in advanced therapies for Stage 3 and Stage 4 patients.
3. Prioritize effective treatments
Encourage the use of treatments with lower recurrence rates (e.g., surgery).
4. Improve post-treatment monitoring
Implement strong follow-up systems to reduce recurrence rates.
4. Data-driven healthcare decisions
Use dashboards like this to guide hospital policies and treatment planning.

### Dataset Source
<img width="1611" height="843" alt="Breast_Cancer_sample_image" src="https://github.com/user-attachments/assets/3ae5b979-ccc0-4112-b67a-ba35bec21489" />

[Download Here](https://docs.google.com/spreadsheets/d/1fh1cpCAQNMNBaLFDGZH1MxmXe5PedtwI8gswPV2n_fU/edit?usp=sharing)
