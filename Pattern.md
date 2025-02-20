## 5. what is RESTful Pattern in ASP.NET Core WebApi ?
* The RESTful pattern is an `architectural style` that is commonly used for designing web services
* provides a set of `principles and constraints` that enable you to build scalable and reliable web APIs
* `RESTful APIs` are built upon HTTP protocols and use standard HTTP methods (GET, POST, PUT, PATCH, and DELETE) to perform operations on resources

* `Resource-based architecture`: RESTful APIs expose resources that can be identified by unique URIs. Each resource is associated with a specific HTTP verb (GET, POST, PUT, DELETE).
* `Stateless communication`: The server does not maintain any client state. Each request from the client must contain all the necessary information for the server to process it
* `Uniform interface`: RESTful APIs typically use a uniform set of `well-defined methods and data formats`. (_In the case of ASP.NET Core WebAPI, the HTTP methods (GET, POST, PUT, etc.) are used, and data is often exchanged in JSON format_)
* `Hypermedia as the engine of application state` (HATEOAS): RESTful APIs can include hypermedia links in the response to guide clients on how to interact with the API (_These links provide information about related resources and available actions_)
* `Caching and scalability`: RESTful APIs can leverage caching mechanisms provided by the HTTP protocol to improve performance and scalability. Clients and intermediate proxies can cache responses based on cache control headers provided by the server. 

## 6. what is SOLID, which case to use it ?
* The SOLID principles are used in object-oriented programming to design systems that are modular, maintainable, and scalable
 
## 25. DI in .NET ?
* -> a design pattern to achieve **`loose coupling`** (Inversion of Control) between class and dependencies - instead of object directly create instance and depend on them, they will receive instance from DI Container

* -> basic steps: 
- use of an `interface or base class` to abstract the dependency implementation; 
- registration of the dependency in a service container (.NET provide IServiceProvider) with specific lifetime with AddScoped, AddSingleton, AddTransitent 
- Inject of the service into the class through Constructor Injection (Most common), Method Injection, Property Injection
* => DI framework create instance of dependency and disposing of it when no longer needed

## Transaction and ACID propterties
### Transaction
* `Transaction` refers to a `logical unit of work` that consists of `mutiple operations` performed on a `database` (insert, update, delete,...)

* Ensure data **`integrity and consistency`** within a database (data modifications, safeguarding against errors, system failures, and concurrent access issues that may arise in a database environment)
* guarantees that a `set of related operations` either succeeds as a whole or fails as a whole
*  If any operation within a `transaction fails`, the `entire transaction` is _rolled back_

* `Transactions` typically adhere to the `ACID properties`

### ACID propterties
* **`Atomicity`**: Transactions are atomic, meaning they are treated as `a single indivisible unit of work`. Either all the operations within a transaction are successfully completed, or none of them are. There is no partial completion of a transaction.

* **`Consistency`**: Transactions ensure that the database remains in a `consistent state` before and after the transaction. The data should satisfy all integrity constraints and business rules defined for the database.

* **`Isolation`**: Transactions are isolated from each other, meaning that the intermediate states of a transaction should not be visible to other concurrent transactions. This prevents interference and maintains data integrity.

* **`Durability`**: Once a transaction is committed and changes are made permanent, they are durable and should survive subsequent system failures or restarts. The changes made during a committed transaction are stored permanently in the database

## Repository and Unit of Work Design Patterns 
### Unit of Work 
* manage multiple operations within **`a single transaction`** to ensure `consistency and atomicity` of changes made to a database
* by grouping related operations into a single unit of work, treat them as a logical transaction
* `Add a middle layer between the Controller and the Repository` -> This class will centralize storage for all related Repository to share 1 instance of DbContext.

* The Unit of Work object is responsible for initiating database operations, tracking changes, and committing or rolling back the transaction
### Repository
* The Repository pattern is a design pattern that provides `an abstraction layer` between DAL (the data access logic) and Business (Business Service Layer)

* encapsulate the `data access operations`, provide a consistent interface (or classes) define the operations for working with data entities (promote separation of concerns)

* interfaces provide a contract for the application to interact with the database without exposing the underlying implementation details (querying the database, updating records)


