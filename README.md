# 🩺 Diabetes Prediction — SQL Analytics Project

**Author:** Rohan Kumar Jha  
**Tools Used:** MySQL Command Line Client, MySQL Workbench  
**Dataset:** [Diabetes Prediction Dataset (Kaggle)]([https://www.kaggle.com/](https://www.kaggle.com/datasets/mathchi/diabetes-data-set))

---

## 🧭 1. Project Overview

This project analyzes a medical dataset containing diagnostic measurements used to determine whether a patient is likely to have diabetes.

**Goals:**
- Explore medical and lifestyle indicators
- Identify patterns associated with diabetes
- Build SQL-based analytical insights
- Perform segmentation and risk scoring
- Create a clean, professional analytics report

**Skills Demonstrated:**
- SQL data import
- Data validation
- Exploratory analysis
- Medical risk analytics
- Feature engineering
- Clean, structured reporting

---

## 📦 2. Dataset Description

The dataset contains **768 patient records** with medical measurements commonly used in diabetes diagnosis.

| Column Name | Description |
|---|---|
| `Pregnancies` | Number of times pregnant |
| `Glucose` | Plasma glucose concentration |
| `BloodPressure` | Diastolic blood pressure (mm Hg) |
| `SkinThickness` | Triceps skin fold thickness (mm) |
| `Insulin` | 2-hour serum insulin (mu U/ml) |
| `BMI` | Body mass index |
| `DiabetesPedigreeFunction` | Genetic diabetes likelihood score |
| `Age` | Age in years |
| `Outcome` | 1 = diabetic, 0 = non-diabetic |

**Why this dataset is valuable:**
- Realistic medical domain
- Perfect for SQL + ML-style feature engineering
- Clean numeric data
- Ideal for risk scoring and segmentation
- Strong portfolio impact

---

## 🧱 3. Database & Table Creation

```sql
-- Create database
CREATE DATABASE diabetes_data;
USE diabetes_data;

-- Create table
CREATE TABLE diabetes (
  Pregnancies               INT,
  Glucose                   INT,
  BloodPressure             INT,
  SkinThickness             INT,
  Insulin                   INT,
  BMI                       FLOAT,
  DiabetesPedigreeFunction  FLOAT,
  Age                       INT,
  Outcome                   INT
);
```

---

## 🖥️ 4. Importing the Dataset Using CMD

Move your CSV to:
```
C:\Users\itsro\OneDrive\Desktop\diabetes.csv
```

```sql
USE diabetes_data;

LOAD DATA LOCAL INFILE 'C:\\Users\\itsro\\OneDrive\\Desktop\\diabetes.csv'
INTO TABLE diabetes
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

---

## 🔍 5. Data Validation in MySQL Workbench

```sql
-- Display first 5 rows
SELECT * FROM diabetes LIMIT 5;

-- Count total rows
SELECT COUNT(*) FROM diabetes;
```

---

## 📘 6. Basic SQL Analysis (10 Tasks)

#### 1. Count diabetic vs non-diabetic patients
```sql
SELECT Outcome, COUNT(*) AS total
FROM diabetes
GROUP BY Outcome;
```

#### 2. Average glucose level
```sql
SELECT AVG(Glucose) AS avg_glucose
FROM diabetes;
```

#### 3. Average BMI
```sql
SELECT ROUND(AVG(BMI), 3) AS avg_bmi
FROM diabetes;
```

#### 4. Patients with high glucose (>140)
```sql
SELECT *
FROM diabetes
WHERE Glucose > 140;
```

#### 5. Count patients by age group
```sql
SELECT
  CASE
    WHEN Age < 30 THEN 'Young'
    WHEN Age BETWEEN 30 AND 50 THEN 'Middle Age'
    ELSE 'Senior'
  END AS age_group,
  COUNT(*) AS total
FROM diabetes
GROUP BY age_group;
```

#### 6. Average insulin level
```sql
SELECT AVG(Insulin) AS avg_insulin
FROM diabetes;
```

#### 7. Patients with BMI > 30 (obesity threshold)
```sql
SELECT *
FROM diabetes
WHERE BMI > 30;
```

#### 8. Average blood pressure
```sql
SELECT AVG(BloodPressure) AS avg_bp
FROM diabetes;
```

#### 9. Highest glucose values
```sql
SELECT *
FROM diabetes
ORDER BY Glucose DESC
LIMIT 10;
```

#### 10. Patients with zero insulin values
```sql
SELECT *
FROM diabetes
WHERE Insulin = 0;
```

---

## 🔥 7. Advanced SQL Analysis (15 Tasks)

#### 1. BMI category segmentation
```sql
SELECT
  CASE
    WHEN BMI < 18.5 THEN 'Underweight'
    WHEN BMI BETWEEN 18.5 AND 24.9 THEN 'Normal'
    WHEN BMI BETWEEN 25 AND 29.9 THEN 'Overweight'
    ELSE 'Obese'
  END AS bmi_category,
  COUNT(*) AS total
FROM diabetes
GROUP BY bmi_category;
```

#### 2. Diabetes rate by BMI category
```sql
SELECT
  CASE
    WHEN BMI < 18.5 THEN 'Underweight'
    WHEN BMI BETWEEN 18.5 AND 24.9 THEN 'Normal'
    WHEN BMI BETWEEN 25 AND 29.9 THEN 'Overweight'
    ELSE 'Obese'
  END AS bmi_category,
  Outcome,
  COUNT(*) AS total
FROM diabetes
GROUP BY bmi_category, Outcome;
```

#### 3. Diabetes rate by glucose level
```sql
SELECT
  CASE
    WHEN Glucose < 100 THEN 'Normal'
    WHEN Glucose BETWEEN 100 AND 125 THEN 'Prediabetic'
    ELSE 'Diabetic Range'
  END AS glucose_category,
  Outcome,
  COUNT(*) AS total
FROM diabetes
GROUP BY glucose_category, Outcome;
```

#### 4. Correlation-style check: high BMI + high glucose
```sql
SELECT *
FROM diabetes
WHERE BMI > 30 AND Glucose > 140;
```

#### 5. Average metrics for diabetic vs non-diabetic
```sql
SELECT Outcome,
  AVG(Glucose) AS avg_glucose,
  AVG(BMI) AS avg_bmi,
  AVG(BloodPressure) AS avg_bp
FROM diabetes
GROUP BY Outcome;
```

#### 6. Age vs diabetes outcome
```sql
SELECT Age, Outcome, COUNT(*) AS total
FROM diabetes
GROUP BY Age, Outcome
ORDER BY Age;
```

#### 7. Insulin vs glucose relationship (binned)
```sql
SELECT
  CASE
    WHEN Insulin < 50 THEN 'Low Insulin'
    WHEN Insulin BETWEEN 50 AND 150 THEN 'Medium Insulin'
    ELSE 'High Insulin'
  END AS insulin_group,
  Outcome,
  COUNT(*) AS total
FROM diabetes
GROUP BY insulin_group, Outcome;
```

#### 8. High-risk patients (medical heuristic)
```sql
SELECT *
FROM diabetes
WHERE Glucose > 150
  AND BMI > 35
  AND Age > 40;
```

#### 9. Diabetes Pedigree Function segmentation
```sql
SELECT
  CASE
    WHEN DiabetesPedigreeFunction < 0.5 THEN 'Low Genetic Risk'
    WHEN DiabetesPedigreeFunction BETWEEN 0.5 AND 1 THEN 'Medium Genetic Risk'
    ELSE 'High Genetic Risk'
  END AS pedigree_group,
  COUNT(*) AS total
FROM diabetes
GROUP BY pedigree_group;
```

#### 10. Diabetes rate by genetic risk
```sql
SELECT
  CASE
    WHEN DiabetesPedigreeFunction < 0.5 THEN 'Low Genetic Risk'
    WHEN DiabetesPedigreeFunction BETWEEN 0.5 AND 1 THEN 'Medium Genetic Risk'
    ELSE 'High Genetic Risk'
  END AS pedigree_group,
  Outcome,
  COUNT(*) AS total
FROM diabetes
GROUP BY pedigree_group, Outcome;
```

#### 11. Build a "Medical Risk Score"
```sql
SELECT
  *,
  ROUND(
    Glucose * 0.4 +
    BMI * 0.3 +
    Age * 0.2 +
    DiabetesPedigreeFunction * 10,
  2) AS risk_score
FROM diabetes
ORDER BY risk_score DESC;
```

#### 12. Patients with extremely low values (possible missing data)
```sql
SELECT *
FROM diabetes
WHERE Glucose = 0
   OR BloodPressure = 0
   OR BMI = 0;
```

#### 13. Age group vs diabetes outcome
```sql
SELECT
  CASE
    WHEN Age < 30 THEN 'Young'
    WHEN Age BETWEEN 30 AND 50 THEN 'Middle Age'
    ELSE 'Senior'
  END AS age_group,
  Outcome,
  COUNT(*) AS total
FROM diabetes
GROUP BY age_group, Outcome;
```

#### 14. Patients with high pedigree function but normal glucose
```sql
SELECT *
FROM diabetes
WHERE DiabetesPedigreeFunction > 1
  AND Glucose < 120;
```

#### 15. Combined risk segmentation
```sql
SELECT
  CASE
    WHEN Glucose > 150 AND BMI > 35 THEN 'Very High Risk'
    WHEN Glucose > 130 AND BMI > 30 THEN 'High Risk'
    WHEN Glucose > 110 THEN 'Moderate Risk'
    ELSE 'Low Risk'
  END AS risk_group,
  COUNT(*) AS total
FROM diabetes
GROUP BY risk_group;
```

---

## 🏁 8. Conclusion

This Diabetes Prediction SQL project provides a complete analysis of medical indicators and their relationship with diabetes.

**Through this project, we explored:**
- Medical risk factors
- Lifestyle indicators
- Diabetes outcome patterns
- Segmentation and scoring
- Feature engineering for predictive modelling

**SQL capabilities demonstrated:**
- Data import & validation
- Aggregations and grouping
- Conditional logic (CASE statements)
- Medical risk scoring
- Analytical storytelling
