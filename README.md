# LITA-Capstone-Project-2

---

### Project Title: Customer Segmentation for a Subscription Service.

### Table of Content

[Project Overview](#project-overview).

[Objectives](#objectives).

[Tools used](#tools-used).

[Data Cleaning and Preparations](#data-cleaning-and-preparations).

[Exploratory Data Analysis](#exploratory-data-analysis).

[Data Analysis and Visualization](#data-analysis-and-visualization).

[Key Findings](#key-findings).

[Recommendations](#recommendations).

[Conclusion](#conclusion).

---

### Project Overview

This project focuses on analyzing customer subscription data to uncover key insights on demographics, subscription trends, and cancellation patterns. Using SQL for data preparation, Power BI for visualization, and Excel for calculations, I developed a comprehensive dashboard suite to aid in customer segmentation and subscription analysis. This project demonstrates my skills in data cleaning, trend analysis, and dashboard creation, helping stakeholders understand subscription dynamics and make data-driven decisions.

---

### Objectives
The primary objectives of this project are:

- To classify customers by subscription type and analyze their revenue contributions.
- To examine regional revenue distribution and customer concentration for growth opportunities.
- To identify trends in subscription duration, active vs. canceled subscriptions, and customer loyalty.

---

### Tools Used

- **Excel**: For data cleaning, calculations, and pivot table analysis.
- **SQL**: For data segmentation, key metric calculations, and subscription trend exploration.
- **Power BI**: To create interactive dashboards, providing visual insights on customer and revenue distribution.
- **GitHub**: For organizing and showcasing this project as part of my portfolio.

---

### Data Cleaning and Preparation

**Data preparation steps ensured accuracy and consistency across tools**:

- *Data Import and Format Checks*: Imported data into Excel, verifying field types for date, text, and numerical consistency.
- *Handling Missing Values*: Identified and removed rows with missing values to ensure data integrity.
- *Standardization*: Standardized text in columns like `Region`, `SubscriptionType`, and `CustomerName` to prevent duplicate records due to format inconsistencies.
- *Derived Columns*:
  - *Subscription Duration*: Calculated as the difference between `SubscriptionStart` and `SubscriptionEnd` in Excel.
  - *Active/Cancelled Status*: Derived by evaluating if `SubscriptionEnd` has a date, indicating a canceled subscription.
  - *Revenue per Subscription*: Aggregated revenue per customer across active and canceled subscriptions.

 **Key SQL Queries used for cleaning and preparations**: 
 
- *Duplicate Management*:
```SQL
SELECT *, COUNT(*) AS duplicate_count
FROM CustomerData
GROUP BY CustomerID, CustomerName, Region, SubscriptionType, SubscriptionStart, SubscriptionEnd, Canceled, Revenue, "Subscription Duration"
HAVING duplicate_count > 1;

DELETE FROM CustomerData
WHERE rowid NOT IN (
    SELECT MIN(rowed)
    FROM CustomerData
    GROUP BY CustomerID, CustomerName, Region, SubscriptionType, SubscriptionStart, SubscriptionEnd, Canceled, Revenue, "Subscription Duration"
);
```

- *Null Value Detection and Removal*:
```SQL
SELECT COUNT(*) AS null_count
FROM CustomerData
WHERE CustomerID IS NULL OR CustomerName IS NULL OR Region IS NULL
      OR SubscriptionType IS NULL OR SubscriptionStart IS NULL
      OR SubscriptionEnd IS NULL OR Canceled IS NULL OR Revenue IS NULL
      OR "Subscription Duration" IS NULL;

DELETE FROM CustomerData
WHERE CustomerID IS NULL OR CustomerName IS NULL OR Region IS NULL
      OR SubscriptionType IS NULL OR SubscriptionStart IS NULL
      OR SubscriptionEnd IS NULL OR Canceled IS NULL OR Revenue IS NULL
      OR "Subscription Duration" IS NULL;
```

---

### Exploratory Data Analysis
Key questions explored in this analysis:
1. Which subscription types drive the most revenue?
2. How does revenue distribution vary across regions?
3. What are the characteristics of active vs. canceled subscriptions?
4. Who are the top customers by revenue, and what are their purchasing behaviors?
5. What are the average subscription duration and cancellation rates?

These insights aid in understanding revenue, subscription type popularity, and customer loyalty.

---

### Data Analysis and Visualization

1. **Excel Analysis**: I used excel online, so you can get the file here [Download Here](https://1drv.ms/x/c/41bec79bae4bb512/EaOvzB2De4dKh3UD2P5_T08BaV3IeyUJoaf8c_w6c3HF8w?e=Ne8teT).

- **Key Formulas Used**:
  
- *Average Subscription Duration*: `=AVERAGE(SubscriptionDurationRange)`
  
- *Active Subscriptions*: `=COUNTIF(CancelledRange, "FALSE")`
  
- *Cancelled Subscriptions*: `=COUNTIF(CancelledRange, TRUE")`
  
- *Cancellation Rate*: `=COUNTIF(CancelledRange, "TRUE") / COUNTA(CustomerNameRange)`
  
- *Average Revenue per Subscription*: `=AVERAGE(RevenueRange)`

Pivot tables were used to summarize revenue by region, count subscriptions, and average subscription durations.

**Visualization**: 

2.  SQL Analysis

- **Total Number of Customers from each Region**:
```SQL
SELECT Region, COUNT(CustomerID) AS Total_Customers
FROM CustomerData
GROUP BY Region
UNION ALL
SELECT 'Total', COUNT(CustomerID)
FROM CustomerData;
```

**Visualization**: 

- **Total Revenue by Subscription Type**:
```SQL
SELECT SubscriptionType, SUM(CAST(REPLACE(Revenue, ',', '') AS INTEGER)) AS Total_Revenue
FROM CustomerData
GROUP BY SubscriptionType
UNION ALL
SELECT 'Total', SUM(CAST(REPLACE(Revenue, ',', '') AS INTEGER))
FROM CustomerData;
```

**Visualization**: 

- **Top 3 Regions by Subscription Cancellation**:
```SQL
SELECT Region, COUNT(CustomerID) AS Cancellations
FROM CustomerData
WHERE Canceled = 'TRUE'
GROUP BY Region
ORDER BY Cancellations DESC
LIMIT 3;
```

**Visualization**: 

### Power BI Visualizations  [Download Here](https://app.powerbi.com/groups/me/reports/1defa032-0b23-405a-9b42-7e89fdb081b6?ctid=b6de804f-51cd-47ef-a151-26514ed475f0&pbi_source=linkShare&bookmarkGuid=c26374cf-d21e-4a4f-8c66-1f0883790118).

The **Customer Insights and Subscription Trends Dashboard** presents customer segmentation by subscription type and region:
- **KPI Cards** display key metrics such as Average Subscription Duration (365.35 days) and Average Revenue per Subscription (1999.0).
- **Subscription Type Breakdown** uses a Pie Chart for regional distributions, with each region representing about 25% of total subscriptions.
- **Revenue by Region and Subscription Type** is visualized through a Bar Chart, highlighting revenue-generating regions by subscription type.
- **Customer Segmentation by Region and Subscription Type** shows an even distribution of subscriptions across regions.

The **Cancellation and Subscription Analysis Dashboard** emphasizes cancellation trends and the active/canceled split by subscription type:
- **Canceled Count and Revenue by Quarter** reveals quarterly trends, showing a Q1-Q4 decline in cancellations and revenue.
- **Customer Count by Subscription Type and Canceled Status** displays the higher cancellation count for Basic subscriptions, visualized in Horizontal Bar Charts.
- **Total Subscriptions** with progress bars indicate total, canceled, and active subscriptions.

---

### Key Findings

**What is Working**
- *Active Subscription Distribution*:
  - *Basic Subscription*: High active count in the East (8,488) and North (3,366), indicating strong regional preference.
  - *Premium and Standard*: Active counts concentrated in the South (Premium, 3,382) and West (Standard, 3,376), hinting at regional inclinations for premium tiers.

- *No Early Cancellations Within Six Months*: Upon analysis, it was discovered that no customers canceled their subscriptions within the first six months. This suggests that customers generally remain engaged during the initial period, indicating effective onboarding or initial satisfaction with the service. This insight points to a retention opportunity within this timeframe, where additional engagement or value reinforcement could encourage longer-term commitment beyond the six-month mark.

- *Revenue from Premium Subscribers*: Despite a lower count, Premium subscriptions show high revenue potential, especially in the South, meriting focused retention efforts.

### What Needs Improvement
- *High Cancellation Rate Across Tiers*:
  - *Basic*: Highest cancellations in the North (5,067), implying that the Basic model may not meet expectations there.
  - *Premium and Standard*: Premium (5,064) and Standard (5,044) see high cancellations in the South and West, respectively, suggesting that value perception may need improvement.

- *Regional Cancellation Patterns*: Each subscription tier has cancellations concentrated in specific regions (North for Basic, South for Premium, West for Standard), pointing to regional influences on customer retention.

### Recommendations
1. **Targeted Retention Strategies**:
   - **Basic Subscription (North)**: Region-specific engagement initiatives to reduce cancellations, such as tailored onboarding and targeted content.
   - **Premium and Standard (South and West)**: Loyalty programs and personalized incentives for these regions to improve retention.

2. **Enhanced Value Proposition**: Align subscription benefits with customer expectations. Consider adding features for each tier to enhance perceived value and address cancellation drivers.

3. **Regionally Tailored Campaigns**: Use marketing campaigns focused on each subscription type’s unique benefits to improve satisfaction and retention.

---

### Conclusion
This project provides actionable insights into customer behavior within a subscription service, identifying key areas for revenue enhancement and retention. Leveraging data-driven recommendations for targeted marketing and customer engagement, this analysis supports sustainable growth and fosters a loyal subscriber base. The use of Excel, SQL, and Power BI enables a thorough analysis of the subscription landscape, empowering stakeholders to make strategic, data-backed decisions.

---
