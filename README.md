# Classic Models Database

This repository contains the Classic Models database, a sample database representing a fictional scale model cars business. The database includes customers, products, orders, payments, and employee information.

## Database Structure

The database consists of the following tables:
- customers - Customer data including name, address, and sales rep
- products - Scale model cars data with product name, vendor, and pricing
- productlines - Product categories with descriptions
- orders - Customer orders with dates and status
- orderdetails - Line items for each order
- payments - Customer payment records
- employees - Employee information including job titles
- offices - Company office locations

## Example SQL Queries

Here are five useful SQL queries for analyzing the Classic Models database:

### 1. Find Total Sales by Product Line

```sql
SELECT productLine, SUM(quantityOrdered * priceEach) AS totalSales
FROM orderdetails
JOIN products ON orderdetails.productCode = products.productCode
GROUP BY productLine
ORDER BY totalSales DESC;
```

This query calculates the total sales amount for each product line, helping identify the most profitable product categories.

### 2. Identify Top Customers by Payment Amount

```sql
SELECT c.customerNumber, c.customerName, SUM(p.amount) AS totalPayment
FROM customers c
JOIN payments p ON c.customerNumber = p.customerNumber
GROUP BY c.customerNumber, c.customerName
ORDER BY totalPayment DESC
LIMIT 10;
```

This query shows the top 10 customers by total payment amount, useful for identifying key accounts.

### 3. Calculate Monthly Order Trends

```sql
SELECT 
    YEAR(orderDate) AS orderYear,
    MONTH(orderDate) AS orderMonth,
    COUNT(*) AS orderCount,
    SUM(quantityOrdered * priceEach) AS monthlyRevenue
FROM orders
JOIN orderdetails ON orders.orderNumber = orderdetails.orderNumber
WHERE status = 'Shipped'
GROUP BY YEAR(orderDate), MONTH(orderDate)
ORDER BY orderYear, orderMonth;
```

This query analyzes monthly ordering patterns, showing both order count and revenue for each month.

### 4. Find Product Inventory Value

```sql
SELECT 
    productLine,
    SUM(quantityInStock) AS totalStock,
    SUM(quantityInStock * buyPrice) AS inventoryValue
FROM products
GROUP BY productLine
ORDER BY inventoryValue DESC;
```

This query calculates the total inventory value for each product line, helping with inventory management.

### 5. Analyze Sales Performance by Employee

```sql
SELECT 
    e.employeeNumber,
    CONCAT(e.firstName, ' ', e.lastName) AS employeeName,
    e.jobTitle,
    COUNT(DISTINCT o.orderNumber) AS orderCount,
    SUM(od.quantityOrdered * od.priceEach) AS salesAmount
FROM employees e
LEFT JOIN customers c ON e.employeeNumber = c.salesRepEmployeeNumber
LEFT JOIN orders o ON c.customerNumber = o.customerNumber
LEFT JOIN orderdetails od ON o.orderNumber = od.orderNumber
GROUP BY e.employeeNumber, employeeName, e.jobTitle
ORDER BY salesAmount DESC;
```

This query analyzes sales performance for each employee, showing the number of orders and total sales amount.

## Additional Information

This database is based on the MySQL Sample Database from [mysqltutorial.org](http://www.mysqltutorial.org/mysql-sample-database.aspx).
