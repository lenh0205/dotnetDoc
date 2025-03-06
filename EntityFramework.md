## 8. what is EntityFrameWork ?

* Entity Framework is an `ORM framework` to simplify the process of database interaction by providing a higher-level abstraction over the underlying database 
* -> work with data in terms of objects and classes rather than directly dealing with database tables, queries, and SQL statements
* Entity Framework enables you to define your application's `data model` using classes and relationships, and it takes care of `mapping` these entities to the database tables, performing `CRUD` operations, and managing the underlying `database connections`

## 10. Có mấy cách làm việc với db trong EF 
* -> **`codefirst`**
* -> **`databasefirst`**

## 9. Briefly present Lazy Loading, Eager Loading, Explicit Loading in Entity Framework

### Usage
* -> In Entity Framework Core, **`lazy loading is disabled by default`**; to enable lazy loading, we need to use **`proxies`** and **`mark navigation properties as virtual`** 
* -> so in default case, 

```cs
// ----------> Enable 'Lazy loading'

// -----> update DbContext
using Microsoft.EntityFrameworkCore.Proxies; // install this NuGet package for lazy load proxy

public class MyDbContext : DbContext
{
    public DbSet<Order> Orders { get; set; }
    public DbSet<Customer> Customers { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder
            .UseSqlServer("YourConnectionString")
            .UseLazyLoadingProxies(); // Enable lazy loading
    }
}

// -----> Ensure navigation properties are 'virtual'
public class Order
{
    public int Id { get; set; }
    public string OrderNumber { get; set; }

    // Must be virtual for lazy loading to work
    public virtual Customer Customer { get; set; } // one-to-one relationship
}
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
}

// ------> usage
var order = db.Orders.FirstOrDefault();
var customer = order.Customer; // loaded automatically without calling .Include() or .Load()
```

```cs
// -> to strict control over when related data is loaded - globally disable lazy loading in DbContext
// -> we can use "ChangeTracker.LazyLoadingEnabled = false;"
public class MyDbContext : DbContext
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        ChangeTracker.LazyLoadingEnabled = false; // Disable lazy loading
    }
}

public class MyDbContext : DbContext
{
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        ChangeTracker.LazyLoadingEnabled = false; // Disable lazy loading
    }
}

// Usage
var customer = order.Customer; // "null" because related data is not loaded automatically
```

### Example
```cs - VD: User with Order is one-to-many
public class User
{
    public int Id { get; set; }

    public virtual ICollection<Order> Orders { get; set; }
}
public class Order
{
    public int Id { get; set; }

    public int UserId { get; set; }
    public virtual User User { get; set; }
}
```

#### Lazy Loading: 
* `Related entities` are loaded from the database **`on-demand`** when `accessed for the first time`

* optimize performance by reducing the amount of data retrieved from the database upfront
* may lead to additional database queries if not used carefully (lead to the N+1 problem)
```cs
var user = dbContext.Users.FirstOrDefault(u => u.Id == 1);
var orders = user.Orders; //The orders are loaded from the database at this point
```

#### Eager Loading
* `Related entities` are loaded from the database **`in a single query`** along with the main entity
* By specifying the `navigation properties` to be included using the **`Include`** or **`.ThenInclude()`** method 

* minimize the number of database round-trips (useful when we know we'll need the related data)
* may **slow down performance** (**`memory usage`** and **`query execution time`**) when it come to a large or unnecessary of data fetching
* may also cause a heavy generated SQL queries with **`multiple JOIN`** in ** or **`Cartesian explosion`** (duplicate data in the result set) when eager loading multiple related entities
```cs
var user = dbContext.Users.Include(u => u.Orders).FirstOrDefault(u => u.Id == 1);
var orders = user.Orders;  // The orders are already loaded along with the user in a single query
```

#### Explicit Loading: 
* `control over when and which related entities are loaded`
* use the **`Load`** method (with Reference or Collection) on a navigation property when needed

* useful for deferring the loading of certain related data until it is explicitly requested
* _explicit ở đây tức là khi nào load thì hãy nói rõ bằng việc s/d method Load()_
```cs
var user = dbContext.Users.FirstOrDefault(u => u.Id == 1);
dbContext.Entry(user).Collection(u => u.Orders).Load();  // Explicitly load the orders for the user
var orders = user.Orders;  // The orders are now loaded for the user
```

### Problem (using Lazy loading)
```cs
var users = dbContext.Users.ToList(); // Lazy loading
foreach (var user in users)
{
    // access "user.Orders" in here
}
// each user in foreach loop, a separate database query will be executed to load the orders
```

* fix with `Eager Loading`
```cs
var users = dbContext.Users.Include(u => u.Orders).ToList();
// ...
```

* fix with `Explicit loading` 
```cs
var users = dbContext.Users.ToList();
foreach (var user in users)
{
    // Explicitly load the orders for the current user
    dbContext.Entry(user).Collection(u => u.Orders).Load();

    // access "user.Orders" in here 
}
```


## 17. viết câu LINQ 1 cách tối ưu ?

## 23. AsNoTracking?
https://honeycombsoft.com/improve-performance-in-your-dot-net-app/
https://learn.microsoft.com/en-us/ef/core/querying/tracking
https://www.krisvandermast.com/post/2018/02/03/Use-AsNoTracking-to-speed-up-Entity-Framework-performance.html
https://www.devtrends.co.uk/blog/avoid-asnotracking-and-include-when-querying-using-entity-framework-in-asp.net
https://stackoverflow.com/questions/12211680/what-difference-does-asnotracking-make
https://nicolas-rfontes.medium.com/why-we-need-to-use-asnotracking-in-net-c-entityframework-cc2bbdb83a58
https://goatreview.com/improve-reading-query-with-asnotracking/
https://medium.com/@satyaprakashsahoo.blog/what-is-the-use-of-asnotracking-in-entity-framework-cb6c453d1f1#:~:text=AsNoTracking%20is%20a%20method%20in,row%20in%20the%20result%20set.

* -> improve performance
*  By default, all queries that return model objects from DB are trackable (unless you changed default behaviour with AutoDetectChangesEnabled property)
*  When the data context retrieves data from the database, the Entity Framework places the retrieved objects in the cache and monitors changes that occur to those objects until it uses the SaveChanges() or SaveChangesAsync() method, which commits any changes to the database
*  But we don’t always need to track changes. For example, we just need to display data for viewing.
* => When AsNoTracking() is applied, the data returned from the request is not cached, that is, the request becomes untracked
* => This means that the Entity Framework does not perform any additional processing and does not allocate additional space for storing objects retаrieved from the database.
* => therefore, such queries are faster

```cs
// compare performance (speed of execution and memory usage) between without "AsNoTracking" and with "AsNoTracking"
[MemoryDiagnoser]
public class MemoryBenchmarkerDemo
{
    [Benchmark]
    public List GetUsers()
    {
        using (AppDbContext db = new AppDbContext())
        {
            return db.Users.ToList();
        }
    }

    [Benchmark]
    public List GetUsersWithNoTracking()
    {
        using (AppDbContext db = new AppDbContext())
        {
            return db.Users.AsNoTracking().ToList();
        }
    }
}

Method    Mean    Error     StdDev    Gen 0    Gen 1    Gen 2    Allocated
GetUsers    17.000 ms    
GetUsersWithNoTracking
```

```cs
// can not update any item or collection that we received with AsNoTracking,
// because Entity Framework does not store these objects in the cache

using System.Linq;
using Microsoft.EntityFrameworkCore;

namespace AsNoTrackinsExample
{
    class Program
    {
        static void Main(string[] args)
        {
            using (AppDbContext db = new AppDbContext())
            {
                var user = db.Users.AsNoTracking().FirstOrDefault();
                user.Age = 22;
                db.SaveChanges(); // Age will not be updated 
            }
        }
    }
}
```

```cs
// ChangeTracker - manage the tracking of an object and provide tracking information

// -> disable tracking in general for the context object
using (AppDbContext db = new AppDbContext())
{
    db.ChangeTracker.AutoDetectChangesEnabled = false;
    // ...
}

// -> find out how many objects are currently being tracked
using (AppDbContext db = new AppDbContext())
{
    var users = db.Users.ToList();

    int count = db.ChangeTracker.Entries().Count();
}
```

# Enumerations in C# with IEnumerable & IEnumerator
* modern programming languages makes it very easy to iterate collections
* `iterations` are within two interface: **`IEnumerable and IEnumerator`**
```cs
public static void Main()
{
    var simpleList = new List<String>(){ "Hello", "to", "Medium" };
    foreach (var item in simpleList)
    {
        Console.WriteLine(item);
    }
}
// => Output: 
// Hello
// to
// Medium
```

## IEnumerable
* an _IEnumerable or generic counterpart IEnumerable<T>_ represents an **`iterable collection`** - exposes a certain set of methods
* Well-known examples for iterable collections (therefore also implementers of IEnumerable) are _List<T>, Dictionary<T>,_ **`generator blocks`** created with the **`yield`** keyword

* The _IEnumerable_ requires _implementers_ to offer exactly one method, called **`GetEnumerator()`**
* Also, a **`foreach`** block can only be used in combination with an `IEnumerable/IEnumerable<T>`

## IEnumerator
* The _IEnumerator/IEnumerator<T>_ is the main glue to get the iteration going
* It is essentially telling the collection to deliver an item, and move on accordingly

* An IEnumerator has 2 key properties and methods:
* -> **`Current`** - This property delivers the `item` at the position the iteration is `currently pointing` to
* -> **`MoveNext()`** -> This method `moves the "pointer"` of the iteration to the next element. The `return value` of this method indicates whether there are more items to consume or whether the end of the iteration was reached.
* -> The collection might also offer the **`Reset()`** method, although this is not guaranteed and will `throw a NotSupportedException when working with a generator block` (also not necessary for simple iterations)

##  how it works altogether
* the _IEnumerator<T> implementation_ for a _plain List<T>_ is pretty straightforward. 
* `MoveNext()` will move the list to the next item, and `Current` will return just that item
* the code also contains some sanity checks to verify an item at the requested index is present

* The `foreach` section is turned into:
```cs
List<string>.Enumerator enumerator = simpleList.GetEnumerator(); // deliver underlying IEnumerator
try
{
      while (enumerator.MoveNext())
      {
           string current = enumerator.Current;
           Console.WriteLine(current); // our user code
      }
}
finally
{
      ((IDisposable)enumerator).Dispose();
}
```
* The List<string> implementing the IEnumerable<string> interface is asked to deliver it’s underlying IEnumerator<string>
* The `MoveNext()` method is called within a while loop -> the MoveNext() method _returns false once the collections was iterated over_ and therefore also `supplies the break condition` for the while loop.
* Inside the while loop body, the `Current` property is requested.
* Now our user code will execute.
* The IEnumerable<T> interface also `inherits` from **`IDisposable`** so _enumerators which need to dispose_ have to chance to do so. However the implementer is  also `free to leave the Dispose() method free`


# LINQ deferred execution

## How LINQ is so fast ?
* Even large queries execute in pretty decent time; despite seemingly doing a lot of iterations
* The answer is **`Deferred Execution`** 
* this means that a query (no matter how big) is **`only executed/ materialized`**, when **`explicitly told`** to do so. 
* Up to the point in time this execution is triggered, no enumeration of a query is done at all, and **`the query is really only that a query`**

## The Query
```cs
var fruitList = new List<string>(){ "Banana", "Apple", "Cherry", "Apricot"};

var fruitsWithA = fruitList.Where(fruit => {
    Console.WriteLine("Filtering...");
    return fruit.StartsWith('A');
});

foreach (var fruit in fruitsWithA)
{
    Console.WriteLine(fruit);
}

// Output:
Filtering...
Filtering...
Apple
Filtering...
Filtering...
Apricot
```
* the **`filtering and iteration`** are `not a sequential process`, but rather **`work together`**
* LINQ cleverly uses the **`IEnumerator & IEnumerable combination`** to delay (defer) the actual execution
* primary things responsible for LINQs great performance

## LINQ under the hood
* The `.Where method` is an extension method residing in the `Where.cs file` (https://source.dot.net/#System.Linq/System/Linq/Where.cs,a75569430a96c893)
* in Where.cs file, LINQ does some checks on the **`IEnumerable<TSource> object`** (parameter)
* depending on the type of the object, picks an **`appropriate Iterator object`**
* our data is a `List<TSource>`, so LINQ will pick the _WhereListIterator<TSource>_ return value

* the WhereListIterator<TSource> _implements_ the **`Iterator<TSource> base class`** (a base class for a lot of LINQ iterators) and **`the IEnumerable<TSource>`** as well as the **`IEnumerator<TSource>`** interface
* It also stores the **`predicate`** as well as the **`source`**, but does not yet act on it -> allows the execution to take place in a **`deferred fashion`**

* In true LINQ fashion, the IEnumerable<T> provided in form of the WhereListIterator<T> can now be used to further build on top (e.g. by adding .Select() clauses) but as of yet it still only holds up as a query blueprint
```
// tức là thực tế câu query có thể mở rộng giống như này:
IEnumerable<int> query = numbers.Where(condition).Select(condition);
// "numbers" is "IEnumerable<int> provided
//  ".Where" creates a new IEnumerable<int> called WhereListIterator<int> -> serve as query blueprint
//  the "query" variable represents the LINQ query, nothing hasn't executed yet
```

* when we **`materialize`** the query (actual iteration over the IEnumerable<T>):
* For a LINQ query, materialization usually comes with `.ToList(), .ToArray()` (method starts with a “.To” already signals that it will materialize the query)

* we are using a simple **`foreach`** loop -> it'll compile to:
```cs
IEnumerator<string> enumerator = enumerable.GetEnumerator();
try
{
    while (enumerator.MoveNext())
    {
        string current = enumerator.Current;
        Console.WriteLine(current);
    }
}
finally
{
    if (enumerator != null)
    {
        enumerator.Dispose();
    }
}
```
* The `enumerator` of WhereListIterator<TSource> gets grabbed. This is coming from the base class Iterator<T>, and depending on some conditions like enumeration state or thread id, either returns `this` or returns `a clone of the iterator`
* `MoveNext()` is called. This will retrieve the `source enumerator`. In our case, this retrieves the `iterator of the fruitList variable` (_but for complex Linq queries, this will retrieve the enumerator of the previous call in the chain_)
* Now, the `source collection` is enumerated over. For every source item, we will run the `predicate` (as we are in a .Where() method), and if the predicate is matched, set the `Current` item the foreach-generated enumeration picks up as well as return true. If the predicate does not match, we skip the item.
* Imagine the whole process as a stream. The source enumeration returns an item, then pauses. Then, our enumeration works on that item, and returns it to the top-most caller in the chain. Once the caller is done, we will move to the next item, and essentially pass this call all the way down to the source collection the LINQ query operates on (_It is important to understand that this “stream” can act over a lot of layers_)
* For every matched item, we output the matching string to the Console.

# Types of Entities in Entity Framework
## POCOs - Plain Old CLR Objects
* The classes that we create in Model folder, are called `POCOs`
* contain only the state and behavior of the application and they don’t have the persistence logic of the application
* The main benefits of POCO's are really used when you start to use things like the repository pattern, ORM's and dependency injection

* VD: when we create an ORM (let's say EF) which pulls back data from somewhere (db, web service, etc.), then passes this data into objects (POCO's). Then if one day we decide to switch over to nHibernate from EF, we should not have to touch your POCO's at all, the only thing that should need to be changed is the ORM

## Dynamic Proxy Entities (POCO Proxy)
