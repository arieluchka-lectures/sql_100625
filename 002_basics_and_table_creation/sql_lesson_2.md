
# SQL Lesson 2: Basics and Table Creation

## Query Modifiers

### LIMIT
Shows only the first N rows that were returned.

```sql
SELECT *
FROM Orders
LIMIT 10;
```

### DISTINCT
Shows all the **UNIQUE** values (removes duplicates).

**Single column:**
```sql
SELECT DISTINCT Country
FROM Customers;
```

**Multiple columns:**
```sql
SELECT DISTINCT Country, City
FROM Customers
ORDER BY Country;
```

### BETWEEN
Filters values within a range (inclusive).

**Instead of this:**
```sql
SELECT * 
FROM Products
WHERE UnitPrice >= 10 AND UnitPrice <= 20;
```

**Use this:**
```sql
SELECT * 
FROM Products
WHERE UnitPrice BETWEEN 10 AND 20;
```

**Example with dates:**
```sql
SELECT * 
FROM Orders
WHERE OrderDate BETWEEN "2016-01-01" AND "2016-12-31"
ORDER BY OrderDate;
```

### IN
Checks if a value matches any value in a list.

**Instead of this:**
```sql
SELECT * 
FROM Customers
WHERE Country = "Mexico"
   OR Country = "UK"
   OR Country = "Sweden";
```

**Use this:**
```sql
SELECT * 
FROM Customers
WHERE Country IN ("Sweden", "UK", "Mexico")
ORDER BY Country;
```






---

## Practice Exercises - Query Modifiers

### Exercise 1
Get the name and unit price of the top 5 most expensive products.

<details>
<summary>Solution</summary>

```sql
SELECT ProductName, UnitPrice 
FROM Products
ORDER BY UnitPrice DESC
LIMIT 5;
```
</details>

### Exercise 2
Find the first 3 employees hired by the company. Return their first name, last name, and hire date.

<details>
<summary>Solution</summary>

```sql
SELECT FirstName, LastName, HireDate
FROM Employees
ORDER BY HireDate
LIMIT 3;
```
</details>

### Exercise 3
Get a list of all the different job titles held by employees (no duplicates).

<details>
<summary>Solution</summary>

```sql
SELECT DISTINCT Title
FROM Employees;
```
</details>

### Exercise 4
Select all customers who are not from the 'USA' or 'UK'. Return their company name and country.

<details>
<summary>Solution</summary>

**Using NOT IN:**
```sql
SELECT CompanyName, Country
FROM Customers
WHERE Country NOT IN ("USA", "UK");
```

**Alternative solution:**
```sql
SELECT CompanyName, Country
FROM Customers
WHERE Country != "USA"
  AND Country != "UK";
```
</details>

### Exercise 5
Get a list of unique supplier IDs who have products currently running low on stock (between 3 and 10 items).

<details>
<summary>Solution</summary>

```sql
SELECT DISTINCT SupplierID
FROM Products
WHERE UnitsInStock BETWEEN 3 AND 10;
```
</details>

---

## Aggregate Functions

Aggregate functions perform calculations on a set of values and return a single value.

### Available Functions
- `MIN()` - Returns the minimum value
- `MAX()` - Returns the maximum value
- `AVG()` - Returns the average value
- `SUM()` - Returns the sum of values
- `COUNT()` - Returns the count of values

### Examples

**Average price:**
```sql
SELECT AVG(UnitPrice) AS average_price_of_a_product
FROM Products;
```

**Minimum price:**
```sql
SELECT MIN(UnitPrice) AS lowest_price_of_a_product
FROM Products;
```

**Maximum price:**
```sql
SELECT MAX(UnitPrice) AS highest_price_of_a_product
FROM Products;
```

**Sum of units in stock:**
```sql
SELECT SUM(UnitsInStock)
FROM Products;
```

**Count rows:**
```sql
SELECT COUNT(*)
FROM Customers
WHERE Country = "Germany";
```

**Count non-null values:**
```sql
SELECT COUNT(Fax)
FROM Customers;
```

### GROUP BY

Groups rows that have the same values in specified columns.

**Basic grouping:**
```sql
SELECT City, COUNT(*)
FROM Customers
GROUP BY City;
```

**Group with HAVING clause:**
```sql
SELECT ShipCountry, COUNT(*) AS number_of_orders
FROM Orders
GROUP BY ShipCountry
HAVING COUNT(*) > 1000
ORDER BY COUNT(*) DESC;
```

> **Note:** `HAVING` is used to filter groups (after `GROUP BY`), while `WHERE` filters individual rows (before grouping).


---

## Practice Exercises - Aggregations

### Exercise 1
Calculate the average freight for all orders.

<details>
<summary>Solution</summary>

```sql
SELECT AVG(Freight) AS average_freight
FROM Orders;
```
</details>

### Exercise 2
Count how many orders each customer placed in the year 2014.

<details>
<summary>Solution</summary>

```sql
SELECT CustomerID, COUNT(*) AS order_count
FROM Orders
WHERE OrderDate BETWEEN "2014-01-01" AND "2014-12-31"
GROUP BY CustomerID;
```
</details>

### Exercise 3
Count how many orders had a discount applied to them (use the `OrderDetails` table).

<details>
<summary>Solution</summary>

```sql
SELECT COUNT(*) AS orders_with_discount
FROM OrderDetails
WHERE Discount > 0;
```
</details>

### Exercise 4
Group orders by discount amount to see how many had each discount level (0.05, 0.10, etc.).

<details>
<summary>Solution</summary>

```sql
SELECT Discount, COUNT(*) AS order_count
FROM OrderDetails
WHERE Discount > 0
GROUP BY Discount
ORDER BY Discount;
```
</details>

---

## JOINS

Joins combine rows from two or more tables based on a related column.

### Basic Syntax
```sql
SELECT *
FROM <TABLE1>
JOIN <TABLE2>
ON TABLE1.SHARED_COLUMN = TABLE2.SHARED_COLUMN;
```

### INNER JOIN
Returns only rows where there is a match in both tables.

**Connect Customers to Orders:**
```sql
SELECT *
FROM Orders
JOIN Customers
ON Orders.CustomerID = Customers.CustomerID;
```

**Filter joined data:**
```sql
SELECT *
FROM Orders
JOIN Customers
ON Orders.CustomerID = Customers.CustomerID
WHERE Customers.CustomerID = "AGRAN";
```

**Select specific columns:**
```sql
SELECT Orders.OrderID, Orders.EmployeeID, Customers.ContactName, Customers.Phone
FROM Orders
JOIN Customers
ON Orders.CustomerID = Customers.CustomerID;
```

### JOIN Practice Example
Get a list of all orders with the employee ID, customer name, and phone number.

<details>
<summary>Solution</summary>

```sql
SELECT Orders.OrderID, Orders.EmployeeID, Customers.ContactName, Customers.Phone
FROM Orders
JOIN Customers
ON Orders.CustomerID = Customers.CustomerID;
```
</details>

---


## Table Creation and Management

### CREATE TABLE

Basic syntax for creating a new table:

```sql
CREATE TABLE <TABLE_NAME> (
	COLUMN1 TYPE1,
	COLUMN2 TYPE2,
	COLUMN3 TYPE3
);
```

### SQLite Data Types

SQLite has 5 main data types:
- **TEXT** - String values
- **INTEGER** - Whole numbers
- **REAL** - Floating point numbers (decimals)
- **NULL** - Null values
- **BLOB** - Binary Large Object (files, images, etc.)

### Constraints

Constraints enforce rules on table columns:

- **NOT NULL** - Column cannot have NULL values
- **UNIQUE** - All values in the column must be unique
- **DEFAULT** - Sets a default value if none is provided
- **PRIMARY KEY** - Uniquely identifies each row
- **FOREIGN KEY** - Links to a PRIMARY KEY in another table
- **CHECK** - Ensures values meet a specific condition

### CREATE TABLE with Constraints

```sql
CREATE TABLE Students (
	student_id INTEGER PRIMARY KEY,
	first_name TEXT DEFAULT "student_name",
	last_name TEXT NOT NULL,
	phone TEXT UNIQUE
);
```

### INSERT INTO

Add data to a table:

```sql
INSERT INTO Students(student_id, first_name, last_name, phone)
VALUES (1, "John", "Doe", "052-1234567");
```

**Partial insert (using defaults):**
```sql
INSERT INTO Students(student_id, phone)
VALUES (2, "052-12345678");
```

### DROP TABLE

Delete an entire table:

```sql
DROP TABLE <TABLE_NAME>;
```

**Example:**
```sql
DROP TABLE Students;
```

> **Warning:** `DROP TABLE` permanently deletes the table and all its data!



