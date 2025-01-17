Basic EDA with HR IBM Employee Attrition using SQL

Overview

This project performs a basic exploratory data analysis (EDA) of the fictional IBM Employee Attrition dataset. Using SQL queries, we aim to uncover factors contributing to employee attrition and provide insights into employee demographics, job satisfaction, salary patterns, and more.

About the Dataset

Source: The dataset is publicly available on Kaggle.

Purpose: Understand the key factors influencing employee attrition.

Nature: Fictional dataset created by IBM data scientists.

Workflow

Step 1: Data Preparation

Upload the dataset (Excel workbook) to SQL Server Management Studio.

Confirm successful upload by querying the data:

SELECT *
FROM ['IBM Employee Attrition$'];

Step 2: Exploratory Data Analysis

1. Attrition Analysis

Identify total employees, count of employees with attrition, and calculate the attrition percentage:

SELECT
  SUM(EmployeeCount) AS Total_Employees,
  SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS AttritionCount,
  SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*) * 100 AS AttritionPercentage
FROM ['IBM Employee Attrition$'];

2. Employee Demographics

Analyze demographics like age, gender, and marital status:

SELECT
  Age,
  Gender,
  MaritalStatus,
  COUNT(*) AS TotalEmployees
FROM ['IBM Employee Attrition$']
GROUP BY Age, Gender, MaritalStatus;

3. Business Travel Analysis

Examine the distribution of employees across business travel categories:

SELECT
  BusinessTravel,
  COUNT(*) AS TotalEmployees
FROM ['IBM Employee Attrition$']
GROUP BY BusinessTravel;

4. Department Analysis

Analyze employee distribution by department:

SELECT
  Department,
  COUNT(*) AS TotalEmployees
FROM ['IBM Employee Attrition$']
GROUP BY Department;

5. Job Role and Satisfaction Analysis

Evaluate job satisfaction, performance rating, and years in current role by job role:

SELECT
  JobRole,
  COUNT(*) AS TotalEmployees,
  AVG(JobSatisfaction) AS AvgJobSatisfaction,
  AVG(PerformanceRating) AS AvgPerformanceRating,
  AVG(YearsInCurrentRole) AS AvgYearsInCurrentRole
FROM ['IBM Employee Attrition$']
GROUP BY JobRole;

6. Salary Analysis

Analyze average, maximum, and minimum salary by job role:

SELECT
  JobRole,
  ROUND(AVG(MonthlyIncome), 2) AS AvgMonthlyIncome,
  MAX(MonthlyIncome) AS MaxMonthlyIncome,
  MIN(MonthlyIncome) AS MinMonthlyIncome
FROM ['IBM Employee Attrition$']
GROUP BY JobRole;

7. Satisfaction vs. Attrition

Explore relationships between job satisfaction, environment satisfaction, and attrition:

SELECT
  JobRole,
  COUNT(Attrition) AS AttritionCount,
  SUM(JobSatisfaction) AS JobSatisfactionSum,
  SUM(EnvironmentSatisfaction) AS EnvironmentSatisfactionSum
FROM ['IBM Employee Attrition$']
WHERE Attrition = 'Yes'
GROUP BY JobRole;

8. Age and Attrition

Analyze the relationship between age and attrition rates:

SELECT
  CASE
    WHEN Age BETWEEN 18 AND 25 THEN '18-25'
    WHEN Age BETWEEN 26 AND 35 THEN '26-35'
    WHEN Age BETWEEN 36 AND 45 THEN '36-45'
    WHEN Age BETWEEN 46 AND 55 THEN '46-55'
    ELSE '56+'
  END AS AgeCategory,
  AVG(MonthlyIncome) OVER (PARTITION BY CASE
    WHEN Age BETWEEN 18 AND 25 THEN '18-25'
    WHEN Age BETWEEN 26 AND 35 THEN '26-35'
    WHEN Age BETWEEN 36 AND 45 THEN '36-45'
    WHEN Age BETWEEN 46 AND 55 THEN '46-55'
    ELSE '56+'
  END) AS AvgMonthlyIncome,
  COUNT(*) OVER (PARTITION BY CASE
    WHEN Age BETWEEN 18 AND 25 THEN '18-25'
    WHEN Age BETWEEN 26 AND 35 THEN '26-35'
    WHEN Age BETWEEN 36 AND 45 THEN '36-45'
    WHEN Age BETWEEN 46 AND 55 THEN '46-55'
    ELSE '56+'
  END, Attrition) AS AttritionCount
FROM ['IBM Employee Attrition$']
WHERE Attrition LIKE 'Yes';

9. Overtime vs. Attrition

Evaluate the correlation between overtime and attrition rates:

SELECT
  Department,
  OverTime,
  Attrition,
  COUNT(*) AS Count
FROM ['IBM Employee Attrition$']
WHERE OverTime = 'Yes'
GROUP BY Department, OverTime, Attrition;

10. Department & Role Attrition

Identify departments or roles with higher attrition rates:

SELECT
  Department,
  JobRole,
  Attrition,
  COUNT(*) AS Count
FROM ['IBM Employee Attrition$']
WHERE Attrition LIKE 'Yes'
GROUP BY Department, JobRole, Attrition
ORDER BY Count DESC;

Insights

Key insights can be derived from the above queries, such as:

Attrition rates by department, role, or demographic group.

Relationship between job satisfaction and attrition.

Impact of overtime, salary, and travel requirements on employee retention.

Tools Used

SQL Server Management Studio (SSMS): For running SQL queries and analyzing data.

Microsoft Excel: To upload and preprocess the dataset.

Conclusion

This project demonstrates how SQL can be leveraged to uncover valuable insights from HR data. The analysis provides actionable insights into employee attrition patterns and factors influencing retention.
