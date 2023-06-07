# Common LINQ command:
* _collection is an Array of objects / Table_
* _item is an object / record_
* _key is value of an object Property / column value of a record_

## Where:
* **`filter`** a sequence of values based on a predicate.
```
var result = collection.Where(item => condition);
```

## Select:
* Transforms / **`map`** each element of a sequence into a new form.
```
var result = collection.Select(item => transformation);
```

## First:
* Returns / **`find`** **`the first element`** of a sequence that satisfies a given condition
* If `no matching` element is found, an _exception_ is thrown (_InvalidOperationException_)
```
var result = collection.First(item => condition);
```

## FirstOrDefault:
* Returns / **`find`** the first element of a sequence or a default value if no elements satisfy the given condition
```
var result = collection.FirstOrDefault(item => condition);
```

## Single:
* Returns / **`find`** **`the only element`** of a sequence that satisfies a given condition.
* If `no matching` element is found or if **`multiple elements match`** the condition, an _exception_ is thrown
```
var result = collection.Single(item => condition);
```

## SingleOrDefault: 
* Returns / **`find`** the only element of a sequence or a default value if no elements satisfy the given condition. 
* `Throws an exception` if multiple elements satisfy the condition.
```
var result = collection.SingleOrDefault(item => condition);
```

## OrderBy:
* **`sort`** the elements of a sequence in ascending order.
```
var result = collection.OrderBy(item => key);
```

## OrderByDescending:
* **`sort`** the elements of a sequence in descending order.
```
var result = collection.OrderByDescending(item => key);
```

## Join: 
* Combines two sequences based on a common key (**`map`** lại thành 1 Table mới)
```
var result = collection1.Join(collection2
                            , item1 => key1
                            , item2 => key2
                            , (item1, item2) => result);
```

* _Thực tế nó sẽ trông như này (select * và map 2 cột của bảng này với 2 cột của bảng kia)_
```
var result = students
    .Join(grades,
        student => new { studentId = student.Id, className = student.ClassName },
        grade => new { studentId = grade.StudentId, className = grade.Class },
        (student, grade) => new { student, grade })
```

* **`join into`** - Join Table 1 với Table 2 , the group all columns of Table 1
```
var result = from student in students
             join grade in grades on student.Id equals grade.StudentId into studentGrades
             select new { Student = student, Grades = studentGrades };

// Grades is grades
```

## GroupBy:
* Groups elements of a sequence **`based on a Key`**
```
var result = collection.GroupBy(item => key);
```
* _key is item.onecolumn_

```
foreach (var group in result)
{
    Console.WriteLine($"value of each group "Key" is {group.Key}");
    foreach (var record in group)
    {
        Console.WriteLine($"value of b column of this record is {record.b_column}");
    }
}

// ".Key" to take the column value used to group of that record
```
* `group by into`
```
var groupedProducts = from product in products
                      group product by product.Category into categoryGroup
                      select new { Category = categoryGroup.Key, Products = categoryGroup };

// ".Key" to take the column value used to group of that record
```

## Any:
* Checks if any element (**`some`**) in a sequence satisfies a given condition.
```
var result = collection.Any(item => condition);
```

## All:
* Checks if all elements (**`every`**) in a sequence satisfy a given condition.
```
var result = collection.All(item => condition);
```

## Skip:
* Bypasses a specified number of elements in a sequence.
```
var result = collection.Skip(count);
```

## Take:
* Returns a specified number of contiguous elements from the start of a sequence.
```
var result = collection.Take(count);
```

## Count:
* Returns the number of elements in a sequence.
```
var result = collection.Count();
```

## Sum:
* Computes the sum of the numeric values in a sequence.
```
var result = collection.Sum();
```

## Average:
* Computes the average of the numeric values in a sequence.
```
var result = collection.Average();
```

## Max:
* Returns the maximum value in a sequence.
```
var result = collection.Max();
```

## Min:
* Returns the minimum value in a sequence.
```
var result = collection.Min();
```

## Include
* specify `related entities` that should be `included` in the `query results`
* enables **`eager loading`** -> related entities will be eagerly loaded during the query execution

* commonly used when working with ORM frameworks like Entity Framework
```
var orders = context.Orders.Include(o => o.Customer).ToList();
```

## ToList
* `executes` the query and retrieve all the entities returned by the query and `store them in memory` as a List<T> object

* with large datasets or remote databases, it minimizes the number of times application needs to communicate with the database, reducing network latency and overhead.

## IQueryable vs IEnumerable 
* In most cases, you'll be working with IQueryable in Entity Framework, as it provides better performance by pushing the query execution to the database server
* IEnumerable is used for querying in-memory collections or when the data source is already loaded
* DbSet<T> / DbContext.Set<T> is IQueryable<T> object / `IQueryable data source`

### IQueryable
* The `IQueryable` interface represents a **`queryable data source`** (System. Linq)
* IQueryable inherit IEnumerable -> possesses all features of IEnumerable
* It is commonly used when querying against a database using LINQ to Entities (Entity Framework)
* `Deferred execution` is not possible
* support `lazy loading`
* Custom queries are supported with the help of CreateQuery as well as Execute methods
* use case: **`queryable data sources`**, filters on data right at the data source, applying paging composition, load data in a deferred way

* In the process of querying data from databases
* -> IQueryable can be seen executing a select query on the server-side with the help of its filters. 
* -> it does less work and therefore showcases faster performance

```
IQueryable<Customer> query = context.Customers.Where(c => c.Age > 25);
var result = query.ToList();
```

### IEnumerable
* The IEnumerable interface represents a **`sequence of elements that can be enumerated`** (System. Collections)
* It is the base interface for all collections in .NET
* Supports `deferred execution`
* does not support `lazy loading`
* Custom queries are not supported
* use case: **`read-only collections`**; read objects in the forward direction only without concern about thread safety;
querying data from Lists, Arrays, and other in-memory collections

* In the course of querying data from databases
* -> IEnumerable executes by selecting query on the server-side 
* -> loading data in-memory on the client-side, and then filtering the data.
* -> it performs a more work than IQuerytable -> slower

```
IEnumerable<Customer> customers = GetCustomers();
var result = customers.Where(c => c.Age > 25).ToList();
```

### if replace IQueryable with IEnumerable
```
IEnumerable<Customer> query = DbContext.Customers.Where(c => c.Age > 25);
var result = query.ToList();
``` 
* code will still compile and execute successfully
* the query "query.ToList()" will be executed in-memory against the objects already loaded from the database
* It will not benefit from database-specific optimizations

# Execution types of LINQ methods/ Query operation method
* 3 types: Immediate Query Execution, Deferred Execution (streaming or non-streaming)

* Search `"Classification Table"` để xem Query operation đó thuộc Execution type nào
* chắng hạn As a rule of thumb, if the LINQ method `returns an abstract type` (e.g., IEnumerable), it'sd probably using `deferred execution`

## Query evaluation
* Query evaluation can happen in the client-side (e.g., your program) or the server-side (e.g., in a database)

* "evaluate" a LINQ query that ultimately produces a result:
* -> first, an expression is created
* -> **`query evaluation happens`**: the expression gets converted into `expression trees` (represents the `query logic`)
* -> expression trees **`get translated`** by query provider (e.g: Entity Framework) into other types of queries (Ex: if using LINQ with Entity Framework, LINQ query translated into a SQL Query)
* -> `The translated queries` are executed against their data sources, followed by creating the `desired result objects` (using the results of those translated queries - VD: map bằng Entity Framework)

### Force query evaluation
#### Enumerating with a foreach
*  **`iterating`** through the `resulting collection` from a `LINQ operation`
```
var filteredList = firstList.filterMethod(condition); // no work done here
foreach (var item in filteredList) 
{
 ...
}
```
#### Chaining to deferred extension methods a method that does not implement deferred execution
* **`chain`** a `query method operator` that **`requires access to data`** such as (can be more):
* **ToList()** - forces immediate query evaluation and returns a List
* **First()** - forces immediate query evaluation and returns the first entry of type T in the IEnumerable

## Deferred Query Execution
* no data source is accessed until we **`force the LINQ query to be evaluated`**
* Deferred execution is available in C# by using: **`yield return`**

```
// create query variable:
var lst = from e in emList
            where e.Age > 28
            select new { e.Name };

empList.Add(new Employee {ID = 5, Name = "Bill", Age = 39});

foreach (var emp in lst)
    Console.WriteLine(emp.Name);
// Output: Jack, Rahul, Bill

Console.ReadLine();
```
* The query is actually executed when the query variable is `iterated over`, not when the query variable is created -> **`deferred execution`**

* This allows you to execute a query as frequently as you want to, like fetching the latest information from a database that is being updated frequently by other applications. You will always get the latest information from the database in this case.

### Deferred Streaming Execution (https://dev.to/samfieldscc/linq-37k3)
* the method uses a process known as **`lazy evaluation`** when iterating through a collection
* means that only one element of the collection is read and used each time an iterator asks for a new element of the set
* methods of this one almost always returns in the form of an IEnumerable<T> or IOrderedEnumearble<T>
```
VD: .Where() , .Skip() , .Take() , ...
```
### Deferred Non-Streaming Execution
* method uses a process known as **`"eager evaluation"`**
* the first time an iterator goes through the collection, reading it entirely
* The non-streaming execution might require temporary variables to store the original collection
```
VD: .OrderBy() , .GroupBy() , .Join() ,...
```

## Immediate Query Execution
```
var lst = from e in emList
            where e.Age > 28
            select new { e.Name }.Count();

empList.Add(new Employee {ID = 5, Name = "Bill", Age = 39});

Console.WriteLine("{0}", lst); // Output: 2
```
* _in order to `Count()` the elements, the query must be executed_

* **`The basic different`** is `Deferred execution` of queries produce a `sequence of values`, while `Immediate execution` of queries return a `singleton value` and is `executed immediately`
* To `force immediate execution` of a query that does not produce a singleton value -> call the `ToList(), ToDictionary(), ToArray()` method on a query or query variable. 
* These are called **`conversion operators`** which allow you to make a copy/snapshot of the result and access is as many times we want, without the need to re-execute the query

## Dangers with deferred execution
### Deferred exception execution
```
List<int> dataSource = new List<int>() {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
	List<int> valuesToRemove = null;
	IEnumerable<int> filteredData = new List<int>();

    // try-catch block to make sure IEnumerable object not null
	try
	{
		filteredData = dataSource.Where(t => !valuesToRemove.All(x => x == t)); // dòng này chưa lỗi
	}
	catch (ArgumentNullException e)
	{
		Console.WriteLine(e); 
	}

	foreach (var i in filteredData) // dòng này sẽ throw exception
	{
		Console.WriteLine(i);
	} 
```
* Our program'll be `crashed` do try-catch sai chỗ
* The `.Where()` method uses deferred execution -> it `evaluates the query` to execute when a value is requested
* the requested value is in line "foreach...", which triggers the `query evaluation` and `subsequently` throwing an exception

### Multiple enumerations
#### The performance problem
* sometime, code performs several method calls, `passing the same parameter` between those calls, or maybe we have a pipeline that's executing several computations `on a particular dataset`
* a problem that can occur when one of those parameters is **`an IEnumerable`**

* Because of the use of methods that `cause immediate execution` -> **`multiple enumerations the IEnumerable`** - data is retrieved `numerous times from the data source`
* If your data source is a `List or any other source in memory`, this is not a problem
* if your data source is a file, database, ... -> potentially a **`significant performance problem`**
* As the size of the data returned by the data-source increases, the execution time increments very fast for the multiple enumerations scenario

* **`deferred all execution`** inside the pipeline is the preferred solution
* Try to avoid `query evaluation` if possible unless there is some clear advantage not to do it

#### The consistency problem
* sometimes we might need to **`force query evaluation`** after fetching data from a data source
* If we can't be sure that someone will perform an operation that will `force query evaluation more than once`, it might be **`a good idea to use .ToList()`** before sending the collection to another part of your code so to avoid these two situations.

* If data source is a database. And if we have multiple enumerations, that results in numerous calls to the database to retrieve data at different moments in time. And performing operations on a particular set of records
* in one of the **`calls to the data source will return different results because between requests, someone may have updated the data source`**
* `unexpected consequence` - Someone could perform an update to the same dataset you are working with, which results in you manipulating different datasets where you thought it was always the same




