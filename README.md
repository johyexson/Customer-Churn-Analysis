# Churn-Analysis
![](Intro-Image.png)
# Table of Contents
- [Case Study](#case-study)
- [Data Overview](#data-overview)
- [Hypothesis](#hypothesis)
- [Tools for Analysis](#tools-for-analysis)
- [Method of Analysis](#method-of-analysis)
- [Codes](#codes)
- [Summary of Findings](#summary-of-findings)
- [Recommendations](#Recommendations)
- [Dashboard](#dashboard)
# Case Study
A fictional telecommunications company that provides phone and internet services to 7,043 customers in California wants to evaluate their customer churn rates and is looking to devise strategies to retain customers as well as prevent fall outs.
# Data Overview 
The dataset include details about customer demographics, location, services, current status, etc. It consists of multiple tables, 7043 records and 34 fields stored in a csv file. [Link](https://mavenanalytics.io/data-playground?search=customer%20churn)
# Hypothesis
If the telecommunications company is not properly attuned to the changing needs and support requirements of its customers, there will be high rate of churn among its customers. 
It is also important to note that, even a seemingly small, single-figure increases in churn rate can quickly have a major negative effect on any company’s ability to grow as these churn rates are more likely to compound over time.
# Tools for Analysis 
- Excel
- [MySQL](Customer_Churn_Analysis.sql)
- [Power BI](https://app.powerbi.com/view?r=eyJrIjoiZGZjZWZjNzYtODhlMS00MzFiLWIxYTMtMjAyZjllMjc3ZmM4IiwidCI6ImRmODY3OWNkLWE4MGUtNDVkOC05OWFjLWM4M2VkN2ZmOTVhMCJ9)
# Method of Analysis
The method I utilized was exploratory data analysis (EDA) to detect patterns and trends in the data through statistical methods and visualizations. This process includes the following steps:
- **Data Cleaning**:
This involved removing duplicates, inputting null values, converting data types to appropriate format in order to ensure consistency, and eliminating errors in the data using Excel formulas such as VLOOKUP, XLOOKUP, IFS, etc.
- **Data Transformation**:
This involved executing queries in MySQL to extract insights from the data, utilising clauses and functions(such as CTEs, UPDATE, GROUPBY, CASE statements, etc.) to aggregate data based on the analysis needs.
- **Data Visualization**:
This involved importing the data into Power BI for visualization and developing a dashboard using charts and graphs to present the results/findings of the analysis.
# Codes 
Here is a glimpse of the queries constructed in MySQL to analyze the data:

```sql
-- What is the overall churn rate of customers --
SELECT Total_Customers, Churned_Customers,
CAST((Churned_Customers * 0.1 /Total_Customers * 0.1 * 100) AS DECIMAL (10,2)) AS Churn_Rate
FROM (SELECT 
		COUNT(*) AS Total_Customers
FROM customer_churn.telecom_customer_churn) AS Total,
	(SELECT COUNT(*) AS Churned_Customers
FROM customer_churn.telecom_customer_churn
		WHERE Customer_Status = 'Churned') AS Churned;

-- Adding an extra column to calculate tenure of customers in years --
ALTER TABLE customer_churn.telecom_customer_churn
ADD Tenure_Range VARCHAR (50);

UPDATE customer_churn.telecom_customer_churn
SET Tenure_Range =
CASE 
	WHEN Tenure < 12 THEN "Less than a year"
    WHEN Tenure = 12 THEN "1 year"
    WHEN Tenure > 12 AND Tenure <= 24 THEN "2 years"
    WHEN Tenure > 24 AND Tenure <= 36 THEN "3 years"
    WHEN Tenure > 36 AND Tenure <= 48 THEN "4 years"
    WHEN Tenure > 48 AND Tenure <= 60 THEN "5 years"
    WHEN Tenure > 60 AND Tenure <= 72 THEN "6 years"
END;

-- What is the typical tenure for churned customers? --
SELECT Tenure_Range,
	COUNT(*) AS Total_Customers,
    SUM(Churn) AS Churned_Customers,
    CAST(SUM(Churn) * 1.0/ COUNT(*) * 100 AS DECIMAL (10,2)) AS Churn_Rate
FROM customer_churn.telecom_customer_churn
GROUP BY Tenure_Range
ORDER BY Churn_Rate DESC;
```
The full code can be viewed [here](Customer_Churn_Analysis.sql)

# Summary of Findings
- The overall churn rate of customers was 26.54% with the total number of churned customers being 1869 out of 7043, showing that the company has lost a significant portion of its customers.
Trends based on the result of the analysis shows that longer tenures are associated with lower churn rates while customers that have been with the company for less than a year have the highest churn rate
![](Insight_1.png)

 - The key drivers of churn identified by the customers as reasons for churning were predominantly competitors and attitude of support persons leading to service dissatisfaction
![](Insight_2.png)

- Based on the analysis, the most preferred payment method by churned customers was bank withdrawal indicating that there could be issues regarding service dissatisfaction which led to churn
![](Insight_3.png)

- Customers with monthly contracts have higher churn rates as there is little or no incentive to go into a one year or two year contract
![](Insight_4.png)

# Recommendations
What should the telecommunications company do after seeing this trends?

- Since most of the churned customers only spent a few months with the company, customer loyalty should be built earlier on to mitigate customer attrition. The company can only actively reduce churn by focusing on the areas where they’re losing customers.
- It is more cost-efficient to retain existing customers than acquiring new ones. Therefore, improve customer satisfaction, provide efficient services as well as incentives and ensure proper customer support to reduce the rate at which customers switch to other competitiors
- To reduce churn, incentivise customers to switch to one year or two year contracts. Also, since there isn't much difference between the average monthly charges of all contract types, raising the average monthly charges for month to month contracts will enhance the switch and this will have a huge impact on the revenue
 
# Dashboard
You can explore the dashboard I created in Power BI [here](https://app.powerbi.com/view?r=eyJrIjoiZGZjZWZjNzYtODhlMS00MzFiLWIxYTMtMjAyZjllMjc3ZmM4IiwidCI6ImRmODY3OWNkLWE4MGUtNDVkOC05OWFjLWM4M2VkN2ZmOTVhMCJ9)


