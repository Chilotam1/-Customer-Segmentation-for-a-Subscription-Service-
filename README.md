# LITA CAPSTONE PROJECT 2
## Project Title: Customer-Segmentation-for-a-Subscription-Service
---
### Table of Content
[Project Overview](#project-overview)

[Data Sources](#data-sources)

[Tools](#tools)

[Analysis Workflow](#analysis-workflow)

[Exploratory Data Analysis](#exploratory-data-analysis)

[Data Analysis](#data-analysis)

[Data Visualizations](#data-visualizations)

[Findings](#findings)

[Recommendations](#recommendations)

[References](#references)

### Project Overview
---
 This project involves analyzing customer data for a subscription service to identify segments and trends. The analysis aims to understand customer behavior, track subscription types, and identify key trends in cancellations and renewals. I seek to gather enough insight to identify trends, make data-driven recommendations and gain a deeper understanding of customers subscription.

 ### Data Sources
 ---
 The primary source of data used here is CustomerData.csv. This dataset was given to me by Incubator Hub and similar dataset can also be gotten from any open data source online.

 ### Tools
 ---
- Ms Excel [Download Here](https://www.microsoft.com)
  - Data Cleaning
  - Pivot Table Analysis
- SQL - Quering and Analysis
- Power BI - Data Visualization and Analysis
- GitHub -  Portfolio building

### Analysis Workflow
---
#### Data Preparation in Excel
- Data Cleaning: Removing duplicates
- Basic Calculations:
   1. Using pivot tables to find total revenue by subscription, number of subscription by subscribtion type and month, average subscription duration, and month.
   2. Using Excel formulas to calculate subscription duration for each customer

#### SQL Queries and Data Analysis
- Data Import: Importing the data into SQL Server.
- Data Transformation: Using SQL queries to remove null values, filter, group, and aggregate data.

#### PowerBi Dashboard Development
- Data Import: Importing the raw data into Power BI
- Data Transformation: Removing duplicates and blank rows, as well as creating measures
- Visualizations: Creating visuals such as column charts, bar charts, donut chart, and cards represent customer data.

### Exploratory Data Analysis
---
EDA involved the exploring of the Data to answer some questions about the data such as;
- What is the total number of customers from each region?
- What is the most popular subscription type by the number of customers?
- How many customers canceled their subscription within 6 months?
- How many customers have subscriptions longer than 12 months?
- What is the total revenue by subscription type? 
- What are the top 3 regions by subscription cancellations? 
- What is the total number of active and canceled subscriptions?

### Data Analysis
---
This includes some queries I worked with during the analysis. Examples:

--- retrieve the total number of customers from each region---
```sql
SELECT Region, COUNT(DISTINCT(CustomerName)) AS Number_of_Customers
FROM dbo.CustomerData
GROUP BY REGION
```
--- find the most popular subscription type by the number of customers---
``` sql
SELECT SubscriptionType, COUNT(DISTINCT(CustomerName)) AS Number_of_Customers
FROM CustomerData
GROUP BY SubscriptionType
ORDER BY COUNT(CustomerName) DESC;
```
--- find customers who canceled their subscription within 6 months---
```sql
SELECT CustomerName, Canceled AS Canceled_Subscription
FROM CustomerData
WHERE DATEDIFF(month,SubscriptionStart,SubscriptionEnd) <= 6
		AND Canceled = '0'
```
---calculate the average subscription duration for all customers---
```sql
SELECT COUNT(DISTINCT(CustomerName)) AS All_Customers, 
	   AVG(DATEDIFF(day, SubscriptionStart, SubscriptionEnd)) AS Avg_Subscription_Duration_Days
FROM 
	  CustomerData;
```
---Find customers with subscriptions longer than 12 months---
```sql
SELECT CustomerName, SubscriptionStart, SubscriptionEnd
FROM CustomerData
WHERE DATEDIFF(month,SubscriptionStart,SubscriptionEnd) > 12;
```
--- calculate total revenue by subscription type---
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
---REMOVING NULL VALUES IN THE DATASET---
```sql
DELETE FROM CustomerData
WHERE CustomerID is NULL 
	AND CustomerName is NULL AND Region is NULL
```
---- find the top 3 regions by subscription cancellations---
```sql
SELECT TOP 3 Region, COUNT(*) AS Cancellation_Count
FROM CustomerData
WHERE Canceled = 0 --- Subscriptions that were canceled
GROUP BY Region
ORDER BY Cancellation_Count DESC;
```
--- find the total number of active and canceled subscriptions---
```sql
SELECT 
    CASE 
        WHEN Canceled = 1 THEN 'Active' 
        WHEN Canceled = 0 THEN 'Cancel' 
    END AS Subscription_Status,
    COUNT(*) AS Total_Number
FROM CustomerData
GROUP BY Canceled;

Update CustomerData
set Canceled = CASE WHEN Canceled = 1 THEN 0 ELSE 1 END
FROM CustomerData;
```
### Data Visualizations

<img width="907" alt="Customer Dashboard" src="https://github.com/user-attachments/assets/e35b676d-28f2-4a7f-b156-2d5a9a6dcf4d">

<img width="959" alt="Customer Data Pivot Table" src="https://github.com/user-attachments/assets/a5d0b81d-901a-4927-9003-da4105f22f82">

### Findings

At the end of this data analysis , it was determined that;
1. The total number of customers in each region is 5,
2. The most popular subscription type is the Basic subscription with 10 customers
3. No customer canceled their subscription within 6 months and no customer has a subscription longer than 12 months.
4. The average subscription duration of all customers is 365 days, which is 1 year.
5. The total revenue by subscription type is as follows;
   	|SubscriptionType|Revenue|
   	|--------|--------|
   	|Basic|₦ 33,776,735|
   	|Premium|₦ 16,899,064|
   	|Standard|₦ 16,864,376|

   making a total subscription revenue of ₦ 67,540,175
7. The top 3 regions by cancelations are the North, South and West.
8. There is a total of 18,612 active subscription, and 15,175 canceled subscriptions.

### Recommendations
Based on the analysis, we recommend the following actions;
 - Since Basic subscriptions are the most popular, with 10 customers, and also generate the highest revenue, contributing about half of the total revenue at ₦ 33,776,735, consider creating tailored campaigns that promote the value of Premium and Standard subscriptions to Basic users. Highlighting exclusive features or benefits that Basic customers miss out on could encourage upgrades, increasing both revenue per customer and customer lifetime value.
 - The North, South, and West regions have the highest cancellation rates. Therefore, it is advised to investigate specific factors that might contribute to cancellations in these regions. This could involve surveying customers to identify service gaps, pricing concerns, or other issues unique to these areas. Enhancing customer support, adjusting regional offerings, or providing localized discounts could help reduce churn in these key regions.
 - Consider introducing longer-term subscription options (e.g., 18-month or 24-month plans) with slight discounts or bundled perks to encourage customers to commit for a longer duration. This could stabilize revenue and lower churn.
 - There are 18,612 active subscriptions, which is a healthy base but still outpaced by the 15,175 canceled subscriptions, suggesting room for growth. In other to increase the active subscriber base, offer incentives for new sign-ups, such as a referral program or limited-time discounts. Tracking these strategies' performance could help identify effective drivers for boosting active subscriptions.
 - It is observed that while the Basic plan generates significant revenue, the Premium and Standard subscriptions are comparably lower despite potentially higher price points. To address this, reevaluate the pricing and value propositions of the Premium and Standard plans. Consider whether adding more exclusive features or providing visible added value can make these plans more attractive, potentially balancing the revenue share more evenly among the subscription types.

### Limitations and Learnings
I had to remove duplicate entries that would have affected the accuracy of my conclusion in the analysis. I also created new measures that would enable me visualize key insights. Through this analysis, I was able to gain more insights in handling data inconsistencies, SQL query optimization, and Power BI performance.

### References
During this analysis, I made reference to the following sources to better my analysis
- [Ladies In Tech Africa Bootcamp||Data Analysis](https://www.youtube.com/live/ZZJiY4Tmtgo?feature=shared)
- [W3Schools online learning](https://pathfinder.w3schools.com/learningpaths)



   	
