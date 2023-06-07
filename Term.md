# Model binding
* a process in ASP.NET Core MVC 
* maps the data from an incoming request to the parameters of an action method or to properties of a model object

# Serializing
* `converting an object or data structure into a format that can be easily stored, transmitted, or reconstructed later`
* In the context of ASP.NET Core MVC, serializing refers to converting the data in a view or response object into a suitable format for transfer over the network (send back to client)
* The most common serialization format used in ASP.NET Core MVC is JSON
* For example, in JSON serialization, the object's properties are typically converted into JSON key-value pairs
* ASP.NET Core MVC provides built-in like `Newtonsoft.Json (Json.NET)` or the `System.Text.Json`

# query result 
* represents the data returned by executing a query 
* query result can include one or more rows, each row containing a set of values representing the data that satisfies the query criteria
```
IQueryable<Customer> query = DbContext.Customers.Where(c => c.Age > 25);
var result = query.ToList();
// "result" variable represents the query result - holds the actual data based on the executed query
// "query" is a representation of the query itself
```

# Virtual table
* also known as a derived table
* is a table-like structure that is created dynamically from a query result or a combination of tables in a database
* exists only in memory

