# Freebees SQL project
INTRODUCTION







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

SELECT * FROM genre;


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
