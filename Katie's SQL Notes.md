# 📝 Katie's SQL Notes

Hi, I'm Katie! This is my work-in-progress notes on SQL. 

## 📚 Table of Content
- Create, alter, drop, truncate and delete database 
- Create, drop and delete index
- Alter table and column
- Set primary and foreign key
- Data types	
  - Text
  - Numerical
  - Date time
- Base query
- Filter techniques
  - Using WHERE
  - Limit results with TOP
  - Remove duplicates with DISTINCT
  - Comparison operators
  - NULL values
  - Match text with LIKE and wildcards
- JOINS
  - Inner Joins
  - Left Joins
  - Right Joins
  - Full Outer Joins
- Grouping records
  - GROUP BY, COUNT and HAVING
- Functions
  - Aggregate functions
  - String functions
  - Mathematical functions
  - Date functions
- Window functions
- Condition statement
  - IIF
  - If Else
  - CASE WHEN
- Create temp table
- Subquery
- PIVOT
- Create and use variables
- Result set operators
  - Combine results with UNION
  - Return distinct rows with EXCEPT
  - Return common rows with INTERSECT
 
***

## 📌 Data Types

***

## 📌 Basics

***

## 📌 Filtering Techniques

### Using WHERE

***

### Limit Results with TOP

**1. Limit results with TOP**
````sql
SELECT TOP 3 TaxRate
FROM Sales.SalesTaxRate
ORDER BY TaxRate DESC;
````
***

**Limit results with TOP PERCENT**

Specify the number of percentage of results to generate. For example, to generate the first 50% of the results, use `TOP 50 PERCENT`. 

````sql
SELECT TOP 50 PERCENT TaxRate, Name
FROM Sales.SalesTaxRate
ORDER BY TaxRate DESC;
````
***

**Limit results with TOP X WITH TIES**

Results include multiple records of the same values from the last record. 

For example, we are interested to know the Top 5 students in the classroom. Since there are 2 students who have received the same 5th highest score in the classroom, hence there will be a total of 6 rows of records in the results table - 1, 2, 3, 4, 5, 5.

````sql
SELECT TOP 5 WITH TIES student_name, score
FROM classroom
ORDER BY score;
````

***

### Remove duplicates with DISTINCT

To remove duplicates and retrieve unique values only.

````sql
SELECT DISTINCT City, StateProvinceID
FROM Person.Address
ORDER BY City;
````

***

### Comparison operators

| Comparison Operator | Description |
| ------------------- | ----------- |
| =                   | Equal to    |
| !=                  | Not equal to    |
| <>                  | Not equal to   |
| >                   | Greater than    |
| >=                  | Greater than or equal to    |
| <                   | Less than    |
| <=                  | Less than or equal to    |

***

**NULL Values**

Take note that we cannot use `!=` or `<>` on NULL values.

````sql
SELECT WorkOrderID, ScrappedQty, ScrapReasonID
FROM Production.WorkOrder
WHERE ScrapReasonID IS NOT NULL;
````
***

**ISNULL**

For example, replace NULL values with '99'.
````sql
SELECT WorkOrderID, ScrappedQty, ISNULL(ScrapReasonID, 99) AS ScrapReason
FROM Production.WorkOrder;
````

***

**Match texts using LIKE and Wildcards**

````sql
WHERE first_name LIKE 'a%' -- Finds any values that starts with "a"
WHERE first_name LIKE '%a' -- Finds any values that ends with "a"
WHERE first_name LIKE '%ae%' -- Finds any values that have "ae" in the middle
WHERE first_name LIKE '_b%' -- Finds any values with "b" in the second position
WHERE first_name LIKE 'a_%_%' -- Finds any values that starts with "a" and are at least 3 characters in length
WHERE first_name LIKE 'a%o' -- Finds any values that starts with "a" and ends with "o"
WHERE first_name LIKE 'a___' -- Finds any value that starts with "a" and has 3 characters
WHERE first_name LIKE '[abc]%' -- Finds any values with "a", "b" or "c"
WHERE first_name LIKE '[a-f]%' -- Finds any values with "a" to "f"
WHERE first_name LIKE 'a[l-n]' -- Finds any values that starts with "a" and has "l", "m" or "n"
WHERE first_name LIKE 'a[c-e]__' -- Finds any values that starts with "a" and has "c", "d" or "e" in the middle and ends with 2 characters
````

***

## 📌 JOINS

### Inner Joins

````sql
SELECT p.BusinessEntityID, p.FirstName, p.LastName, pp.PhoneNumber
FROM Person.Person AS p
JOIN Person.PersonPhone AS pp
	ON p.BusinessEntityID = pp.BusinessEntityID;
````  

***

### Left Joins

````sql
SELECT p.BusinessEntityID, p.PersonType, p.FirstName, p.LastName, e.JobTitle
FROM Person.Person AS p
LEFT JOIN HumanResources.Employee AS e
	ON p.BusinessEntityID = e.BusinessEntityID;
````
***

### Right Joins

````sql
SELECT p.BusinessEntityID, p.PersonType, p.FirstName, p.LastName, e.JobTitle
FROM Person.Person AS p
RIGHT JOIN HumanResources.Employee AS e
	ON p.BusinessEntityID = e.BusinessEntityID;
````

***
### Cross Joins

For example, table_1 has 10 rows and table_2 has 5 rows. A `CROSS JOIN` would result in 10 rows x 5 rows = 50 rows table.

````sql
SELECT d.Name AS DepartmentName, a.Name AS AddressName
FROM HumanResources.Department AS d
CROSS JOIN Person.AddressType AS a;
````

***

## 📌 Grouping records

### GROUP BY and COUNT

Every column in `GROUP BY` clause needs to be in `SELECT` clause.

````sql
SELECT City, StateProvinceID, COUNT(*) AS AddressCount
FROM Person.Address
GROUP BY City, StateProvinceID
ORDER BY AddressCount DESC;
````

***
### GROUP BY and HAVING

`HAVING` must be used in conjuction with `GROUP BY`.

````sql
SELECT City, StateProvinceID, COUNT(*) AS AddressCount
FROM Person.Address
GROUP BY City, StateProvinceID
HAVING City = 'New York'
````

***

## 📌 Functions

### Aggregate Functions

| Aggregate Functions     | Description                 |
| ----------------------- | --------------------------- |
| COUNT(*)                | Count total number of rows  |
| COUNT(DISTINCT column)  | Count unique values only    |
| COUNT_BIG()             |                             |
| SUM, AVG, MIN, MAX()    | Self-explanatory            |
| STDEV, VAR, VARP()      | Self-explanatory            |


***
### String Functions

| String Functions  | Description                 |
| ----------------- | --------------------------- |
| UPPER()           | Convert 'NaMe' into UPPERCAESE  |
| LOWER()           | Convert 'NaMe' into lowercase   |
| LEN()             | Count number of characters in a value                            |
| TRIM()            | Trim blank spaces in a value            |
| LEFT(,3)         | Return 1st 3 characters from the left            |
| RIGHT(,3)        | Returns 1st 3 characters from the right           |
| CONCAT()          | Combine 2 or more values          |
| CONCAT_WS()       | To insert ' ' (blank space/-) between all values          |

Refer to examples below.

````sql
SELECT FirstName, LastName, 
	UPPER(FirstName) AS FName, LOWER(LastName) AS LName, 
	LEN(FirstName) AS LenFName,
	LEFT(LastName, 3) AS First3, RIGHT(LastName,3) AS Last3,
	TRIM(LastName) AS TrimmedName
FROM Person.Person;
````

````sql
SELECT FirstName, LastName,
	CONCAT(FirstName, ' ', MiddleName, ' ', LastName) AS FullName,
	CONCAT_WS(' ', FirstName, MiddleName, LastName) AS FullNameWS -- 'WS' stands for 'With Separator'
FROM Person.Person;
````
***

### Mathematical Functions

| Mathematical Functions  | Description                 |
| ----------------- | --------------------------- |
| ROUND(,2)           | Round with 2 decimals  |
| ROUND(,-2           | Round up to nearest hundreds   |
| CEILING()           | Round up to nearest integer                           |
| FLOOR()            | Round down to nearest integer            |

````sql
SELECT BusinessEntityID, SalesYTD,
	ROUND(SalesYTD, 2) AS Round2, -- 2 decimals
	ROUND(SalesYTD, -2) AS Round100, -- Round up to nearest hundreds
	CEILING(SalesYTD) AS RoundCeiling, -- Round up to nearest integer
	FLOOR(SalesYTD) AS RoundFloor -- Round down to nearest integer
FROM Sales.SalesPerson;
````
***
### Date Functions

| Date Functions  | Description                 |
| ----------------| --------------------------- |
| YEAR()          | Returns year from date  |
| MONTH()         | Returns month from date   |
| DAY()           | Returns day from date                           |
| GETDATE()       | Returns today's date           |
| GETDATEUTC()    | Returns today's date in            |
| DATEDIFF()      |             |
| FORMAT()        |             |
| FORMAT()        |             |


***

## 📌 Window functions

***

## 📌 Condition statement

### IIF 

````sql
SELECT IIF (SalesYTD > 2000000, 'Met sales goal', 'Has not met goal') AS Status,
	COUNT(*)
FROM Sales.SalesPerson
GROUP BY IIF (SalesYTD > 2000000, 'Met sales goal', 'Has not met goal');
````

### If Else

### CASE WHEN

***

## 📌 Create Temp Table

***
## 📌 Subquery

### Subquery in SELECT
````sql
SELECT BusinessEntityID, SalesYTD, 
	(SELECT MAX(SalesYTD) 
	FROM Sales.SalesPerson) AS HighestSalesYTD,
	(SELECT MAX(SalesYTD) 
	FROM Sales.SalesPerson) - SalesYTD AS SalesGap
FROM Sales.SalesPerson
ORDER BY SalesYTD DESC
````

### Subquery in HAVING
````sql
SELECT SalesOrderID, SUM(LineTotal) AS OrderTotal
FROM Sales.SalesOrderDetail
GROUP BY SalesOrderID
HAVING SUM(LineTotal) > 
	(
	SELECT AVG(ResultTable.MyValues) AS AvgValue -- subsquery no. 2. Cannot combine both subqueries as cannot do AVG(SUM(LineTotal))
	FROM
		(SELECT SUM(LineTotal) AS MyValues --subquery no. 1
		FROM Sales.SalesOrderDetail
		GROUP BY SalesOrderID) AS ResultTable
	);
````

### Correlated Subquery

````sql
SELECT BusinessEntityID, FirstName, LastName,
	(SELECT JobTitle
	FROM HumanResources.Employee
	WHERE BusinessEntityID = MyPeople.BusinessEntityID) AS JobTitle --Better to use LEFT JOIN as give same results
FROM Person.Person AS MyPeople
WHERE EXISTS (SELECT JobTitle
		FROM HumanResources.Employee
		WHERE BusinessEntityID = MyPeople.BusinessEntityID); -- Better to use JOIN or WHERE EXISTS

````

***

## 📌 PIVOT

````sql
SELECT 'AverageListPrice' AS 'ProductLine', -- To add meaning to the table
FROM
	(SELECT ProductLine, ListPrice
	FROM Production.Product) AS SourceData
PIVOT (AVG(ListPrice) FOR ProductLine IN (M, R, S, T)) AS PivotTable; -- For every product line M, R, S, T we are going to find the average price
````

***

## 📌 Create and Use Variables

### Using DECLARE

````sql
DECLARE @MyFirstVariable INT;

SET @MyFirstVariable = 10;

SELECT @MyFirstVariable AS MyValue, 
	@MyFirstVariable * 5 AS Multiplication,
	@MyFirstVariable + 10 AS Addition


DECLARE @VarColor VARCHAR(20) = 'Blue';

SELECT ProductID, Name, ProductNumber, Color, ListPrice
FROM Production.Product
WHERE Color = @VarColor;
````

### Create counter for Looping Statement
````sql
DECLARE @Counter INT = 1;

WHILE @Counter <= 3
BEGIN
	SELECT @Counter AS CurrentValue
	SET @Counter = @Counter + 1
END;
````

````sql
DECLARE @Counter INT = 1;
DECLARE @Product INT = 710;

WHILE @Counter <= 3
BEGIN
	SELECT ProductID, Name, ProductNumber, Color, ListPrice
	FROM Production.Product
	WHERE ProductID = @Product
	SET @Counter = @Counter + 1
	SET @Product = @Product + 10
END;
````
***
## 📌 Result set operators

### Combine results with UNION

````sql
SELECT ProductCategoryID, NULL AS ProductSubCategoryID, Name -- Create an empty column to represent ProductSubCategoryID column from 2nd table. Both table data types must be same.
FROM Production.ProductCategory
UNION
SELECT ProductCategoryID, ProductSubCategoryID, Name
FROM Production.ProductSubcategory;
````

***

### Return distinct rows with EXCEPT

````sql
SELECT BusinessEntityID
FROM Person.Person
WHERE PersonType <> 'EM' -- Select everyone who is not an employee
EXCEPT
SELECT BusinessEntityID
FROM Sales.PersonCreditCard; -- Everyone with credit card
````

You can achieve the same results as above using a `LEFT JOIN` below.

````sql
SELECT p.BusinessEntityID
FROM Person.Person AS p
LEFT JOIN Sales.PersonCreditCard AS c --Same results using LEFT JOIN
	ON p.BusinessEntityID = c.BusinessEntityID
WHERE p.PersonType <> 'EM' AND c.CreditCardID IS NULL;
````

***

### Return common rows with INTERSECT

````sql
SELECT ProductID
FROM Production.ProductProductPhoto
INTERSECT
SELECT ProductID
FROM Production.ProductReview;
````


You can achieve the same results as above using `JOIN` below.

````sql
SELECT DISTINCT(p.ProductID) -- Same results using JOIN
FROM Production.ProductProductPhoto AS p
JOIN Production.ProductReview AS r
	ON p.ProductID = r.ProductID;
````
***
