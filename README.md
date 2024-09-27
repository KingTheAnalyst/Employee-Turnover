# Case Study: How can TDI Corporation reduce Employee Turnover?

TDI Corporation, a company in the technology sector, has been facing a high employee turnover rate, which is negatively affecting productivity and the overall success of the organization. The Human Resources (HR) department provided a dataset containing information on employee demographics, job roles, race, and tenure to better understand the factors contributing to employee attrition.

## Objective:
The purpose of this case study is to analyze the dataset to identify key factors contributing to employee attrition and provide recommendations to reduce turnover and improve employee retention.

---

## Assumptions and Hypotheses

### Assumptions:
- Employee attrition is not affected by external factors (e.g., economic downturn).

### Hypotheses:
1. **Tenure and Likelihood of Leaving:**
   - **Null:** Employees' tenure does not affect the likelihood of leaving the company.
   - **Alternative:** Employees with shorter tenure are more likely to leave the company.
   
2. **Demographic Groups and Attrition:**
   - **Null:** There is no difference in attrition rates among different demographic groups (e.g., gender, race).
   - **Alternative:** Certain demographic groups are more prone to attrition compared to others.

3. **Job Departments and Turnover Rates:**
   - **Null:** Job departments do not show differences in turnover rates due to factors such as stress, job dissatisfaction, or lack of growth opportunities.
   - **Alternative:** Specific job departments show higher turnover rates due to factors like stress, job dissatisfaction, or lack of growth opportunities.

4. **Job Location and Attrition:**
   - **Null:** Job location does not impact employee attrition.
   - **Alternative:** Employees working remotely or at headquarters are more likely to leave the company.

5. **Seasonal Pattern in Employee Turnover:**
   - **Null:** Employee turnover does not follow a seasonal pattern, and attrition rates are consistent throughout the year.
   - **Alternative:** Employee turnover follows a seasonal pattern, with certain quarters showing higher attrition due to hiring or termination practices.

---

## Methodology

### Data Collection and Preparation
- **Data Source:** The dataset was provided by the organization and consists of HR-related information.
- **Data Preparation:**
  - **Table Importation:** Imported the HR data into Microsoft SQL Server.
  - **Table Duplication:** Created a duplicate of the original table to perform manipulations while preserving the original data.
  - **Handling Duplicates:** Identified and removed 13 duplicate records after thorough verification.
  - **Data Cleaning:**
    - Gender Standardization: Updated gender entries by changing 'FM' to 'female' and 'M' to 'male', while leaving non-conforming entries unchanged.
    - Column Addition: Added new columns for tenure, age, tenure_category, year, age_group, and employment_status for more detailed analysis.

### Visualization and Reporting
- **Views Creation:** Created views for each query in the SQL database.
- **Data Export:** Imported the query results and cleaned HR_INFO table into Excel.
- **Dashboard Creation:** Developed a comprehensive dashboard in Excel to visualize attrition factors interactively.

---

## Analysis and Hypothesis Testing

### 1. Demographic Analysis (Hypothesis 2)
- **Hypothesis:** Certain demographic groups are more prone to attrition.
- **Results:**
  - **Gender Analysis:** Female employees had the highest attrition rate at 18.08%.
  - **Age Group Analysis:** Younger employees aged between 22-34 experienced the highest attrition rate at 18.72%.
  - **Race Analysis:** The Native Hawaiian or Other Pacific Islander demographic had a notably higher attrition rate.
- **Conclusion:** Hypothesis 2 is supported. Certain demographics, particularly females and younger employees, are more prone to attrition.

### 2. Departmental Analysis (Hypothesis 3)
![image](https://github.com/user-attachments/assets/497e916c-5ebc-4a28-8505-7d73034d738d)

- **Hypothesis:** Specific departments experience higher turnover due to stress, job dissatisfaction, or lack of growth opportunities.
- **Results:** The Auditing department had the highest attrition rate at 23.81%.
- **Conclusion:** Hypothesis 3 is supported. Specific departments like Auditing have notably higher attrition rates.

### 3. Tenure Analysis (Hypothesis 1)
![image](https://github.com/user-attachments/assets/8bcf76d1-df9c-46dc-ad86-3ddd79b807d6)


- **Hypothesis:** Employees with shorter tenure (less than 3 years) are more likely to leave the company.
- **Results:** Employees with less than 3 years of tenure had significantly higher turnover rates.
- **Conclusion:** Hypothesis 1 is supported. Shorter tenure is an important predictor of attrition.

### 4. Location Analysis (Hypothesis 4)
![image](https://github.com/user-attachments/assets/5bf00293-5eb3-469d-a399-c2ae7cf8ff94)

![image](https://github.com/user-attachments/assets/61e824f6-f2d9-461d-8202-e91a77e8db94)

- **Hypothesis:** Job location may influence employee attrition.
- **Results:** Indiana had the highest attrition rate at 18.29%, followed by Ohio at 17.96%.
- **Conclusion:** Hypothesis 4 is partially supported. Remote employees in certain states show higher turnover.

### 5. Seasonal Trends (Hypothesis 5)
![image](https://github.com/user-attachments/assets/b8e1b6ff-4b89-4a81-bc44-a2ebbbcf3eef)


- **Hypothesis:** Employee turnover follows a seasonal pattern.
- **Results:** No clear seasonal pattern was observed. However, attrition steadily increased from 2001 to 2020.
- **Conclusion:** Hypothesis 5 is not supported.

---

## Key Insights:
1. Female employees and younger employees aged 22-34 show higher attrition rates.
2. The Auditing department shows the highest turnover, likely due to job dissatisfaction.
3. Employees with less than 3 years of tenure are more likely to leave, suggesting a need for better onboarding.
4. Location-based attrition patterns were observed in Indiana and Ohio, with higher remote employee turnover in Indiana.

---

## Recommendations:
1. Improve the quality of data collection to understand attrition trends better.
2. Conduct further analysis using data on compensation, job level, performance ratings, etc.
3. Organize surveys to understand employees' reasons for leaving, especially for high-risk demographics and departments.
4. Provide additional support for employees in their early years to address concerns and enhance retention.
5. Implement targeted interventions for high-turnover departments like Auditing.
6. Review remote work policies, particularly in high-attrition locations like Indiana.

---

## Conclusion:
The analysis of the HR dataset has provided valuable insights into employee attrition patterns. By categorizing tenure, calculating attrition rates, and visualizing data through an interactive dashboard, we have gained an understanding of the factors influencing employee turnover.

---
## Appendix: Dashboard


## Appendix: SQL Queries for Attrition Analysis

### 1. Yearly Attrition Rate
```sql
1. Yearly Attrition Rate
1.  ----------------------------------------------------------------------------------------
-- To get the yearly attrition rate, you need
-- 1. Year: The year in question.
-- 2. Employees Left: The number of employees who terminated in that year.
-- 3. Average Employees: The average number of employees in that year.
 
WITH employee_hiring AS (
    SELECT 
        YEAR(hire_date) AS hire_year,
        COALESCE(YEAR(termdate), YEAR(GETDATE())) AS term_year,
        hire_date,
        termdate
    FROM HR_INFO
),
employees_left AS (
    SELECT 
        term_year AS year,
        COUNT(*) AS employees_left
    FROM employee_hiring
    WHERE termdate IS NOT NULL
    GROUP BY term_year
),
average_employees AS (
    SELECT 
        years.hireyear,
        SUM(CASE WHEN E.hire_year <= years.hireyear AND E.term_year >= years.hireyear THEN 1 ELSE 0 END) / 2.0 AS avg_employees
    FROM 
        employee_hiring E
    CROSS JOIN (
        SELECT DISTINCT YEAR(hire_date) AS hireyear 
        FROM HR_INFO 
        UNION
        SELECT DISTINCT YEAR(termdate) 
        FROM HR_INFO 
        WHERE termdate IS NOT NULL
    ) AS years
    GROUP BY years.hireyear
)
SELECT 
    A.hireyear,
    COALESCE(E.employees_left, 0) AS employees_left,
    A.avg_employees,
    CASE 
        WHEN A.avg_employees > 0 THEN ROUND((COALESCE(E.employees_left, 0) / A.avg_employees) * 100, 2)
        ELSE 0
    END AS attrition_rate
FROM 
    average_employees A
LEFT JOIN employees_left E ON A.hireyear = E.year
ORDER BY A.hireyear;-------------------------------------------------------------------------------------------		


2. Attrition by Race

 1. -- Attrition by Race
 2. -- Total number of employees by race
 3. WITH TotalByRace AS (
 4.     SELECT 
 5.         race,
 6.         COUNT(*) AS "Total Employees"
 7.     FROM HR_INFO
 8.     GROUP BY race
 9. ),
10.  
11. -- Number of employees who have left the company by race
12. AttritionByRace AS (
13.     SELECT 
14.         race,
15.         COUNT(*) AS Attrition
16.     FROM HR_INFO
17.     WHERE termdate IS NOT NULL
18.     GROUP BY race
19. ),
20.  
21. -- Calculate attrition rate by race
22. AttritionRateByRace AS (
23.     SELECT 
24.         a.race,
25.         a.Attrition,
26.         t."Total Employees",
27.         CAST(a.Attrition AS FLOAT) / t."Total Employees" * 100 AS AttritionRate
28.     FROM AttritionByRace a
29.     JOIN TotalByRace t ON a.race = t.race
30. )
31.  
32. SELECT 
33.     race,
34.     Attrition,
35.     "Total Employees",
36.     CAST(AttritionRate AS DECIMAL(5, 2)) AS AttritionRate
37. FROM AttritionRateByRace
38. ORDER BY AttritionRate DESC;	
39.  

3. Attrition by Gender
 1. --------------------------------------------------------------------------------------------
 2. -- Attrition by gender
 3. -- Total number of employees by gender
 4. WITH TotalByGender AS (
 5.     SELECT 
 6.         gender,
 7.         COUNT(*) AS TotalCount
 8.     FROM HR_INFO
 9.     GROUP BY gender
10. ),
11.  
12. -- Number of employees who have left the company by gender
13. AttritionByGender AS (
14.     SELECT 
15.         gender,
16.         COUNT(*) AS AttritionCount
17.     FROM HR_INFO
18.     WHERE termdate IS NOT NULL
19.     GROUP BY gender
20. ),
21.  
22. -- Calculate attrition rate by gender
23. AttritionRateByGender AS (
24.     SELECT 
25.         a.gender,
26.         a.AttritionCount,
27.         t.TotalCount,
28.         CAST(a.AttritionCount AS FLOAT) / t.TotalCount * 100 AS AttritionRate
29.     FROM AttritionByGender a
30.     JOIN TotalByGender t ON a.gender = t.gender
31. )
32.  
33. SELECT 
34.     gender,
35.     AttritionCount,
36.     TotalCount,
37.     CAST(AttritionRate AS DECIMAL(5, 2)) AS AttritionRate 
38. FROM AttritionRateByGender
39. ORDER BY gender;
40.  

4. Attrition by Age Group
 1. --------------------------------------------------------------------------------------------
 2. -- Attrition by Age group
 3.  
 4. WITH Age_Group AS (
 5.     SELECT 
 6.         AgeGroup,
 7.         termdate,
 8.         id  
 9.     FROM HR_INFO
10. ),
11.  
12. -- Total number of employees and attrition by age group
13. Counts AS (
14.     SELECT 
15.         AgeGroup,
16.         COUNT(*) AS "Total Employees",
17.         SUM(CASE WHEN termdate IS NOT NULL THEN 1 ELSE 0 END) AS Attrition
18.     FROM Age_Group
19.     GROUP BY AgeGroup
20. )
21.  
22. SELECT 
23.     AgeGroup,
24.     "Total Employees",
25.     Attrition,
26.     CAST(Attrition AS FLOAT) / "Total Employees" * 100 AS AttritionRate
27. FROM Counts
28. ORDER BY AgeGroup;
29.  

5.Attrition by Job Department 
 1. --------------------------------------------------------------------------------------------
 3. -- ATTRITION BY JOB DEPARTMENT
 4.  
 5. WITH DepartmentCategorization AS (
 6.     SELECT 
 7.         department,
 8.         termdate,
 9.         id  
10.     FROM HR_INFO
11. ),
12.  
13. -- The total number of employees and attrition by department
14. Counts AS (
15.     SELECT 
16.         department,
17.         COUNT(*) AS "Total Employees",
18.         SUM(CASE WHEN termdate IS NOT NULL THEN 1 ELSE 0 END) AS Attrition
19.     FROM DepartmentCategorization
20.     GROUP BY department
21. )
22.  
23. SELECT 
24.     department,
25.     "Total Employees",
26.     Attrition,
27.     CAST(Attrition AS FLOAT) / "Total Employees" * 100 AS AttritionRate
28. FROM Counts
29. ORDER BY AttritionRate DESC;
30.  

1.	Impact of Years of Service on Attrition 
 1. ------------------------------------------------------------------------------------
 2. -- IMPACT OF YEARS OF SERVICE ON THE ATTRITION
 3.  
 4. -- Tenure Categorization
 5. WITH TenureCategorization AS (
 6.     SELECT 
 7.         tenure_category,
 8.         termdate,
 9.         id  
10.     FROM HR_INFO
11. ),
12.  
13. -- Total employees and attrition by tenure category
14. Counts AS (
15.     SELECT 
16.         tenure_category,
17.         COUNT(*) AS Total_Employees,
18.         SUM(CASE WHEN termdate IS NOT NULL THEN 1 ELSE 0 END) AS Attrition
19.     FROM TenureCategorization
20.     GROUP BY tenure_category
21. )
22. SELECT
23.     tenure_category,
24.     Total_Employees,
25.     Attrition,
26.     CAST((CAST(Attrition AS FLOAT) / Total_Employees * 100) AS DECIMAL(5,2)) AS AttritionRate
27. FROM Counts
28. ORDER BY 
29.     AttritionRate DESC;
30.  

2.	Impact of Location on Attrition
 1. ----------------------------------------------------------------------------------------
 2. -- IMPACT OF LOCATION ON THE ATTRITION
 3.  
 4. -- Location
 5. WITH "Location" AS (
 6.     SELECT 
 7.         location,
 8.         termdate,
 9.         id  
10.     FROM HR_INFO
11.     WHERE 
12.         hire_date IS NOT NULL
13.         AND (termdate IS NULL OR termdate <= GETDATE())
14. ),
15.  
16. -- Total employees and attrition by location
17. LocationCounts AS (
18.     SELECT 
19.         "location",
20.         COUNT(*) AS TotalCount,
21.         SUM(CASE WHEN termdate IS NOT NULL THEN 1 ELSE 0 END) AS AttritionCount
22.     FROM "Location"
23.     GROUP BY "location"
24. )
25.  
26. SELECT 
27.     "location",
28.     TotalCount,
29.     AttritionCount,
30.     CAST(AttritionCount AS FLOAT) / TotalCount * 100 AS AttritionRate
31. FROM LocationCounts
32. ORDER BY 
33.     AttritionRate DESC;
34.  

3.	Attrition by Location State
 1. -- Attrition by location state
 2. WITH EmployeeData AS (
 3.     SELECT 
 4.         location_state,
 5.         CASE 
 6.             WHEN termdate IS NOT NULL THEN 1 ELSE 0 
 7.         END AS terminated
 8.     FROM HR_INFO
 9. ),
10. Counts AS (
11.     SELECT 
12.         location_state,
13.         COUNT(*) AS TotalCount,
14.         SUM(terminated) AS Attrition
15.     FROM EmployeeData
16.     GROUP BY location_state
17. )
18. SELECT 
19.     location_state,
20.     TotalCount,
21.     Attrition,
22.     CAST(Attrition AS FLOAT) / TotalCount * 100 AS AttritionRate
23. FROM Counts
24. ORDER BY AttritionRate DESC;
25.  

4.	Attrition by Job Location and State
 1. -- Attrition by Job Location and State
 2. WITH EmployeeData AS (
 3.     SELECT 
 4.         location_state,
 5.         Location,
 6.         CASE 
 7.             WHEN termdate IS NOT NULL THEN 1 ELSE 0 
 8.         END AS terminated
 9.     FROM HR_INFO
10. ),
11. Counts AS (
12.     SELECT 
13.         location_state,
14.         Location,
15.         COUNT(*) AS TotalCount,
16.         SUM(terminated) AS Attrition
17.     FROM EmployeeData
18.     GROUP BY location_state, Location
19. )
20. SELECT 
21.     location_state,
22.     Location,
23.     TotalCount,
24.     Attrition,
25.     CAST(Attrition AS FLOAT) / TotalCount * 100 AS AttritionRate
26. FROM Counts
27. ORDER BY AttritionRate DESC;
28.  




