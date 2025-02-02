# Telecommunication_project

## Project_overview

   Customers are the most important resources for any companies or businesses. What if these customers leave the company due to high charges, better competitor offers, poor customer 
   services or something unknown? Hence, Customer churn rate is one of the important metrics for companies to evaluate their performance.

   Customer churn rate is a KPI to understand the leaving customers. Churn rate represents the percentage of customers that company lost over all the customers at the beginning of the 
   interval.

   For example, If company had 400 customers at the beginning of the month and only 360 at the end of the month, means company’s churn rate is 10%, because company lost 10% of the customer 
   base. Companies always try to minimize the churn rate to as close as 0%.

   This project involved analyzing customer churn for a telecommunications company using MySQL for data cleaning and data preparation, and visualizing key insights using Power BI. The interactive dashboard highlights churn trends, customer demographics, payment methods, and contracts to assist in decision-making and retention strategies.

## Introduction

Dataset has information related to,

**Demographic:**

*Gender* - Male / Female

*Age range* - In terms of Partner, Dependent and Senior Citizen

**Services:**

*Phone service* - If customer has Phone service, then services related to Phone like; Multiline Phone service

*Internet Service* - If customer has Internet service, then services related to Internet like;

*Online security*

*Online backup*

*Device protection*

*Tech support*

*Streaming TV*

*Streaming Movies*

**Account type:**

*Tenure* - How long customer is with the company?

*Contract type* - What kind of contract they have with a company? Like Monthly bases

On going bases - If on going bases, then One month contract or Two year contract

*Paperless billing* - Customer is paperless billion option or not?

*Payment method* - What kind of payment method customer has?

Mailed check

Electronic check

Credit card (Automatic)

Bank transfer (Automatic)

## Key Objectives
   Outline the main goals of your analysis:
   * Identify customer churn rates and retention percentages.
   * Analyze churn patterns by gender, payment methods, and contract types.
   * Present actionable insights to improve customer retention.

## Tools & Techniques

Tools: MySQL, Power BI, DAX 

Techniques: Data cleaning, aggregation, and visualization; creating KPIs and measures; designing interactive dashboards.

### Step 1: Data Cleaning and data preparation using MySQL
  
Check the duplicate customer_id and remove it 
          
*check duplicate customerID*  
```
SELECT 
    customerID, COUNT(customerID)
FROM
    `telco-customer-raw-data`
GROUP BY customerID
HAVING COUNT(customerID) > 1;
```
*Result:*

 ![Picture2](https://github.com/user-attachments/assets/bf819458-3994-4bbd-b3a6-49d239883cb5)



*Remove Duplicate:*
```
WITH CTE AS (
    SELECT 
        customerID,
        ROW_NUMBER() OVER (PARTITION BY customerID) AS row_num
    FROM 
        `telco-customer-raw-data`
)
DELETE FROM `telco-customer-raw-data`
WHERE customerID IN (
    SELECT customerID
    FROM CTE
    WHERE row_num > 1
);
```
*After execution of the above code*

*Result*

![Picture3](https://github.com/user-attachments/assets/a7137726-bbb6-4bae-9ce8-69ff1858de18)

*Update Churn column data (replace 'Yes' to 'Churned' and 'No' to 'Existing' customer*

```
update `telco-customer-raw-data`
set Churn=
case 
    when Churn ='Yes' then 'Churned'
    when Churn ='NO' then 'Existing' 
end ;
```
After that Export updated data and import that file to Power BI for data aggregation and visulaization

### Step 2: Aggregation using DAX, then Data Visualization in Power BI 

*Aggregation Using DAX*

count customer using customerID column
```
Customer = COUNT('Telco-Customer-cleaned-data'[customerID] )
```

count of Existing and Churned customer using *Churn* column
```
Count_Existing_Customer = COUNTROWS(FILTER('Telco-Customer-cleaned-data','Telco-Customer-cleaned-data'[Churn]="Existing"))
```
```
Count_Churned_Customer = COUNTROWS(FILTER('Telco-Customer-cleaned-data','Telco-Customer-cleaned-data'[Churn]="Churned"))
```
Now want to find percentage of churned and Existing customer 

```
Churned_Customer = 'Telco-Customer-cleaned-data'[Count_Churned_Customer]/'Telco-Customer-cleaned-data'[Customer]
```
```
Existing_Customer = 'Telco-Customer-cleaned-data'[Count_Existing_Customer]/'Telco-Customer-cleaned-data'[Customer]
```

Now will be able to visualize the data

### *Final Dashboard*

![Picture4](https://github.com/user-attachments/assets/f5395127-9153-4c9e-999e-1a68fb3f64f0)


#### Features of the Dashboard

##### Total customers and revenue insights

*Total Customers:* The total customer count (7,039) represents the size of the company's customer base, providing a foundation for analyzing churn rates, retention strategies, and market reach.
*Total Revenue:* The sum of total charges (16.05M) reflects the cumulative revenue generated by the customer base, serving as a crucial indicator of the company's financial performance.

##### Churn percentage of Churned and Existing customers
A churn rate of 26.54% highlights the need to identify factors contributing to customer attrition, such as service quality, pricing, or competition, and implement targeted retention strategies to mitigate future churn.

An Existing rate of 73.46% underscores the strength of the existing customer base, indicating that a majority of customers remain loyal to the brand. Enhancing customer satisfaction and building long-term relationships can further improve this metric.

##### Visualizations by gender, payment method, contract type, and phone services.

*Gender:* The distribution of customers is nearly balanced between male (50.49%) and female (49.51%), indicating no significant gender disparity in the customer base.

*Payment Method:* The analysis shows a clear preference for electronic checks among customers, followed by mailed checks, bank transfers, and credit cards. This highlights potential areas for promoting diverse payment options.

*Contract Type:* A majority of customers are on month-to-month contracts, suggesting a need for strategies to encourage longer-term contracts for increased customer retention and revenue stability.

*Phone Services:* Most customers (90.31%) are subscribed to phone services, with a smaller segment (9.69%) opting out. This demonstrates the significance of phone services in the customer portfolio.

##### Average monthly and total charges per contract type.

*Monthly Charges:* Customers on month-to-month contracts have the highest average monthly charges, indicating flexibility comes at a premium. Charges decrease for one-year and two-year contracts, reflecting cost savings for long-term commitments.

*Total Charges:* Two-year contracts contribute the highest total charges due to extended durations, followed by one-year contracts and month-to-month contracts, highlighting the financial benefits of retaining long-term customers.

    


