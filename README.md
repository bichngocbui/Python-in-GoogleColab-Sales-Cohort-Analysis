# Python - GoogleColab - Sales - Cohort - Analysis
## Project Overview
This project focuses on implementing cohort analysis using KPMG transaction data to understand customer engagement patterns and retention over time. The analysis will specifically examine time-based cohorts, tracking customers from their first purchase through subsequent transactions to identify behavioral patterns and trends in customer engagement.
## Busssiness Context
Customer retention is a critical metric for businesses, especially in competitive markets. While growing the customer base is essential, retaining existing customers often has a higher ROI. Understanding how engagement evolves over time helps businesses identify trends and refine their strategies to improve loyalty and revenue. By leveraging cohort analysis, we can address questions such as:
- Are new customers staying engaged with the product/service?
- Which acquisition months have the best retention rates?
- How do retention patterns differ between customer groups?
These insights are vital for shaping marketing, product development, and customer experience strategies.
## Key Analysis Areas
### Cohort Definition
- A cohort is a collection of users who have something in common. A traditional cohort, for example, divides people by the week or month of which they were first acquired. When referring to non-time-dependent groupings, the term segment is often used instead of the cohort.
- Cohort analysis is a tool to measure user engagement over time. It helps to know whether user engagement is actually getting better over time or is only appearing to improve because of growth. Customers are divided into mutually exclusive cohorts, which are then tracked over time. Vanity indicators don’t offer the same level of perspective as cohort research.
- Generally, there are three major types of Cohort: Time cohorts (Customers who signed up for a product or service during a particular time frame), Behavior cohorts: (Customers who purchased a product or subscribed to service in the past), Size cohorts (Refer to the various sizes of customers who purchase the company’s products or services)
__However, we will focus on performing Cohort Analysis based on Time. Customers will be divided into acquisition cohorts depending on the month of their first purchase. The cohort index would then be assigned to each of the customer’s purchases, which will represent the number of months since the first transaction.__
### Retention Analysis
- Calculate retention rates across cohorts.
- Visualize engagement patterns using heatmaps.
### Behavior Insights
- Identify cohorts with the highest/lowest retention.
