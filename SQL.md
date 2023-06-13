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

## View:
* A view is a `virtual table` derived from a SQL query
* It allows you to `encapsulate complex queries` or commonly used query logic into a named object
* Views do not store data themselves but dynamically generate results based on the underlying query definition.
```
-- Creating a view to retrieve customer information
CREATE VIEW vw_Customers AS
SELECT CustomerID, FirstName, LastName, Email
FROM Customers
WHERE Country = 'USA';
```

## Procedure:
* A procedure is `a named set of SQL statements` stored in the database that can be executed repeatedly. 
* Procedures help group SQL statements into a single logical unit, providing reusability and modularity.
* They are invoked / called by other programs or database users.
```
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

## Subquery:
* a `nested query embedded within another query`
* It allows you to perform complex queries by using the results of the inner query in the outer query
```
-- Retrieving orders for customers from the USA
SELECT OrderID, OrderDate, TotalAmount
FROM Orders
WHERE CustomerID IN (
    SELECT CustomerID
    FROM Customers
    WHERE Country = 'USA'
);
```

## Triggers:
* Triggers are `database objects` associated with a table 
* `automatically execute` in response to specified events, such as INSERT, UPDATE, or DELETE operations. 
* They enforce data integrity, perform additional actions or validations, or maintain audit trails of database changes.
```
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

## 26. CHAR And VARCHAR Data Types In SQL Server 
### CHAR
* store `characters` of a **`fixed length`** - CHAR always `reserves a fixed amount of storage space, regardless of the actual length of the stored string`

* The storage size is specified in terms of characters
* If the stored string is `shorter` than the defined length, it is `padded` with `spaces` to fill the remaining space
* If the stored string is `longer` than the defined length, it will get `truncate or error`

* Data could be a string of single-byte or multibyte numbers, letters, numbers or any other characters that are supported by the database locale. 

```
// If you define a CHAR(10) column and store the string 'Hello' in it, 
// the stored value will be 'Hello ' (with six additional spaces to reach the defined length)
```
### VARCHAR
* store `characters` of **`variable length`** - VARCHAR dynamically `adjusts the storage space based on the actual length of the stored string`

* The storage size is specified in terms of characters
* `No padding` is applied to the stored string

* stores character strings of up to 255 bytes in a variable-length field
* The data can consist of letters, numbers, and symbols. 

```
// If you define a VARCHAR(10) column and store the string 'Hello' in it, 
// the stored value will be 'Hello' (without any additional spaces).
```
### Different
* CHAR stores only `fixed-length character string` data types whereas VARCHAR stores `variable-length string` where an upper limit of length is specified

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
