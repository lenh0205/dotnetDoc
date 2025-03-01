## index
* an **`index`** is a `on-disk structure` associated with a table or view that `speeds retrieval of rows` from the table or view
* An index contains `keys` built from one or more columns in the table or view. 
* These keys are stored in a structure (B-tree) -> find the row or rows associated with the key values quickly and efficiently

### Mechanism
* avoid scanning the entire table when executing queries with search conditions
* can `directly access the index` to locate the relevant rows
* The index enables the database engine to locate and retrieve the required data more efficiently
* especially for large tables or frequently queried tables

### Trace-off
* enhance read performance but slightly slow down write operations (such as INSERT, UPDATE, and DELETE)
* because the `index needs to be updated along with the table`
* Additionally, indexes consume storage space, so creating too many unnecessary indexes can negatively impact overall database performance

### Structure of An index
* **`Search Key column`**: It contains a `copy of the values` from the `indexed column(s)` of the table, organized in a `specific order` -> quick lookup and searching
* **`Data Reference column`**: It contains a `pointer or reference` to the physical location where the `corresponding record(s)` with the indexed column value(s) are stored -> efficient retrieval of the data associated with the index

### Most commonly used types of indexes in SQL
* are **`B-tree indexes`** and **`hash indexes`**
* Other types of indexes: bitmap indexes, full-text indexes, spatial indexes, may also exist depending on DBMS and specific use case

#### B-tree index
* organizes data in a **`balanced tree structure`** and is efficient for range-based queries
* each node contains multiple keys and pointers to child nodes

* are widely used in relational databases and support efficient data insertion and deletion

#### Hash index
* uses a **`hash function`** to map keys to index entries and provides direct access to data based on the indexed value. 
* Hash indexes are fast for exact match queries but may not be efficient for range-based queries or partial matching

### Most commonly used types of indexes in SQL Server
* SQL Server `primarily uses B-tree indexes`, but it also supports a specialized form of a `hash index` called a `hash partitioned index`
* Clustered Index, Non-clustered Index, and Unique Index,... are implementations of the B-tree index structure

#### Clustered Index: 
* **`sort`** and store the data rows in the table (or view) based on their `key values` (_the columns included in the index definition_)

* Each table can have `only one` clustered index (_because the data rows themselves can be stored in only one order_)
* If a table has no clustered index, its data rows are stored in an unordered structure called a **`heap`**
* `By default`, primary key constraints create clustered indexes
```
CREATE CLUSTERED INDEX IX_Employee_ID ON Employee (EmployeeID);
```
#### Non-Clustered Index: 
*  have a structure `separate` from the data rows. 
* A nonclustered index contains the `nonclustered index key values`
* each _key value entry_ has a **`pointer to the data row`** that contains the key value

* The pointer from an index row in a nonclustered index to a data row is called a **`row locator`**
* A table can have `multiple` non-clustered indexes to improve the performance of different queries
```
CREATE NONCLUSTERED INDEX IX_Employee_LastName ON Employee (LastName);
```
####  Unique Index: 
* enforces the uniqueness of values in the indexed column or columns. 
* It is similar to a non-clustered index, but it **`disallows duplicate values`** in the indexed columns
```
CREATE UNIQUE INDEX IX_Employee_Email ON Employee (Email);
```
#### Filtered Index: 
* a specialized type of index that contains a `subset of rows` based on a `filter condition`.
* It can improve query performance when the filter condition is frequently used
```
CREATE INDEX IX_Employee_Active ON Employee (EmployeeID) WHERE IsActive = 1;
```

## 6. ACID
* _ACID is a set of properties that **define how transactions should behave to prevent data corruption, inconsistencies, and concurrency issues**_
* -> **`Atomicity`** - Everything happens fully or not at all.
* -> **`Consistency`** - The database stays correct before and after the transaction.
* -> **`Isolation`** - Transactions don’t interfere with each other.
* -> **`Durability`** - Once saved, data is permanent, even if the system crashes

## 5. Transaction
* -> a **logical unit of work** in a database that **consists of one or more operations**
* -> ensures that either **`all operations succeed or none are applied`**, maintaining **data integrity**
* -> transaction reply on **`ACID`** to ensure that data remains **accurate, reliable, and consistent**

## 3. Transaction Isolation Level
* -> since multiple users run transactions at the same time, SQL provides **isolation levels** to **`control how transactions affect each other`**
* https://blog.vietnamlab.vn/transaction-isolation-levels-trong-dbms/
* https://www.geeksforgeeks.org/transaction-isolation-levels-dbms/
* https://learn.microsoft.com/vi-vn/sql/t-sql/language-elements/transaction-isolation-levels?view=sql-server-2017
* https://www.cockroachlabs.com/blog/sql-isolation-levels-explained/
* https://www.metisdata.io/blog/transaction-isolation-levels-and-why-we-should-care

### the main isolation levels
* -> **`READ UNCOMMITTED`** (the lowest level) - transactions can read uncommitted data (risky!)
* -> **`READ COMMITTED`** (default in SQL Server) transactions only see committed data (safer)
* -> **`REPEATABLE READ`** - prevents changes to rows that a transaction is reading
* -> **`SERIALIZABLE`** (the strictest level) - prevents new inserts or changes until a transaction is done

## 2. a 'Lock'
* -> is a way **SQL Server protects data** when **`multiple users are reading/writing at the same time`**

* -> **`Shared Lock (S)`** - used when reading data (so other users can also read, but not change it)
* -> **`Exclusive Lock (X)`** - used when modifying data (no one else can read or write)
* -> **`Update Lock (U)`** - used before updating a row

## 4. Table Hint
* -> a special type of explicit command that is used to **`override the default behavior of the SQL Server query optimizer`** during the **T-SQL query execution**
* _one example is WITH (NOLOCK)_

### Mechanism
* -> accomplished by **`enforcing a specific locking method, a specific index or query processing operation`** (_such index seek or table scan_)
* -> to be used by the **`SQL Server query optimizer`** to build the **query execution plan**
* -> the table hints can be added to the FROM clause of the T-SQL query, affecting the table or the view that is referenced in the FROM clause only

## 1. 'WITH(NOLOCK)' in SQL Server
* _https://www.sqlshack.com/understanding-impact-clr-strict-security-configuration-setting-sql-server-2017/_
* _https://www.sqlshack.com/understanding-the-impact-of-nolock-and-with-nolock-table-hints-in-sql-server/_
* -> nó là 1 **`table hint`** (_được sử dụng nhiều hơn cả trong **SELECT T-SQL statements**_) allowing the user to retrieve the data without being affected by the locks due to another process that is changing it
* _nói đúng hơn là override the **`default transaction isolation level`** of the table - the **READ COMMITTED** isolation level, in which retrieving the changing data will be blocked until these changes are committed_
* _tells SQL **`not to use shared locks when reading data`** (is the equivalent of using READ UNCOMMITED as a transaction isolation level)_

* => this **`makes queries faster`** because they don't wait for other transactions to finish; but it also means we **`can read uncommitted (incorrect) data`**
* (_nói chung là nó cho phép SQL đọc dữ liệu từ các bảng mà không bị chặn bởi các tiến trình khác - việc này giúp cải thiện hiệu suất truy vấn, tuy nhiên có khả năng dữ liệu đọc được bị sai lệch, không chuẩn_)

* => it is not safe for applications where **data accuracy is important**
* => is a good idea when the system uses explicit transactions heavily, which blocks the data reading very frequently
* => is used when working with systems that accept out of sync data or when **speed matters more than accuracy**, such as the reporting or analytics systems

* _Ex: in banking applications with high transaction rates, this is dangerous because we might show an incorrect balance or process a wrong transaction_

### Problem 
* -> **`Dirty Reads`** – which can occur when reading the data that is being modified or deleted during the uncommitted data read, so that the data you read could be different, or never even have existed
* -> **`Nonrepeatable Reads`** – occurs when it is required to read the same data multiple times and the data changes during these readings; in this case, you will read multiple versions of the same row
* -> **`Phantom Reads`** – we will get more records when the transaction that is inserting new records is rolled back, or fewer records when the transaction that is deleting existing data is rolled back

* -> Another problem that may occur when other transactions move data you have not read yet to a location that you have already scanned, or have added new pages to the location that you already scanned. In this case, you will miss these records and will not see it in the returned result. If another transaction moves the data that you have already scanned to a new location that you have not read yet, you will read the data twice. Also, as the requested data could be moved or deleted during your reading process, the below error could be faced:
```r
Msg 601, Level 12, State 1
Could not continue scan with NOLOCK due to data movement.
```

### Example:
```sql
-- chạy trên một cửa sổ sql studio management khác
-- nhưng ta sẽ không comit tới database để những bản ghi bị khóa 
BEGIN TRAN
UPDATE Person.Contact SET Suffix = 'B' WHERE ContactID < 20
-- ROLLBACK or COMMIT

-- nếu giờ ta chạy câu truy vấn dưới đây thì ta sẽ thấy nó cứ treo mãi mà không trả về kết quả nào
-- ta có thể chạy 'sp_who2' để biết được là lệnh SELECT đã bị block lại
SELECT * FROM Person.Contact WHERE ContactID < 20

-- để truy xuất những bản ghi đã bị khóa đó, giờ ta sẽ sử dụng chỉ thị NOLOCK
-- ta sẽ thấy cột Suffix bây giờ tất cả giá trị đều là "B"
SELECT * FROM Person.Contact WITH (NOLOCK) WHERE ContactID < 20

-- tương tự với READUNCOMMITTED
-- allows the query to see the data changes before committing the transaction that is changing it
SELECT * FROM Person.Contact WITH (READUNCOMMITTED)

-- => Nếu trong trường hợp lệnh UPDATE trên kia được rollback lại thì dữ liệu trả về bị sai lệch, không chuẩn là ở chỗ đó
```

```sql
-- NOLOCK cần một khóa Sch-S ( ổn định schema ), SELECT sử dụng NOLOCK vẫn có thể bị chặn nếu một bảng bị thay đổi và không được commit
-- run in query window 1
BEGIN TRAN
ALTER TABLE Person.Contact ADD column_b 
VARCHAR(20) NULL ;

-- Nếu chúng ta chạy câu lệnh SELECT nó vẫn sẽ bị chặn trừ khi chúng ta comit hoặc rollback
-- run in query window 2
SELECT * FROM Person.Contact WITH (NOLOCK) WHERE ContactID < 20
```

```sql
-- ta cũng có thể thiết lập Isolation Level cho tất cả truy vấn sử dụng NOLOCK hoặc READUNCOMMITTED, thay vì từng cái

SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; -- turn it on

SELECT * FROM Person.Contact WHERE ContactID < 20 
UPDATE Person.Contact SET Suffix = 'B' WHERE ContactID = 1 
SELECT * FROM Person.Contact WHERE ContactID < 20

SET TRANSACTION ISOLATION LEVEL READ COMMITTED; -- turn it off
```

## 19. Subquery:
* * -> is **`a query nested inside another SQL query`**
* => return a single value, a list of values, or a table, depending on the context

* It allows you to perform complex queries by using the results of the inner query in the outer query
```sql
-- Retrieving orders for customers from the USA
SELECT OrderID, OrderDate, TotalAmount
FROM Orders
WHERE CustomerID IN (
    SELECT CustomerID
    FROM Customers
    WHERE Country = 'USA'
);
```

## 22. Triggers:
* -> is a **database object** that **`automatically executes a set of SQL statements in response to specific events`** (e.g., INSERT, UPDATE, DELETE) on a table
* => enforce business rules, maintain data integrity, or automate tasks

```sql
-- Creating a trigger to update last modified date on customer update
CREATE TRIGGER tr_UpdateLastModified
ON Customers
AFTER UPDATE
AS
BEGIN
    UPDATE Customers
    SET LastModified = GETDATE()
    FROM Customers
    JOIN inserted ON Customers.CustomerID = inserted.CustomerID;
END;
```

## 34. Recursive CTE
* -> is **`a CTE`** that **`calls itself repeatedly`** until **a termination condition is met**
* => useful for problems like **`Hierarchical data `**(_e.g., employees & managers, category trees_), **`Graph traversal`** (_e.g., finding shortest paths_), **`Generating sequences`** (_e.g., numbers, dates_)

```sql
-- SQL Server limits recursion to 100 levels by default
-- to override it:
OPTION (MAXRECURSION 200);
```

### Structure
* -> **`Anchor Query`** (Base case) - runs once
* -> **`Recursive Query`** (Recursive call) - calls itself until a stopping condition is met
```sql
WITH RecursiveCTE AS (
    -- Anchor Query (Base Case)
    SELECT Column1, Column2
    FROM Table
    WHERE SomeCondition

    UNION ALL

    -- Recursive Query (Recursive Call)
    SELECT Column1, Column2
    FROM Table
    JOIN RecursiveCTE ON SomeCondition
)
SELECT * FROM RecursiveCTE;
```

### Example
```sql
-- define table:
CREATE TABLE Employees (
    EmployeeID INT NOT NULL,
    Name VARCHAR(MAX) NOT NULL,
    ProductID INT NULL
);
INSERT INTO Employees (Name, ProductID) VALUES ('CEO', NULL);
INSERT INTO Employees (Name, ProductID) VALUES ('Manager A', 1);

-- Query to get all employees reporting to the CEO:
WITH EmployeeHierarchy AS (
    -- Base Case: Start with the CEO (Top of the hierarchy)
    SELECT EmployeeID, Name, ManagerID, 1 AS Level
    FROM Employees
    WHERE ManagerID IS NULL
    
    UNION ALL
    
    -- Recursive Case: Find employees who report to the previous level
    SELECT e.EmployeeID, e.Name, e.ManagerID, eh.Level + 1
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmployeeID
)
SELECT * FROM EmployeeHierarchy;
```

```sql
-- Generating a Sequence of Numbers
WITH Numbers AS (
    SELECT 1 AS Num  -- Base Case (Start at 1)
    UNION ALL
    SELECT Num + 1 FROM Numbers WHERE Num < 10  -- Recursive Case
)
SELECT * FROM Numbers;

-- Output: 1    2   3   ...     10
```


## 15. SQL Reusability 
* -> Stored Procedure, Function (TVF, Scalar), View, CTE, Temp Table

### View:
* is a **`virtual table`** (means **treated/behaves like a table** but it **doesn't physically store the data**, just a stored query produces a result set dynamically when queried) based on **`a SELECT query`**
* => allows to **`encapsulate complex or common queries and abtract it to simplify complex joins and aggregations`**

```sql
-- create a view
CREATE VIEW vw_ActiveUsers AS
SELECT Id, Name, Email FROM Users WHERE IsActive = 1;
-- usage:
SELECT * FROM vw_ActiveUsers;

-- equivalent to:
SELECT Id, Name, Email FROM Users WHERE IsActive = 1;
```

### Procedure:
* -> is a **`precompiled collection of SQL statements`** stored in a database 
* => **`encapsulate complex logic, ensure code reusability and improve performance`**

```sql
-- Creating a procedure to insert a new customer
CREATE PROCEDURE sp_InsertCustomer
    @FirstName VARCHAR(50),
    @LastName VARCHAR(50),
    @Email VARCHAR(100)
AS
BEGIN
    INSERT INTO Customers (FirstName, LastName, Email)
    VALUES (@FirstName, @LastName, @Email);
END;
```

### CTE - Common Table Expressions
* -> define **a reusable temporary result set** **`within a query`**
* => provide a simple, readable way to **organize a query**

```sql
WITH ActiveUsers AS (
    SELECT Id, Name, Email FROM Users WHERE IsActive = 1
)
SELECT * FROM ActiveUsers;
```

### Temporary Tables
* ->  to **`reuse data within a session`** (_can be used across multiple queries until session end or explicitly dropped_)
* => provide reusability across **multiple queries**
```sql
CREATE TABLE #TempUsers (Id INT, Name NVARCHAR(100));
INSERT INTO #TempUsers SELECT Id, Name FROM Users WHERE IsActive = 1;
SELECT * FROM #TempUsers;
DROP TABLE #TempUsers;
```

### Table Variables
* -> similar to **temporary tables**, but scoped to **`a single batch`**

```sql
DECLARE @TempUsers TABLE (Id INT, Name NVARCHAR(100));
INSERT INTO @TempUsers SELECT Id, Name FROM Users WHERE IsActive = 1;
SELECT * FROM @TempUsers;
```


## 'View' vs 'Function'

* **View**
* -> can't take **parameters**
* -> optimized by using **`Indexed Views`** - **`physically stores the result set`** once the required **`clustered index`** is created and **`automatically updated when base tables change`**
* => faster **performance**, especially for **`complex, frequently used aggregations, joins, query`** 
* -> can use **non-deterministic functions** (_like GETDATE()_)

```sql
-- indexed view for a complex aggregation that is used frequently
-- this Ex show querying "vw_TotalSales" is much faster than recalculating the SUM every time!
CREATE VIEW vw_TotalSales WITH SCHEMABINDING AS
SELECT CustomerID, SUM(TotalAmount) AS TotalSpent
FROM Orders
GROUP BY CustomerID;
GO
CREATE UNIQUE CLUSTERED INDEX idx_vw_TotalSales ON vw_TotalSales (CustomerID);
```
```sql
-- define a regulalr view:
CREATE VIEW vw_SalesSummary
WITH SCHEMABINDING  -- Required for an indexed view
AS
SELECT 
    ProductID, 
    SUM(Quantity) AS TotalQuantity, 
    COUNT_BIG(*) AS OrderCount
FROM dbo.Sales
GROUP BY ProductID;

-- converts the view into an Indexed View (Materialized View) by creating a Clustered Index on the View
CREATE UNIQUE CLUSTERED INDEX IX_SalesSummary 
ON vw_SalesSummary(ProductID);
```

* **Function**
* -> can take **parameters**
* -> cannot have indexes, so they may perform worse on large datasets
* -> cannot use **non-deterministic functions**

## 'Store procedures' vs 'Function'

* **user-defined functions (UDFs)** 
* -> must return a **`computed values`** - a **scalar value** (_can be used inline in SQL statements like SELECT, other procedure, function_) or a **result set** (_can be joined upon_)
* -> **`cannot perform permanent environmental changes`** (must read-only) to SQL Server (i.e., no INSERT or UPDATE statements allowed)
* **Note**: we can still  declare and INSERT (UPDATE and DELETE is not allowed) data into a table variable inside a function

```sql - Ex:
-- Scalar UDF
-- -> returns a single value used for calculations or transformations
CREATE FUNCTION fn_GetDiscount(@price DECIMAL(10,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    RETURN @price * 0.10; -- Returns 10% discount
END;

SELECT fn_GetDiscount(500) AS DiscountAmount; -- Output: 50
```
```sql - Ex:
-- TVF - Table-Valued Function
-- -> returns a table instead of a single value
-- -> can be used like a table in SELECT statements

-- inline TVF
CREATE FUNCTION fn_GetActiveUsers()
RETURNS TABLE
AS
RETURN 
(
    SELECT Id, Name, Email FROM Users WHERE IsActive = 1
);
SELECT * FROM fn_GetActiveUsers(); -- usage

-- Multi-Statement TVF (uses a table variable inside the function)
CREATE FUNCTION fn_GetOrdersByCustomer(@CustomerId INT)
RETURNS @Orders TABLE (OrderID INT, OrderDate DATETIME)
AS
BEGIN
    INSERT INTO @Orders
    SELECT OrderID, OrderDate FROM Orders WHERE CustomerID = @CustomerId;
    
    RETURN;
END;
SELECT * FROM fn_GetOrdersByCustomer(1); -- usage
```

* **store procedures**
* -> can change database objects
* -> can only be invoked using the CALL statement (_we can call a `function` inside stored procedure but not vise versa_)
* -> do not required to return a value, but we can still return values using **`RETURN` or `OUTPUT` parameters**
* -> ngoài ra trong quá trình thực thi, stored procedure có thể s/d **`Transaction`**, **`Error handling (try/catch)`**; còn function thì không thể

```sql - Ex:
--define
CREATE PROCEDURE dbo.AddCustomer  
    @Name NVARCHAR(100),  
    @Email NVARCHAR(100)  
AS  
BEGIN  
    INSERT INTO Customers (Name, Email)  
    VALUES (@Name, @Email);  
END;

--usage
EXEC dbo.AddCustomer 'John Doe', 'john.doe@example.com';
```

## 19. SQL Server Collation
* _https://learn.microsoft.com/en-us/sql/relational-databases/collations/collation-and-unicode-support?view=sql-server-ver16_
* -> used with **`character data types`** (_such as **char** and **varchar**_), specifies the bit patterns that represent each character in a dataset
* -> When we select a collation, we're **`assigning certain characteristics to our data`**
* -> these characteristics affect the results of many operations in the database 

* => determine how the database engine **sorts and compares character data**
* => nó sẽ ảnh hưởng đến data integrity, query performance, cross-system compatibility, localization

* _đây là vấn đề ta cần quan tâm before creating a database_
* _nếu dự án của ta sử dụng multiple database, inconsistent **collation** settings can cause errors_

### Checking
* _when picking a collation, check the SQL Server documentation for a full list of collations and their features_

* -> check **server collation** for **an instance of SQL Server**:
```sql
SELECT SERVERPROPERTY('collation');
```

* -> find list of collations that are **available on instance of SQL Server**
```sql
SELECT * FROM sys.fn_helpcollations();
```

### Options 
* _when `Binary (_BIN)` or `Binary-code point (_BIN2)` is chosen, other collation options aren’t available_
* -> **Case-sensitive (_CS)**
* -> **Accent-sensitive (_AS)**
* -> **Kana-sensitive (_KS)**
* -> **Width-sensitive (_WS)**
* -> **Variation-selector-sensitive (_VSS)a**
* -> **Binary (_BIN)**
* -> **Binary-code point (_BIN2)**

```bash - Example
$ `SQL_Latin1_General_CP1_CI_AS`: Default collation for US English, case-insensitive, accent-sensitive.
$ `Latin1_General_CS_AS`: Case-sensitive and accent-sensitive collation for US English.
$ `French_CI_AS`: Case-insensitive and accent-sensitive collation for French.
$ `Japanese_CI_AS`: Case-insensitive and accent-sensitive collation for Japanese.
$ `Chinese_PRC_CI_AS`: Case-insensitive and accent-sensitive collation for Simplified Chinese (PRC)
```

### Supported
* -> SQL Server supports storing objects that have different collations in database

### Collation levels
* https://www.geeksforgeeks.org/sql-server-collation/

* -> **`Server-level collations`**
* -> **`Database-level collations`**
* -> **`Column-level collations`**
* -> **`Expression-level collations`**

```sql
-- 'Server-level collations' set during SQL Server setup
--  find server-level collation for an SQL Server
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));

-- Database-level collations: to create database with collation:
CREATE DATABASE GeekDB COLLATE Greek_CS_AI; 
-- change collation of user database:
-- -> however, collation for system databases cannot be changed until change of collation for server
-- -> and altering database-level collation doesn’t affect column-level or expression-level collations
ALTER DATABASE GeekDB COLLATE SQL_Latin1_General_CP1_CI_AS; 

-- Column-level collations: while creating or altering table, one can specify collations for each character-string column by using COLLATE clause:
CREATE TABLE Users (
    ID INT PRIMARY KEY,
    Name VARCHAR(100) COLLATE SQL_Latin1_General_CP1_CI_AS -- 'Case-Insensitive' and 'Accent-Sensitive'
);
-- Case-Insensitive:
INSERT INTO Users (ID, Name) VALUES (1, 'John');
SELECT * FROM Users WHERE Name = 'john'; -- still find the record
-- Accent Sensitive:
INSERT INTO Users (ID, Name) VALUES (2, 'José');
SELECT * FROM Users WHERE Name = 'Jose'; -- No result
-- change collation of column:
ALTER TABLE Geektable 
ALTER COLUMN namecol NVARCHAR(10) COLLATE Greek_CS_AI; 

-- Expression-level collations: are used when statement is run, and they affect the way output is returned
SELECT * FROM tablename 
ORDER BY columnname COLLATE collationtype;
```

```sql - sorting
CREATE TABLE SortTest (
    Name VARCHAR(50) COLLATE Latin1_General_CS_AS -- Case-Sensitive
);
INSERT INTO SortTest (Name) VALUES ('apple'), ('Banana'), ('cherry'), ('Apple'), ('banana');
SELECT * FROM SortTest ORDER BY Name;
-- Output:
-- Apple  
-- Banana  
-- apple  
-- banana  
-- cherry

CREATE TABLE SortTest2 (
    Name VARCHAR(50) COLLATE Latin1_General_CI_AS -- Case-Insensitive
);
INSERT INTO SortTest2 (Name) VALUES ('apple'), ('Banana'), ('cherry'), ('Apple'), ('banana');
SELECT * FROM SortTest2 ORDER BY Name;
-- Apple  
-- apple  
-- Banana  
-- banana  
-- cherry 
```

## 3. Database performance
* -> Indexing
* -> Database Replication
* -> Database Sharding

## 4.best practice to write efficient sql queries?

## SQL Process
* https://docs.oracle.com/en/database/oracle/oracle-database/21/tgsql/sql-processing.html#GUID-14D902DA-5CAC-4892-BE8B-880A4F8A6914
* https://www.geeksforgeeks.org/sql-query-processing/

### SQL Performance Tuning
* -> is the process of **optimizing SQL queries** to improve the **`speed and efficiency of database operations`**
* -> involves various techniques to **optimize the execution of queries**, **manage system resources more effectively**, and **ensure that the database responds quickly to user requests**

### Major Factors Affecting SQL Speed (computation and execution time in SQL)
* -> **`Table Size`**: Larger tables with millions of rows can slow down query performance if the query hits a large number of rows.
* -> **`Joins`**: The use of complex joins, especially when joining multiple tables, can significantly affect query execution time.
* -> **`Aggregations`**: Queries that aggregate large datasets require more processing time and resources.
* -> **`Concurrency`**: Simultaneous queries from multiple users can overwhelm the database, leading to slow performance.
* -> **`Indexes`**: Proper indexing speeds up data retrieval but, when misused, can lead to inefficiencies

### SQL query performance
* -> **`SELECT fields instead of using SELECT *`**
- chỉ SELECT những cột cần thiết, không nên SELECT *
- Selecting unnecessary columns increases memory usage and network traffic, slowing performance
```sql
SELECT * FROM Employees;

-- replace with:
SELECT EmployeeID, FirstName, LastName FROM Employees;
```

* -> **`Avoid SELECT DISTINCT`**
- DISTINCT forces sorting or hashing, which can be expensive
- a better approach is using GROUP BY, JOIN conditions, or subqueries to refine results
```sql
SELECT DISTINCT FirstName, LastName,
State FROM GeeksTable;

-- replace with:
SELECT  FirstName, LastName,
State FROM GeeksTable WHERE State IS NOT NULL;
```

* -> **`Use INNER JOIN Instead of WHERE for Joins`**
```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1, GFG2
WHERE GFG1.CustomerID = GFG2.CustomerID

-- replace with:
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1 
INNER JOIN GFG2
ON GFG1.CustomerID = GFG2.CustomerID
```

* -> **`Use WHERE Instead of HAVING`**
- HAVING is applied after aggregation, so filtering earlier using WHERE minimizes data before aggregation
```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
 FROM GFG1 INNER JOIN GFG2
ON GFG1.CustomerID = GFG2.CustomerID
GROUP BY GFG1.CustomerID, GFG1.Name
HAVING GFG2.LastSaleDate BETWEEN "1/1/2019" AND "12/31/2019"

-- replace with:
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1 INNER JOIN GFG2
ON GFG1.CustomerID = GFG2.CustomerID
WHERE GFG2.LastSaleDate BETWEEN "1/1/2019" AND "12/31/2019"
GROUP BY GFG1.CustomerID, GFG1.Name
```

* -> **`Limit Wildcards to the End of a Search Term`**
- Indexes work left to right, so using % at the beginning forces a full table scan.
- If you must search within strings (%No%), consider full-text search (FTS) or indexing solutions
```sql
SELECT City FROM GeekTable WHERE City LIKE ‘%No%’

-- replace with:
SELECT City FROM GeekTable WHERE City LIKE ‘No%’ 
```

* -> **`Use LIMIT for Sampling Query Results`**
- Good practice for debugging/testing queries.
- Be aware that in some databases (SQL Server), LIMIT is replaced by TOP, and in Oracle, it's FETCH FIRST N ROWS
```sql
SELECT GFG1.CustomerID, GFG1.Name, GFG1.LastSaleDate
FROM GFG1
INNER JOIN GFG2
ON GFG1.CustomerID = GFG2.CustomerID
WHERE GFG2.LastSaleDate BETWEEN "1/1/2019" AND "12/31/2019"
GROUP BY GFG1.CustomerID, GFG1.Name
LIMIT 10
```

* -> **`Run Queries During Off-Peak Hours`**
- Scheduling heavy queries reduces impact on production.
- If real-time analytics is needed, consider read replicas, indexed views, or caching mechanisms.

* -> **`Create Small Batches of Data for Deletion and Updation`**
- in case if there will be a rollback, you will avoid losing or killing your data
- also enhances concurrency, other operations can continue processing unaffected data while small batches are being modified
- deleting or updating large amounts of data in bulk can cause performance issues and long locks
```sql
DELETE FROM Orders WHERE OrderDate < '2020-01-01';

-- replace with small batches to prevents long-running transactions and excessive locking 
WHILE 1=1
BEGIN
    DELETE TOP (1000) FROM Orders WHERE OrderDate < '2020-01-01';
    IF @@ROWCOUNT = 0 BREAK;
END
```

* -> **`Use CASE instead of UPDATE`**
- Using CASE inside an UPDATE can help apply conditional changes in a single scan rather than running multiple UPDATE statements
```sql
UPDATE Employees SET Status = 'Active' WHERE LastLogin >= '2023-01-01';
UPDATE Employees SET Status = 'Inactive' WHERE LastLogin < '2023-01-01';

-- replace with:
UPDATE Employees
SET Status = CASE 
    WHEN LastLogin >= '2023-01-01' THEN 'Active'
    ELSE 'Inactive'
END;
```

* -> **`Use Temp Tables`** 
- thay vì join 1 bảng lớn, ta sẽ extract thành 1 bảng tạm nhỏ as frequently accessed data để reducing repeated table scans, computational load when performning complex joins on large datasets
```sql
SELECT o.OrderID, c.CustomerName  
FROM Orders o  
JOIN Customers c ON o.CustomerID = c.CustomerID  
WHERE c.Region = 'West';

-- replace with:
SELECT CustomerID INTO #TempCustomers FROM Customers WHERE Region = 'West';

SELECT o.OrderID, c.CustomerName  
FROM Orders o  
JOIN #TempCustomers c ON o.CustomerID = c.CustomerID;
```

* -> **`Avoid Negative Searches`**
- Queries that use **NOT IN**, **NOT LIKE**, or **<>** slow down our DB performance a lot
```sql
SELECT * FROM Customers WHERE NOT Country = 'USA';

-- replace with:
SELECT * FROM Customers WHERE Country IN ('Canada', 'UK', 'Germany');
--or
SELECT c.CustomerID FROM Customers c  
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID  
WHERE o.CustomerID IS NULL;
```

* -> **`No Need to Count Everything in the Table`**
- EXISTS(SELECT 1 FROM dbo.T1) is faster than COUNT(*), as it stops searching after finding the first match
```sql
DECLARE @CT INT;
SET @CT = (SELECT COUNT(*) FROM Orders WHERE Status = 'Pending');
IF @CT > 0
BEGIN
    PRINT 'Orders Pending';
END

-- replace with:
IF EXISTS (SELECT 1 FROM Orders WHERE Status = 'Pending')
BEGIN
    PRINT 'Orders Pending';
END
```

* -> **`Avoid Using Globally Unique Identifiers (GUIDs)`**
- GUIDs (NEWID()) are large and unordered, which can fragment indexes and slow performance.
- Use IDENTITY (auto-increment) or SEQUENTIAL GUIDs (NEWSEQUENTIALID()) when unique values are needed in an ordered manner
```sql
CREATE TABLE Orders (
    OrderID UNIQUEIDENTIFIER DEFAULT NEWID() PRIMARY KEY,
    OrderDate DATETIME
);

-- replace with:
CREATE TABLE Orders (
    OrderID INT IDENTITY(1,1) PRIMARY KEY,
    OrderDate DATETIME
);
```

* -> **`Avoid Using Triggers`**
- using triggers results in lock many tables until the trigger completes the cycle. 
- we can split the data into several transactions to lock up certain resources; this will help us in making our transaction faster
```sql
CREATE TRIGGER trg_UpdateStock  
ON Orders AFTER INSERT  
AS  
BEGIN  
    UPDATE Products SET Stock = Stock - 1  
    FROM Products p JOIN inserted i ON p.ProductID = i.ProductID;  
END

-- replace with:
CREATE PROCEDURE ProcessOrder(@ProductID INT)  
AS  
BEGIN  
    UPDATE Products SET Stock = Stock - 1 WHERE ProductID = @ProductID;  
END
```

* -> **`Avoid Using ORM`**
- ORMs is useful for rapid development and maintaining business logic; however, it may generate inefficient queries that my cause a bad performance in our daily encounters
- instead of completely avoiding ORMs, optimize ORM-generated queries by using **stored procedures**, **tuning queries**, and **leveraging lazy/eager loading** wisely

* -> **`Avoid Using DISTINCT If Not Necessary`**
- instead of using DISTINCT, ensure that duplicates are eliminated at the source by properly structuring joins and conditions
```sql
SELECT DISTINCT CustomerID FROM Orders;

-- replace with:
SELECT CustomerID FROM Orders GROUP BY CustomerID;
```

* -> **`Use Fewer Cursors`**
- Cursors process one row at a time, leading to slow performance and locking issues
- Use set-based operations (JOIN, CASE, MERGE, CTE) instead of cursors whenever possible
```sql
DECLARE cur CURSOR FOR SELECT OrderID FROM Orders;
OPEN cur;
FETCH NEXT FROM cur INTO @OrderID;
WHILE @@FETCH_STATUS = 0  
BEGIN  
    -- Process each order  
    FETCH NEXT FROM cur INTO @OrderID;  
END  
CLOSE cur;
DEALLOCATE cur;

-- replace with:
UPDATE Orders SET Status = 'Processed' WHERE OrderDate < '2023-01-01';
```

* -> **`Avoid using 'subqueries'`**
```sql
SELECT * FROM customers WHERE customer_id IN (SELECT customer_id FROM orders WHERE order_date >= DATEADD(day, -30, GETDATE()));

-- this query will be faster than the previous query:
SELECT DISTINCT c.* FROM customers c JOIN orders o ON c.customer_id = o.customer_id WHERE o.order_date >= DATEADD(day, -30, GETDATE());
```

* -> **`using stored procedure`**
- khi ta gửi 1 raw SQL queries from the application, câu query dài có thể ảnh hưởng tới traffic
- và đồng thời database sẽ phải đọc câu query này để đưa ra execution plan rồi mới thực thi đc
- stored procedure thì khác, khi được lưu DB sẽ biết nó đc parsed and optimized thế nào, nên có thể thực thi ngay lập tức

* -> **`'Index' tuning`**

## 18. Difference between TRUNCATE, DELETE and DROP commands?
### DELETE
* a Data Manipulation Language Command (DML) -> used to modify `data`
* can either `delete all` the rows or can delete specific rows from a table (based on conditions using the `WHERE` clause)
* slower than the TRUNCATE command. The TRUNCATE command does not remove the structure of the table
* DELETE operations are logged individually -> allows for rollback or recovery if necessary

### TRUNCATE
* a Data Definition Language Command (DDL) -> used to modify the `structure of the database`
* used to `delete all the rows of a relation (table)` in one go. 
* we `can’t delete the single row` as here `WHERE` clause is not used
* It is comparatively faster than the DELETE command as it deletes all the rows fastly. 
* it deallocates the data pages without logging each row deletion -> can't use the ROLLBACK command to restore the data

### DROP
* a Data Definition Language Command (DDL) -> modify the `structure of the database`
* permanently removes the specified database table, including all its contents (definition, indexes, data, constraints, triggers,...)
* can't use the ROLLBACK command to restore the table
* DROP can't be used with tables referenced by foreign keys

## 19. Primary Key and Foreign Key
* **Key** - a column or a combination of columns that uniquely identifies a record in a database table

### Primary Key
* A primary key is used to ensure that data in the `specific column` is `unique` 
* A column `cannot have NULL` values. 
* It is either an existing table column or a column that is specifically generated by the database according to a defined sequence

### Foreign Key
* A foreign key is `a column or group of columns` in a relational database table that `provides a link` between data in two tables. 
* It is a column (or columns) that references a column (most often the primary key) of another table

### Composite key
* it can also be understood as a `primary key` made by the combination of two or more columns in a table to uniquely identify every row in a table
* A composite key cannot be null

### Candidate Key
* A Candidate for Primary Key (may or may not be selected)

## 20. Group by dùng để làm gì ?

## 21. UNION, UNION ALL, MINUS,  INTERSECT ?
###  UNION - UNION ALL
* `UNION` operator used to combine the result-set of two or more SELECT statements; selects only distinct values
* `UNION ALL` - like UNION but allow duplicate values 

### INTERSECT
* used to retrieve the **`common records`** between the result-sets of two SELECT statements

### The MINUS 
* returns all the records in the first SELECT query that are not returned by the second SELECT query

## Database Schema in SQL Server
* a `a list of logical structures of data` (_tables, views, stored procedures, indexes, triggers, functions_)
* a  schema is an individual entity (container of objects) distinct from the user who constructs the object

 v * -> Schemas may be assigned security permissions -> an effective method for distinguishing and defending database objects based on user access privileges

### feature
* A schema can belong to only `one database` whereas a database can have one or `multiple schemas`
* `SQL Server` provides us with a few **`built-in schemas`** such as **`dbo, guest, sys, etc`**
* A database schema can be `owned` by a `database role` or an `application role` along with the `database user` - They are called **`schema owners`**
* _`dbo`_ is the **`default schema`** for a newly created database.
* `Schema ownership` _can be transferred_ from one user to another user in the same database.
* A database user can be dropped without dropping the database objects owned by the user; But the schema `cannot be deleted` if it owns database objects
```
CREATE SCHEMA hrdbo AUTHORIZATION Steve;

CREATE TABLE hrdbo.Consultant
(  
    ConsultantID int,
    FirstName nvarchar(50) NOT NULL,  
    LastName nvarchar(50) NOT NULL
);
```

## 22. WHERE - HAVING
* The `HAVING` clause was added to SQL because the `WHERE` keyword cannot be used with **aggregate functions**
```
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

## 23. Hàm Count để làm gì ?
* COUNT() function returns the number of rows that matches a specified criterion.

## 24. Phân biệt Inner join và Left join ?
* The INNER JOIN keyword selects records that have matching values in both tables

* The LEFT JOIN keyword returns all records from the left table (table1), and the matching records from the right table (table2) (The result is 0 records from the right side, if there is no match)

## 25. Viêt ra một câu mẫu cú pháp Insert Delete Update trong SQL ?
```
INSERT INTO Customers (CustomerName, ContactName, Address)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21');
```
```
DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
```
```
UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;
```

## 26. Data Types to store string in SQL Server 

### CHAR(n) - store Fixed-Length Character (up to (1 to 8000) character)
* -> reserves **`a fixed amount of storage space, regardless of the actual length`** of the stored string
* _if the stored string is **shorter** than the defined length, it is `padded with spaces` to fill the remaining space_
* _if the stored string is **longer** than the defined length, it will get `truncate` (default) or `error` (if SET ANSI_WARNINGS ON is enabled)_
* => best for data with a **consistent length, like country codes or fixed-length IDs**
* _can only store character <= 1 byte_

```cs
// Ex: CHAR(10) always takes 10 bytes, even if the string has fewer characters 
```

### VARCHAR(n) - store Variable-Length Character (up to (1 to 8000) character)
* -> using **`only the needed space`**
* => best for general **text storage where length varies**

* stores character strings of up to 255 bytes in a variable-length field
```cs
// Ex: VARCHAR(100) stores "Hello" using only 7 bytes 
// 5 bytes for actual string and extra 2 bytes to store the length of the string

// storing 6 characters in VARCHAR(6), VARCHAR(100), or VARCHAR(MAX) uses the same amount of storage
// the size declare in VARCHAR only to limit how much to store
```

### VARCHAR(MAX) 
* -> is **VARCHAR** but allow to store up to **`2^31-1 (2,147,483,647) characters`**
* => used for very **large text data like long descriptions, JSON, or XML**

### Unicode Character Data Types
* _these support Unicode (UTF-16) for **`international text storage`**:_
* -> **`NCHAR(n)`** – **fixed-length** Unicode string (1 to 4,000 characters); takes **2 bytes per character**
* -> **`NVARCHAR(n)`** – **variable-length** Unicode string (1 to 4,000 characters); uses **2 bytes per character**, but **only stores the actual length**
* -> **`NVARCHAR(MAX)`** – Stores up to 2^31-1 Unicode characters

## 27. Sự khác biệt giữa khóa chính (Primary Key) và các ràng buộc duy nhất (Unique Constraints) là gì?
### Primary Key:
* _`a column or a combination`_ of columns that `uniquely identifies each row` in a table
* _`only one primary key`_ defined per table.
* _`automatically have a NOT NULL constraint`_, meaning they cannot contain NULL values.
* create _`a clustered index`_ by default - physically organizing the data in the table based on the key values
* are typically used as the **`main identifier`** for a table and are often used as a `reference in foreign key` relationships with other tables.

### A Unique Constraint 
* ensures that the _`values in a column or a combination of columns`_ are unique, but do not necessarily serve as the main identifier for a table as primary key
* _`Multiple unique constraints`_ can be defined per table, allowing uniqueness to be enforced on different sets of columns.
* _`can include NULL values`_, and multiple rows with NULL values in the constrained columns are allowed since NULL is not considered equal to any other value, including another NULL.
* Unique constraints create a _`non-clustered index`_ by default - which provides efficient lookup for the unique values but does not organize the data physically.
* Unique constraints can be referenced by foreign keys, just like primary keys

## 28. Chuẩn hóa - Normalization là gì? Ví dụ về 1NF, 2NF, 3NF Database
* Normalization is `process of organizing data in a database` to protect data and eliminate redundancy, inconsistent dependency
* It includes creating tables and establishing relationships between them according to rules designed
* Each rule is called a **`normal form`**  addressing specific types of data anomalies
* `third normal form` is considered the highest level necessary for most applications

### Problem
* `Redundant data` -> wastes disk space and creates maintenance problems
```
// must change data that exists in more than one place 
// must change in exactly the same way in all locations
-> stored only in the one table and nowhere else in the database
```

* `inconsistent dependency` - make data difficult to access because the path to find the data may be missing or broken
```
// look in the Customers table for the address of a particular customer is intuitive
// look there for the salary of the employee who calls on that customer is not make sense
-> The employee's salary should be moved to the "Employees" table
```

### 1NF - First Normal Form
* Eliminate repeating groups in individual tables
* Create a separate table for each set of related data.
* Identify each set of related data with a primary key

* _`Do not use multiple fields in a single table to store similar data`_
```
// an inventory record may contain fields for Vendor Code 1, Vendor Code 2,...
// -> Instead, place all vendor information in a separate table called "Vendors"
// -> link inventory to vendors with an item number key / vendors to inventory with a vendor code key
```

### 2NF - Second Normal Form
* Meet every rule from 1NF
* Create separate tables for sets of values that apply to multiple records
* Relate these tables with a foreign key

* _`Records should not depend on anything other than a table's primary key (a compound key, if necessary)`_ (move the data to the appropriate table or create a new table and primary key)
```
// The customer's address is needed by the Customers table, but also by the Orders, Shipping, Invoices, Accounts Receivable, and Collections Tables
// -> Instead of storing the customer's address as a separate entry in each of these tables
// -> store it in one place (either in the Customers table or in a separate Addresses table)

// the address attribute is dependent on the primary key of the Customers table. Other tables that require the address information can establish a relationship with the Customers table through a foreign key
```

### 3NF - Third Normal Form
* Meet every rule from 2NF
* Eliminate fields that do not depend on the key (_Remove attributes that rely on other non-key attributes_)


### BCNF - Boyce Codd Normal Form 
* Even when a database is in 3rd Normal Form, still there would be anomalies resulted if it has more than one `Candidate Key`
