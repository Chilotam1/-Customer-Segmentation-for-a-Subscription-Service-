# LITA CAPSTONE PROJECT 2
## Project Title: Customer-Segmentation-for-a-Subscription-Service

#### Project Overview
 This project involves analyzing customer data for a subscription service to identify segments and trends. The analysis aims to understand customer behavior, track subscription types, and identify key trends in cancellations and renewals. I seek to gather enough insight to identify trends, make data-driven recommendations and gain a deeper understanding of customers subscription.

 #### Data Sources
 The primary source of data used here is CustomerData.csv. This dataset was given to me by Incubator Hub and similar dataset can also be gotten from any open data source online.

 #### Tools
- Ms Excel [Download Here](https://www.microsoft.com)
  - Data Cleaning
  - Pivot Table Analysis
- SQL - Quering and Analysis
- Power BI - Data Visualization
- GitHub -  Portfolio building

### 4. Analysis Workflow
---
#### 4.1 Data Preparation in Excel
- Data Cleaning: Removing duplicates
- Basic Calculations:
   1. Using pivot tables to find total revenue by subscription, number of subscription by subscribtion type and month, average subscription duration, and month.
   2. Using Excel formulas to calculate subscription duration for each customer

#### 4.2 SQL Queries and Data Analysis
- Data Import: Importing the data into SQL Server.
- Data Transformation: Using SQL queries to remove null values, filter, group, and aggregate data.

#### 4.3 PowerBi Dashboard Development
- Data Import: Importing the raw data into Power BI
- Data Transformation: Removing duplicates and blank rows, as well as creating measures
- Visualizations: Creating visuals such as column charts, bar charts, donut chart, and cards represent customer data.

### 5. Exploratory Data Analysis
---
EDA involved the exploring of the Data to answer some questions about the data such as;
- What is the total number of customers from each region?
- What is the most popular subscription type by the number of customers?
- How many customers canceled their subscription within 6 months?
- How many customers have subscriptions longer than 12 months?
- What is the total revenue by subscription type? 
- What are the top 3 regions by subscription cancellations? 
- What is the total number of active and canceled subscriptions?

### 6. Data Analysis
---
This includes some queries I worked with during the analysis. Example:

```sql
SELECT Region, COUNT(DISTINCT(CustomerName)) AS Number_of_Customers
FROM dbo.CustomerData
GROUP BY REGION
```
``` sql
SELECT SubscriptionType, COUNT(DISTINCT(CustomerName)) AS Number_of_Customers
FROM CustomerData
GROUP BY SubscriptionType
ORDER BY COUNT(CustomerName) DESC;
```
```sql
SELECT CustomerName, Canceled AS Canceled_Subscription
FROM CustomerData
WHERE DATEDIFF(month,SubscriptionStart,SubscriptionEnd) <= 6
		AND Canceled = '0'
```
```sql
SELECT COUNT(CustomerName) AS All_Customers, 
	   AVG(DATEDIFF(day, SubscriptionStart, SubscriptionEnd)) AS Avg_Subscription_Duration_Days
FROM 
	  CustomerData;
```
```sql
SELECT CustomerName, SubscriptionStart, SubscriptionEnd
FROM CustomerData
WHERE DATEDIFF(month,SubscriptionStart,SubscriptionEnd) > 12;
```
```sql
SELECT 
    CASE 
        WHEN Canceled = 1 THEN 'Active' 
        WHEN Canceled = 0 THEN 'Cancel' 
    END AS Subscription_Status,
    COUNT(*) AS Total_Number
FROM CustomerData
GROUP BY Canceled;
```
```sql
DELETE FROM CustomerData
WHERE CustomerID is NULL 
	AND CustomerName is NULL AND Region is NULL
```

