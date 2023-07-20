# SQL questionnaire

## Setup

Import this [fake database content](mysqlsampledatabase.zip) into your database.

Below you will find a list of questions.

Find out the answer to each question using the dummy data in your database.

**Copy this file** (see: copy raw content) and fill in your queries + answer on the given location in each question.

## START !

### 1) How many customers do we have?

```
SELECT COUNT(*) FROM customers;
```

solution: `122 Customers`

### 2) What is the customer number of Mary Young?

```
SELECT customerNumber
FROM customers
WHERE contactLastName = 'Young' AND contactFirstName = 'Mary';
```

solution: `219`

### 3) What is the customer number of the person living at Magazinweg 7, Frankfurt 60528?

```
SELECT customerNumber
FROM customers
WHERE addressLine1 = 'Magazinweg 7' AND city = 'Frankfurt' AND postalCode = '60528';
```

solution: `247`

### 4) If you sort the employees on their last name, what is the email of the first employee?

```
SELECT email
FROM employees
ORDER BY lastName ASC
LIMIT 1;
```

solution: `gbondur@classicmodelcars.com`

### 5) If you sort the employees on their last name, what is the email of the last employee?

```
SELECT email
FROM employees
ORDER BY lastName DESC
LIMIT 1;
```

solution: `gvanauf@classicmodelcars.com`

### 6) What is first the product code of all the products from the line 'Trucks and Buses', sorted first by productscale, then by productname.

```
SELECT productCode
FROM products
WHERE productLine = 'Trucks and Buses'
ORDER BY productScale ASC, productName ASC
LIMIT 1;
```

solution: `S12_4473`

### 7) What is the email of the first employee, sorted on their last name that starts with a 't'?

```
SELECT email
FROM employees
WHERE lastName LIKE 't%'
ORDER BY lastName ASC
LIMIT 1;
```

solution: `lthompson@classicmodelcars.com`

### 8) Which customer (give customer number) payed by check on 2004-01-19?

```
SELECT customerNumber
FROM customers
WHERE customerNumber IN (
    SELECT customerNumber FROM payments
    WHERE paymentDate = '2004-01-19'
);
```

solution: `177`

### 9) How many customers do we have living in the state Nevada or New York?

```
SELECT COUNT(*)
FROM customers
WHERE state IN ('NV', 'NY');
```

solution: `7`

### 10) How many customers do we have living in the state Nevada or New York or outside the united states?

```
SELECT COUNT(*)
FROM customers
WHERE state IN ('NV', 'NY') OR country NOT IN ('USA');
```

solution: `93`

### 11) How many customers do we have with the following conditions (only 1 query needed): - Living in the state Nevada or New York OR - Living outside the USA or the customers and with a credit limit above 1000 dollar?

```
SELECT COUNT(*)
FROM customers
WHERE (state IN ('NV', 'NY') OR country NOT IN ('USA')) AND creditLimit > 1000;
```

solution: `70`

### 12) How many customers don't have an assigned sales representative?

```
SELECT COUNT(*)
FROM customers
WHERE salesRepEmployeeNumber IS NULL;
```

solution: `22`

### 13) How many orders have a comment?

```
SELECT COUNT(*)
FROM orders
WHERE comments IS NOT NULL;
```

solution: `80`

### 14) How many orders do we have where the comment mentions the word "caution"?

```
SELECT COUNT(*)
FROM orders
WHERE comments LIKE '%caution%';
```

solution: `4`

### 15) What is the average credit limit of our customers from the USA? (rounded)

```
SELECT ROUND(AVG(creditLimit))
FROM customers
WHERE country = 'USA';
```

solution: `78103`

### 16) What is the most common last name from our customers?

```
SELECT contactLastName, COUNT(contactLastName) AS count
FROM customers
GROUP BY contactLastName
ORDER BY count DESC
LIMIT 1;
```

solution: `Young with a count of 4`

### 17) What are valid statuses of the orders and numbers?

- [ Y ] Resolved

- [ Y ] Cancelled

- [ N ] Broken

- [ Y ] On Hold

- [ Y ] Disputed

- [ Y ] In Process

- [ ] Processing

- [ Y ] Shipped

```
SELECT status, count(status) AS count
FROM orders
GROUP BY status;

```

solution:
| status | count |
|------------|-------|
| Cancelled | 6 |
| Disputed | 3 |
| In Process | 6 |
| On Hold | 4 |
| Resolved | 4 |
| Shipped | 303 |

### 18) In which countries don't we have any customers? ---CHECK

Check if Austria, Canada, China, Germany, Greece, Japan, Philippines, South Korea are in the list of countries. If they do not exist print them out.

- [y] Austria

- [y] Canada

- [n] China

- [y] Germany

- [n] Greece

- [y] Japan

- [y] Philippines

- [n] South Korea

```
SELECT country
FROM (
    SELECT 'Austria' AS country
    UNION ALL
    SELECT 'Canada'
    UNION ALL
    SELECT 'China'
    UNION ALL
    SELECT 'Germany'
    UNION ALL
    SELECT 'Greece'
    UNION ALL
    SELECT 'Japan'
    UNION ALL
    SELECT 'Philippines'
    UNION ALL
    SELECT 'South Korea'
) AS country_list
WHERE country NOT IN (
    SELECT DISTINCT country
    FROM customers
);


```

solution: `<China, Greece, South Korea>`

### 19) How many orders where never shipped?

```
SELECT COUNT(*)
FROM orders
WHERE status NOT IN ('Shipped');

```

solution: `23`

### 20) How many customers does Steve Patterson have with a credit limit above 100 000 EUR?

```
SELECT COUNT(*)
FROM customers
WHERE salesRepEmployeeNumber = (
    SELECT employeeNumber
    FROM employees
    WHERE lastName = 'Patterson' AND firstName = 'Steve'
) AND creditLimit > 100000;
```

solution: `3`

### 21) How many orders have been shipped to our customers?

```
SELECT COUNT(*)
FROM orders
WHERE customerNumber IN (
    SELECT customerNumber
    FROM customers
);
```

solution: `326`

### 22) How much products does the biggest product line have? And which product line is that?

```
SELECT productLine, COUNT(productLine) AS count
FROM products
GROUP BY productLine
ORDER BY count DESC
LIMIT 1;

```

solution:

| productLine  | count |
| ------------ | ----- |
| Classic Cars | 38    |

### 23) How many products are low in stock? (below 100 pieces)

```
SELECT COUNT(*)
FROM products
WHERE quantityInStock < 100;

```

solution: `2`

### 24) How many products have more the 100 pieces in stock, but are below 500 pieces.

```
SELECT COUNT(*)
FROM products
WHERE quantityInStock > 100 AND quantityInStock < 500;
```

solution: `3`

### 25) How many orders did we ship between and including June 2004 & September 2004?

```
SELECT COUNT(*)
FROM orders
WHERE status = 'shipped' AND shippedDate BETWEEN '2004-06-01' AND '2004-09-30';
```

solution: `42`

### 26) How many customers share the same last name as an employee of ours?

```
SELECT COUNT(*)
FROM customers
WHERE contactLastName IN (
    SELECT lastName
    FROM employees
);
```

solution: `9`

### 27) Give the product code for the most expensive product for the consumer?

```
SELECT productCode
FROM products
WHERE MSRP= (
    SELECT MAX(MSRP)
    FROM products
);
```

solution: `S10_1949`

### 28) What product (product code) offers us the largest profit margin (difference between buyPrice & MSRP).

```
SELECT productCode
FROM products
WHERE MSRP - buyPrice = (
    SELECT MAX(MSRP - buyPrice )
    FROM products
);
```

solution: `S10_1949`

### 29) How much profit (rounded) can the product with the largest profit margin (difference between buyPrice & MSRP) bring us.

```
SELECT ROUND(MSRP - buyPrice)
FROM products
WHERE MSRP - buyPrice = (
    SELECT MAX(MSRP - buyPrice)
    FROM products
);
```

solution: `116`

### 30) Give the product numbers of the products (separated with spaces) that have never been sold? --- CHECK

```
SELECT productCode
FROM products
WHERE productCode NOT IN (
    SELECT productCode
    FROM orderdetails
);
```

solution: `S18_3233`

### 31) How many products give us a profit margin below 30 dollar?

```
SELECT COUNT(*)
FROM products
WHERE MSRP - buyPrice < 30;

```

solution: `23 products`

### 32) What is the product code of our most popular product (in number purchased)?

```
SELECT productCode
FROM products
WHERE productCode IN (
    SELECT productCode
    FROM orderdetails
    GROUP BY productCode
    ORDER BY COUNT(productCode) DESC
);

USE MariaDB does not accept LIMIT in subqueries
```

solution: `S10_1949`

### 33) How many of our popular product did we effectively ship?

```
SELECT COUNT(*)
FROM products
WHERE productCode IN (
    SELECT productCode
    FROM orderdetails
    GROUP BY productCode
    ORDER BY COUNT(productCode) DESC
) AND productCode IN (
    SELECT productCode
    FROM orderdetails
    WHERE orderNumber IN (
        SELECT orderNumber
        FROM orders
        WHERE status = 'shipped'
    )
);
```

solution: `109`

### 34) Which check number paid for order 10210. Tip: Pay close attention to the date fields on both tables to solve this. --- CHECK

```

SELECT checkNumber
FROM payments
WHERE customerNumber IN (
    SELECT customerNumber
    FROM orders
    WHERE orderNumber = 10210
) AND paymentDate = (
    SELECT MAX(paymentDate)
    FROM payments
    WHERE customerNumber IN (
        SELECT customerNumber
        FROM orders
        WHERE orderNumber = 10210
    )
);
```

solution: `AU750837`

### 35) Which order was paid by check CP804873?

```
SELECT orderNumber
FROM orders
WHERE customerNumber IN (
    SELECT customerNumber
    FROM payments
    WHERE checkNumber = 'CP804873'
);

```

solution: `10108, 10198, 10330`

### 36) How many payments do we have above 5000 EUR and with a check number with a 'D' somewhere in the check number, ending the check number with the digit 9?

```
SELECT COUNT(*)
FROM payments
WHERE amount > 5000 AND checkNumber LIKE '%D%9';
```

solution: `3`

### 38) In which country do we have the most customers that we do not have an office in? ---CHECK

```
SELECT country
FROM customers
WHERE country NOT IN (
    SELECT country
    FROM offices
) GROUP BY country
ORDER BY COUNT(country) DESC
LIMIT 1;
```

solution: `Germany`

### 39) What city has our biggest office in terms of employees?

```
SELECT city
FROM offices
WHERE officeCode IN (
    SELECT officeCode
    FROM employees
    GROUP BY officeCode
    ORDER BY COUNT(officeCode) DESC
);

```

solution: `San Francisco`

### 40) How many employees does our largest office have, including leadership?

```
SELECT COUNT(*)
FROM employees
WHERE officeCode IN (
    SELECT officeCode
    FROM offices
    WHERE officeCode IN (
        SELECT officeCode
        FROM employees
        GROUP BY officeCode
        ORDER BY COUNT(officeCode) DESC
    )
);
```

solution: `23`

### 41) How many employees do we have on average per country (rounded)?

```
SELECT ROUND(AVG(count))
FROM (
    SELECT COUNT(employeeNumber) AS count
    FROM employees
    GROUP BY officeCode
) AS count;
```

solution: `3`

### 42) What is the total value of all shipped & resolved sales ever combined?

```
SELECT SUM(quantityOrdered * priceEach)
FROM orderdetails
WHERE orderNumber IN (
    SELECT orderNumber
    FROM orders
    WHERE status IN ('shipped', 'resolved')
);
```

solution: `8999330.52`

### 43) What is the total value of all shipped & resolved sales in the year 2005 combined? (based on shipping date)

```
SELECT SUM(quantityOrdered * priceEach)
FROM orderdetails
WHERE orderNumber IN (
    SELECT orderNumber
    FROM orders
    WHERE status IN ('shipped', 'resolved') AND YEAR(shippedDate) = 2005
);
```

solution: `1427944.97`

### 44) What was our most profitable year ever (based on shipping date), considering all shipped & resolved orders?

```

```

solution: `<your solution here>`

### 45) How much revenue did we make on in our most profitable year ever (based on shipping date), considering all shipped & resolved orders?

```



```

solution: `<your solution here>`

### 46) What is the name of our biggest customer in the USA of terms of revenue?

```
SELECT customerName
FROM customers
WHERE customerNumber IN (
    SELECT customerNumber
    FROM orders
    WHERE status IN ('shipped', 'resolved') AND customerNumber IN (
        SELECT customerNumber
        FROM customers
        WHERE country = 'USA'
    ) GROUP BY customerNumber
    ORDER BY SUM('quantityOrdered' * 'priceEach') DESC
);

```

solution: `Signal Gift Stores`

### 47) How much has our largest customer inside the USA ordered with us (total value)?

```


```

solution: `your solution here`

### 48) How many customers do we have that never ordered anything?

```
SELECT COUNT(*)
FROM customers
WHERE customerNumber NOT IN (
    SELECT customerNumber
    FROM orders
);
```

solution: `24`

### 49) What is the last name of our best employee in terms of revenue?

```
SELECT lastName
FROM employees
WHERE employeeNumber IN (
    SELECT employeeNumber
    FROM orders
    WHERE status IN ('shipped', 'resolved') AND employeeNumber IN (
        SELECT employeeNumber
        FROM employees
    ) GROUP BY employeeNumber
    ORDER BY SUM('quantityOrdered' * 'priceEach') DESC
);
```

solution: `<Murphy>`

### 50) What is the office name of the least profitable office in the year 2004?

```
SELECT city
FROM offices
WHERE officeCode IN (
    SELECT officeCode
    FROM employees
    WHERE employeeNumber IN (
        SELECT employeeNumber
        FROM orders
        WHERE status IN ('shipped', 'resolved') AND YEAR(shippedDate) = 2004
    ) GROUP BY officeCode
    ORDER BY SUM(quantityOrdered * priceEach) ASC
);
```

solution: `San Francisco`

## Are you done? Amazing!

![](../_assets/clap-clap-clap.gif)
