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
- Generally, there are three major types of Cohort: Time cohorts (Customers who signed up for a product or service during a particular time frame), Behavior cohorts (Customers who purchased a product or subscribed to service in the past), Size cohorts (Refer to the various sizes of customers who purchase the company’s products or services).

__However, we will focus on performing Cohort Analysis based on Time. Customers will be divided into acquisition cohorts depending on the month of their first purchase. The cohort index would then be assigned to each of the customer’s purchases, which will represent the number of months since the first transaction.__
### Retention Analysis
- Calculate retention rates across cohorts.
- Visualize engagement patterns using heatmaps.
### Behavior Insights
- Identify cohorts with the highest/lowest retention.
## Bussiness Value
By performing this analysis, the company can:
- Improve Retention Strategies: Identify the drivers of long-term customer engagement and focus on high-performing cohorts.
- Enhance Marketing ROI: Tailor campaigns to nurture early-stage users and re-engage at-risk customers.
- Growth Strategy Optimization: Understand which acquisition periods deliver the most valuable customers and replicate those conditions.
## Dataset Description
The KPMG transaction dataset includes:
![image](https://github.com/user-attachments/assets/5c5828f6-7828-4807-9090-488e4bb9bd9d)
### Technical Implementation
- Pandas for data manipulation
- NumPy for numerical computations
- Matplotlib/Seaborn for visualization
## Cohort analysis
### EDA
```python
# Display a concise summary of the DataFrame, including data types and non-null counts  
df.info()
# Generate descriptive statistics for numerical columns in the DataFrame  
df.describe()
# Filter the DataFrame to include only rows with 'order_status' equal to 'Approved', 
# and drop rows where 'order_status' has missing values  
df = df[df['order_status'] == 'Approved'].dropna(subset=['order_status'])
# Display the cleaned and filtered DataFrame  
df
```
![image](https://github.com/user-attachments/assets/3c434ce6-36f5-4509-a526-d41f7dfe0b5e)
### Cohort calculation
```python
# Create a new column 'transaction_month' to extract the month and year from 'transaction_date' and convert it to the first day of that month as a timestamp  
df['transaction_month'] = df['transaction_date'].dt.to_period('M').dt.to_timestamp()
# Create a new column 'cohort_month' to identify the first transaction month for each customer, grouped by 'customer_id'  
df['cohort_month'] = df.groupby('customer_id')['transaction_date'].transform(min).dt.to_period('M').dt.to_timestamp()
# Calculate the 'cohort_index' column, representing the number of months elapsed since the customer's first transaction (cohort month)  
df['cohort_index'] = ((df['transaction_month'].dt.year - df['cohort_month'].dt.year) * 12 +
                      (df['transaction_month'].dt.month - df['cohort_month'].dt.month))
# Display the DataFrame with the newly added cohort-related columns  
df
```
![image](https://github.com/user-attachments/assets/b87356b3-6df1-4579-b66b-ee47f5ba7ae2)

```python
# Group the data by 'cohort_month' and 'cohort_index', and calculate the number of unique customers ('customer_id') in each cohort and time period  
cohort_data = df.groupby(['cohort_month', 'cohort_index'])['customer_id'].nunique().reset_index()
# Create a pivot table 
cohort_table = cohort_data.pivot_table(index='cohort_month',    # Rows represent the 'cohort_month' (month of first transaction)
                                       columns='cohort_index',  # Columns represent the 'cohort_index' (number of months since the first transaction)
                                       values='customer_id')    # Values represent the number of unique customers  
# Calculate the retention table by dividing each cohort's unique customer count by the cohort's initial count (month 0) to compute retention percentages, then multiplying by 100  
retention_table = cohort_table.div(cohort_table[0], axis=0) * 100
# Format the index of the retention table to display 'cohort_month' in the 'YYYY-MM' format  
retention_table.index = retention_table.index.strftime('%Y-%m')
```
![image](https://github.com/user-attachments/assets/48fefb29-c4b6-4fbc-be71-add578b3360a)

```python
# Create a heatmap to visualize the retention table
plt.figure(figsize=(12, 8))
# Use Seaborn to plot the heatmap:
sns.heatmap(retention_table,
            annot=True,    # Data: 'retention_table'
            fmt='.1f',     # Display values with one decimal place (fmt='.1f')
            cmap='Blues',  # Color map: 'Blues'
            vmin=0,        # Range of values: 0 to 100 (vmin=0, vmax=100)
            vmax=100)
# Add a title and axis labels for better understanding of the chart
plt.title('MoM Retention Rate for Customer Transaction Data')
plt.xlabel('Cohort Index (Months)')
plt.ylabel('Cohort Month')
# Show the heatmap
plt.show()
```
![image](https://github.com/user-attachments/assets/e0cb9d00-a8a9-4a22-b577-5736b22b8443)

The heatmap shows the Month-over-Month (MoM) retention rates for customer transactions, with the cohort months on the y-axis and cohort index (months since the first purchase) on the x-axis. Each cell in the heatmap represents the retention rate for a specific cohort month and cohort index (month since first purchase).
### Insights
- Declining Retention Across Cohorts: All cohorts show a significant drop in retention after the first month, which is a common trend. 
- Consistency in Long-Term Retention: Across most cohorts, the retention rates stabilize between 30-40% in the later months. 
- Seasonality Effect: Some cohorts, such as July 2017, exhibit better retention compared to others. This may indicate seasonal influences or successful engagement strategies during these months.
- Drop-off Patterns: Retention sharply decreases after the first month in later cohorts (August–December 2017), with some showing retention below 30% by the second month. This could indicate either customer churn issues or a potential decline in the quality of acquired customers.
### Recommendations
#### Strengthening the first 60-day customer experience
The biggest drop occurs in the first month. Improve onboarding experiences by: 
- Personalizing the journey for new customers
- Offering promotions or incentives in the first month to encourage continued engagement
- Proactively addressing barriers to usage or satisfaction.
#### Implementing proactive customer success programs
- Analyze High-Retention Cohorts: Investigate why July 2017 has higher retention in months 4 and 5. Was a special campaign, feature, or service improvement implemented during that period? Use these findings to replicate success.
- Refine Customer Acquisition: Cohorts from August to December show weaker retention early on. Review acquisition channels or campaigns during these months to ensure the quality of new customers aligns with long-term retention goals.
#### Developing segment-specific retention strategies
Use retention data to segment customers based on their activity patterns and focus on high-value segments with personalized offers and communication.
## Conclusion
The customer retention analysis reveals both challenges and opportunities in our current business model. With a significant drop in the first 60 days (60-65% loss) but some cohorts maintaining stable long-term retention around 35-40%, there's clear room for improvement. This analysis brings valuable business insights by identifying key revenue opportunities through improved retention strategies, optimized customer acquisition costs, and enhanced customer lifetime value. The data-driven approach provides clear direction for strategic improvements, enabling more precise targeting and engagement initiatives that can lead to sustainable business growth.

