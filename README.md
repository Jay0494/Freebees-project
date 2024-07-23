# **Customer Churn Insights with SQL - Chinook Database**
## **Introduction**
Hi there! I’m excited to share with you a project that was both a challenging and rewarding experience for me: analyzing customer churn using the Chinook database. This project allowed me to dive deep into SQL and apply my data analysis skills to uncover actionable insights about customer behavior and retention.


**Project Highlights:**
- **Purpose:** I set out to understand why customers were churning and identify patterns that could help improve retention strategies as well as other tasks.
- **Database:** The Chinook database served as a fantastic learning tool, providing a realistic dataset to work with.
- **Tools:** SQL for data exploration, cleaning, and optimization, combined with analytical thinking to derive meaningful insights.

## **Project Overview**

### **1. Data Exploration and Preparation**

The journey began with a thorough exploration of the Chinook database:
- I reviewed the database schema to grasp the relationships between tables and the nature of the data.
- By analyzing data, I got to know the data better and assessed its quality.
- I tackled missing values, duplicates, and consistency issues to ensure the data was ready for in-depth analysis.

### **2. Data Optimization**

To ensure that the analysis was as efficient as possible:
- I crafted SQL queries to be both effective and efficient, paying close attention to execution plans.
- I refined queries to handle large datasets smoothly and deliver results faster.

### **3. Analysis and Insights**
The heart of the project is:
-  Identifying customers at risk of churning.

  ## INSIGHTS 
![Task 1](https://github.com/user-attachments/assets/3670214e-534c-4e74-b8f4-6010f06c6a42)
![Task 2](https://github.com/user-attachments/assets/1ab88d6e-e9be-4600-9de4-de5660a98b82)
![Task 4](https://github.com/user-attachments/assets/560df49c-dce2-4b1d-848a-60136cfdf668)
![Task 5](https://github.com/user-attachments/assets/89493f69-c258-403e-b5d2-814b2e0a2471)
![PROBLEM STATEMENT ](https://github.com/user-attachments/assets/a9775043-d2d5-4e2c-a531-2162ada3d98b)

## STEPS TO RESOLVE CUSTOMER CHURN 
- Keep customers informed about new features, updates, and company news.
- Interact with customers on social media platforms to build a sense of community.
- Encourage satisfied customers to become brand advocates and refer others.
- Use Customer Relationship Management (CRM) tools to manage and analyze customer interactions and data.
- Employ predictive analytics to anticipate churn and take preemptive actions.
- Automate routine customer engagement tasks to ensure timely and consistent communication.
- Use data to predict potential issues and address them before they lead to churn.
- Establish channels for ongoing customer feedback to address concerns in real-time.

## **SQL CODE**
```sql
## Data preparation


## FORMAT DATE COLUMN 
ALTER TABLE invoice
MODIFY COLUMN InvoiceDate Date;

##REMOVE SPECIAL CHARACTERS 
SELECT
    REGEXP_REPLACE(FirstName, '[^a-zA-Z]', '') AS First_Name,
    REGEXP_REPLACE(LastName, '[^a-zA-Z]', '') AS Last_Name
FROM customer;

UPDATE customer
SET
    FirstName = REGEXP_REPLACE(FirstName, '[^a-zA-Z]', ''),
    LastName = REGEXP_REPLACE(LastName, '[^a-zA-Z]', '');


## remove nulls and drop unneccesary columns 
ALTER TABLE customer
DROP COLUMN Company,
DROP COLUMN State,
DROP COLUMN PostalCode,
DROP COLUMN Fax,
DROP COLUMN Email,
DROP COLUMN Address; 

##clean invoice dataset
SELECT *  
FROM invoice;

ALTER TABLE invoice
DROP COLUMN BillingAddress,
DROP COLUMN BillingState,
DROP COLUMN BillingPostalCode;

## clean playlist dataset 
SELECT *
FROM track;

SELECT
* FROM genre;

## DROP COLUMN
ALTER TABLE track
DROP COLUMN Composer,
DROP COLUMN Bytes,
DROP COLUMN Milliseconds;

## MOVING FORWARD LETS DEAL WITH THE TASKS 

#TASK 1 Find the top 5 customers who spent the most
SELECT CONCAT(FirstName, ' ', LastName) AS TOP_five_customers
FROM customer
INNER JOIN invoice
ON customer.CustomerId = invoice.CustomerId
ORDER BY Total Desc
LIMIT 5;



##TASK 2: list all invoices and the name of the customer who placed them 
SELECT CONCAT(customer.FirstName, ' ', customer.LastName) AS Customer_name, invoice.InvoiceId
FROM customer 
INNER JOIN invoice ON
customer.CustomerId = invoice.CustomerId;



##task 3: top 3 tracks with the highest unit price 
SELECT `Name` AS Track_Name
FROM track
ORDER BY UnitPrice DESC
LIMIT 3;


## task 4; total sales for each country 
SELECT DISTINCT(customer.Country), (SUM(ROUND(invoice.Total))) AS Total_sales
FROM customer
INNER JOIN invoice
ON customer.CustomerId = invoice.CustomerId
GROUP BY customer.Country
ORDER BY Total_sales DESC;

## PROBLEM STATEMENT : FIND THE CUSTOMERS AT RISK OF CHURN
WITH CustomersWithoutRecentPurchases AS (
    SELECT c.CustomerId, CONCAT(c.FirstName, ' ', c.LastName) AS Customer_Name
    FROM customer c
    LEFT JOIN invoice i 
    ON c.CustomerId = i.CustomerId
    AND i.InvoiceDate >= DATE_SUB('2013-12-22',INTERVAL 6 MONTH)
    WHERE i.CustomerId IS NULL
)
SELECT * FROM CustomersWithoutRecentPurchases;
```
