## 4. what is 'indexer' ?

## 14. What are generics in C#.NET
* a feature that allows you to create `reusable code` by parameterizing classes, methods, and other constructs with one or more types
* Generics are commonly used in collections to create flexible and type-safe data structures
 
## 15. "Nullable" type in C#
* Nullable<T> type allows to assign null values to value types.
* only be used with value types and structs, not reference types

* **`"?"`** operator is a `shorthand` syntax for Nullable types (_after underlying value type_)

* Use the **`HasValue`** property or the null-conditional operator (?.) to check whether a value is assigned to a nullable type or not
* Use **`Value`** property to get the value of nullable type (_but need to check first_)
```
if (nullableInt.HasValue)
{
    int value = nullableInt.Value;
    Console.WriteLine("The value of the nullable type is: " + value);
}
// or
int value1 = myNullableInt?.Value;
```

* The null coalescing operator (**`??`**) allows you to provide a default value if the nullable type is null
```
int? myNullableInt = null;
int value = myNullableInt ?? 1;   // value will be "1" since myNullableInt is null
```
* `Static Nullable class` is a helper class to compare nullable types

## 8. object vs dynamic ?
* -> **object** (System.Object ) - is a statically typed that is **`base type for all other types in C#`**, so it can **`store any value and any other type can be implicitly converted to it`**
* -> **dynamic** - is a dynamically typed that is **`not known until runtime`**; so it is assumed to **`support any operation at compile-time`** and **`dynamic errors are only caught at runtime`**
* => đối với tôi sự khác biết nằm ở chỗ tôi sẽ sử dụng chúng; tôi không sử dụng kiểu object nhiều nhưng thường sử dụng dynamic khi tôi muốn truy cập vào properties và methods của 1 biến mà tôi đã biết rõ cấu trúc

```cs
static void Main()
{
    dynamic value = "Hello, World!"; // Compiler does not check what type 'value' is
    Console.WriteLine(value.Length); // Works fine because at runtime, 'value' is a string so it will have ".Length" property

    value = 100; // Now 'value' holds an int
    Console.WriteLine(value + 50); // Works fine because at runtime, 'value' is an int

    value = DateTime.Now; // Now 'value' holds a DateTime object
    Console.WriteLine(value.Year); // Works fine because at runtime, 'value' is a DateTime

    value = new { Name = "John" }; // Anonymous type
    Console.WriteLine(value.Name); // Works fine at runtime

    dynamic obj = 100;
    Console.WriteLine(obj.Length); // ❌ Runtime error! 'int' does not have 'Length'
}
```

## 17. Data types in C#?
* -> **`Value Types`** (Stored in Stack) - numeric type, boolean, character, Structs & Enums
* -> **`Reference Types`** (Stored in Heap) - class, string, Arrays, delegates, interfaces, object
* -> **`Pointer Types`** (Unsafe Code) - int*, char*, ...
* -> **`Nullable Types`** (? for Value Types) - int? allows null values

## 13. What are value types and reference types?
* **`Value type`** - store the actual data directly in memory
* -> save data  directly into Stack
* -> when we assign a value type variable to another, it creates a copy of the data (_changes to one won't affect the other_)
```
Enum, Struct, int, float, boolean,...
```
* **`Reference Type`** - store a reference (memory address) to the actual data instead of the data itself (_means it can contain  a null-reference - don't refer to anything_)
* -> save **pointer reference** in Stack, but value of variable are in Heap
* -> when we assign a reference type variable to another, both point to the same memory location
```
Class, Object, Array, Indexer, Interface
```

## 4. object initializer
* -> allows to **initialize the value to the fields or properties** of a class at **the time of creating an object** **`without calling a constructor`**
```cs
class Geeks {
    public string author_name { get; set; }
    public int author_id { get; set; }
    public int total_article { get; set; }
}
 
class GFG {
    static public void Main()
    {
        // -> "Geeks" class doesn't contain any constructor
        // -> we simply create the object
        // -> then simply initialize fields at the same time using "curly braces" (an object initializer)
        Geeks obj = new Geeks() {
            author_name = "Ankita Saini",
            author_id = 102,
            total_article = 178};
         
        Console.WriteLine("Author Name: {0}", obj.author_name); // Author Name: Ankita Saini
        Console.WriteLine("Author Id: {0}", obj.author_id); // Author Id: 102
        Console.WriteLine("Total no of articles: {0}", obj.total_article); // Total no of articles: 178
    }
}
```

* -> the compiler will compiles the above program it assign the values to the object like this:
```cs
Geeks __geeks = new Geeks();
__geeks.author_name = "Ankita Saini";
__geeks.author_id = 102;
__geeks. total_article = 178;

Geeks obj =  __geeks; 
```

## 12. static ?
* -> define a member that **`belongs to the class itself rather than an instance of the class`**
* -> a "static" field can be **`changed at runtime`** and can hold any type of data
* -> a static method can only access other static members

```cs
class Example
{
    public static int StaticValue = 10; // Belongs to the class

    public static void PrintValue()
    {
        Console.WriteLine($"Static Value: {StaticValue}");
    }
}

class Program
{
    static void Main()
    {
        Example.PrintValue(); // 10
        Example.StaticValue = 20; // Can be modified
        Example.PrintValue(); // 20
    }
}
```

## 11. const - readonly - init
* _all prevent modification_

### const
* used to declare **compile-time constant** for a **`field`** - fields value **`must be assigned at declaration`** and **`cannot be changed later`**
* const fields are **`implicitly static`**, meaning they can be accessed using the type name without creating an instance of the class
* const fields can only hold **`simple value types`** (_primative value and strings_) 
```cs
class Example
{
    public const double Pi = 3.14159; // Must be initialized at declaration
}
class Program
{
    static void Main()
    {
        Console.WriteLine($"Value of Pi: {Example.Pi}");
        // Example.Pi = 3.14; // ERROR: Cannot modify a const field
    }
}s
```

### readonly:
* a readonly field represents a **runtime constant** for a **`field`** - field value is **evaluated and assigned at runtime**
* can only assigned at the time of **`declaration`** or **`within the constructor`** of class and **`cannot be modified after`** the construction 
* can hold **`any type of value`** (_including complex objects_)
```cs
private readonly double _pi = 5;
private readonly double _radius;

public Circle(double radius)
{
   _pi = 3.14159;
   _radius = radius;
}
```

### init
* -> use for **`immutable properties`** - can only be assigned value at **`declaration`**, in **`constructor`** or **`object initializer`** and **`cannot be modified after object creation`**
* -> can hold **any type of value**

## 3. Different between class, struct, record?

## struct
* -> is **`value type`** (_**immutable** and **value-based equality**_)
* -> suitable for **`small, lightweight, performance-critical data structures`** (_but if **struct** become large, this might be a problem when copying entire object value, instead of just reference like class_)
* -> limited in OOP - **`no Inheritance (Polymorphism)`**
* _built-in C# struct: int, double, bool, DateTime, TimeSpan, Guid_

## class
* -> is **`reference type`**
* -> supports **`full OOP features`** like polymorphism, abstract classes, and virtual methods
* -> **`less performance`** - every time a new object is created, it is allocated on the heap, **increasing Garbage Collector (GC) pressure**; also modifying position frequently leads to **memory fragmentation**

## record
* -> a **`reference type`**; is designed for **`immutable`** data models and **`value-based equality`**

```cs
// -----> provides a compact syntax using positional parameters:
public record Employee(string Name, string Position);
```

```cs
// -----> value-based equality
public record Person(string Name, int Age);

var p1 = new Person("Alice", 25);
var p2 = new Person("Alice", 25);

Console.WriteLine(p1 == p2); // True (value-based equality)
```

```cs
// -----> immutability
// makes it easier to work with immutable data because it provides "with" expression for creating new modified instances
var p3 = p1 with { Age = 26 };  // Creates a new object with modified Age
```

### Positional Record
* -> normally, we can define a "record" throught **positional properties** (_**`define properties inside the constructor`** and are **`immutable (init-only) by default`** unless explicitly modified)
```cs
// "Name" and "Age" are 2 positional properties: 
public record Person(string Name, int Age);
```

```cs
// code above the same as:
public record Person
{
    public string Name { get; init; }  // `init` means immutable after initialization
    public int Age { get; init; }

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

### Members
* -> we **`can also define additional properties`** inside a record beside the **positional properties**
```cs
public record Product(string Name, decimal Price)
{
    public decimal DiscountedPrice => Price * 0.9m; // computed extra property
    public string Color { get; set; } = "White"; // mutable extra property (defined with "set;")
}
```

* -> define a custom **constructor** in a record:
```cs
// -> multiple constructor
public record Employee(string Name, int Age, string Address)
{
    public Employee(string name, int age) : this (name, age, "319") { }
    public Employee(string name) : this(name, 21) { }
}

var emp1 = new Employee("Charlie", 30);
var emp2 = new Employee("David");  // Uses the second constructor

Console.WriteLine(emp1); // Employee { Name = Charlie, Age = 30, Address = 319 }
Console.WriteLine(emp2); // Employee { Name = David, Age = 21, Address = 319 }

// -> if not specific constructor, we have to initiate with "object initializer" or specific "a parameterless Constructor"
// -> because positional records don't automatically provide a parameterless constructor

public record Car
{
    public string Model { get; set; }  // Mutable
    public int Year { get; init; }  // Immutable after initialization
}
var car = new Car { Model = "Toyota", Year = 2022 };
car.Model = "Honda"; // Allowed
car.Year = 2023; // Not allowed (init-only)

public record Car
{
    public string Model { get; init; }
    public int Year { get; init; }

    // Parameterless constructor
    public Car()
    {
        Model = "Unknown";
        Year = 2000;
    }
}
var defaultCar = new Car();
Console.WriteLine(defaultCar);  // Car { Model = Unknown, Year = 2000 }
```

* -> define **methods** inside a record
```cs
public record Person(string Name, int Age)
{
    public string GetDetails()
    {
        return $"{Name} is {Age} years old.";
    }
}
var person = new Person("Alice", 25);
Console.WriteLine(person.GetDetails()); // Alice is 25 years old.
```

### 
* -> **Inheritance** 
```cs
public record Person(string Name, int Age);

public record Employee(string Name, int Age, string Position) : Person(Name, Age)
{
    public string GetJobInfo() => $"{Name} works as {Position}.";
}
```

* -> **Abstraction** and **Polymorphism** 
```cs
public abstract record Animal
{
    public abstract string Speak();
}
public record Dog : Animal
{
    public override string Speak() => "Woof!";
}
public record Cat : Animal
{
    public override string Speak() => "Meow!";
}
```

* -> **Encapsulation**
```cs
public record BankAccount
{
    public string AccountHolder { get; init; }
    
    private decimal _balance;
    
    public decimal Balance
    {
        get => _balance;
        private set
        {
            if (value < 0)
                throw new ArgumentException("Balance cannot be negative.");
            _balance = value;
        }
    }

    public BankAccount(string accountHolder, decimal balance)
    {
        AccountHolder = accountHolder;
        Balance = balance; // Uses setter with validation
    }
}
```

## 16. Anonymous type in C#? 
* -> a temporary, compiler-generated type used when we need **a simple object without defining a class**
* -> key features: declared using **var** with object initializer syntax **{}**, properties are **read-only** (immutable), the type is compiler-generated and **only exists in the scope where it was created**

```cs
var students = new[]
{
    new { Name = "Alice", Grade = "A" },
    new { Name = "Bob", Grade = "B" }
};

var highGraders = students.Where(s => s.Grade == "A");

foreach (var student in highGraders)
{
    Console.WriteLine(student.Name);
}
```

## 2. Boxing and Unboxing ?
* -> **`Boxing`** - is a process of converting a Value Type variable (char, int etc.) to a Reference Type variable (object or any interface type implemented by the value type)
* -> **`Unboxing`** - is an explicit conversion process of converting a Reference Type variable to Value Type

```cs - boxing
// Boxing to 'object'
int num = 2020;
object obj = num; // boxing
num = 100;
System.Console.WriteLine(num); // 100
System.Console.WriteLine(obj); // 2020

// Boxing to 'Interface'
interface IExample
{
    void Display();
}
struct SampleStruct : IExample
{
    public void Display()
    {
        Console.WriteLine("Hello from struct!");
    }
}
SampleStruct s = new SampleStruct();
IExample example = s; // Boxing happens here!
```

```cs - unboxing
int num = 23;         // value type is int and assigned value 23
Object Obj = num;    // Boxing
int i = (int)Obj;    // Unboxing
```

## 17. 'Finally' block vs Dispose() method vs 'Finalize' method ?

### Finally
* -> the try-catch-finally block is to **handle Exception** at runtime
* -> the "finally" block to **`ensure a part of code will be ran whether an exception occurred/is handled or not`**
* => by using finally block, we can **clean up resources** 

### Finalize (known as a "destructor")
* -> **`automatically called by the Garbage Collector (GC)`** when an object is no longer in use to  **`clean up unmanaged resources (_e.g., file handles, database connections, sockets, native API calls, COM objects_) before an object is garbage collected`**
* => we do not need to worry about the **Finalize** method unless we are working with **`unmanaged resources`** 
* => we don't need "Finalize" for normal object cleanup; instead if **our class manages unmanaged resources**, it is recommended to implement **`IDisposable`** and use the **`Dispose`** method 
* -> and **`GC.SuppressFinalize(this)`** in **Dispose** to prevent unnecessary finalization (optimize GC performance)
* -> (optinally) only implement "Finalize" as a last resort **if Dispose() might not be called**


## 3. 'string' is reference type or value type?
* -> string is a **`reference type`**, but it **`behaves like a value`** type 
* -> because it is **`immutable`**

* _when you modify a string, a new object is created instead of modifying the original_
* _assigning a string to another does not create a reference to the same object (unlike other reference types)_

## collection in C#

### interface

#### IEnumerable
* -> exposes an enumerator, which supports a simple iteration over a collection

#### ICollection
* -> defines size, enumerators, and synchronization methods for all nongeneric collections
* -> Ex: **Queue**, **Stack** directly implement the ICollection interface
```cs
public interface ICollection : System.Collections.IEnumerable
```

#### IList
* -> collection of objects that can be **`individually accessed by index`**
```cs
public interface IList : System.Collections.ICollection
```

#### IDictionary
* -> a collection of **`key/value pairs`**

### Collections
* -> https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/collections
* -> most collections support adding or removing elements dynamically; notably, **`Array`**, **`System.Span<T>`**, and **`System.Memory<T>`** don't

#### Indexable collections

#### Key/value pair collections

#### Iterators

#### System.Collections.Concurrent
* help to achieve **`thread-safe`** code
* ConcurrentStack<T> , ConcurrentQueue<T> , ConcurrentDictionary<TKey,TValue>

### Array

#### Single-dimensional arrays
* an array is a **`fixed-size`** collection 
* have specific type

#### Multidimensional arrays

#### Jagged arrays

#### Implicitly typed arrays

### ArrayList (legacy)
* -> giống **List<T>** nhưng **`no type-safety`** (các phần tử đều sẽ được lưu với kiểu là **`object`**)
* => required value type to be **boxed** into objects when added and **unboxed** when retrieved; leads to extra memory allocation and CPU overhead
* => only use this in legacy codebases where refactoring to List<T> is not an option

```cs
ArrayList myList = new ArrayList();
myList.Add(10);   // Adding an integer
myList.Add("Hello"); // Adding a string
myList.Add(3.14); // Adding a double

Console.WriteLine(myList[1]); // Output: Hello
```

### List<T> 
* -> a **`Dynamic array`** with fast **`access by index`** (_ensure order_)
* -> more easy to use with many built-in functionalities like .Add(), .Remove(), .Contains(), sorting, searching, and filtering
```cs
List<string> students = new List<string> { "Alice", "Bob", "Charlie" };
students.Add("David"); // Add a new student
students.Remove("Bob"); // Remove a student
Console.WriteLine(students[0]); // Access first student
```

### The Dictionary<TKey, TValue> 
* a collection of **`KeyValuePair<TKey, TValue>`**
* => fast lookup (O(1) time complexity) of values based on unique keys

```cs
Dictionary<int, string> employees = new Dictionary<int, string>
{
    { 101, "Alice" },
    { 102, "Bob" }
};
Console.WriteLine(employees[101]); // Output: Alice
```

### HashSet<T> 
* an **`unordered`** collection of **`unique elements`**
* provides **fast lookup** (O(1) time complexity)
* => a collection with unique elements and fast lookup (_Ex: removing duplicates from a dataset, like storing unique product codes_)

```cs
HashSet<int> uniqueNumbers = new HashSet<int> { 1, 2, 3, 4 };
uniqueNumbers.Add(3); // Duplicate, ignored
Console.WriteLine(uniqueNumbers.Contains(2)); // Output: True
```

### Stack<T> 
* a **`last-in, first-out`** (LIFO) collection of objects
* _no direct access to elements; only the top element can be accessed_
* _fast **push** (adding) and **pop** (removing) operations (O(1) time complexity)_
* => process elements in reverse order, like an undo mechanism (_Ex: implementing undo/redo operations or backtracking algorithms_)

```cs
Stack<string> history = new Stack<string>();
history.Push("Page 1");
history.Push("Page 2");

Console.WriteLine(history.Pop()); // Output: Page 2 (last inserted item)
```

### Queue<T> 
* a **`first-in, first-out`** (FIFO) collection of objects. 
* _no direct access to elements; only first item can be dequeued_
* _fast enqueue (adding items) and dequeue (removing items) (O(1) time complexity)_
* => to process elements in order (_Ex: Task processing/scheduling, message queues, or handling print jobs_)

```cs
Queue<string> tasks = new Queue<string>();
tasks.Enqueue("Task 1");
tasks.Enqueue("Task 2");

Console.WriteLine(tasks.Dequeue()); // Output: Task 1 (first inserted item)
```

### LinkedList - LinkedList<T> 
* **`doubly linked list`** - each node has pointers to previous and next nodes
* _**fast insertions/removals** in the middle of the list (O(1) time complexity)_
* _**slower lookups** compared to List<T> (O(n) time complexity)_
* => frequently insert/remove elements in the middle of the collection (_Ex:  Implementing a browser's back and forward navigation_)

```cs
LinkedList<string> browsers = new LinkedList<string>();
browsers.AddLast("Google Chrome");
browsers.AddLast("Firefox");
browsers.AddFirst("Edge");

Console.WriteLine(browsers.First.Value); // Output: Edge
```

### ConcurrentDictionary<TKey, TValue>
* **`thread-safe`** Dictionary for concurrent applications.
* prevents data corruption when accessed from multiple threads.
* best for scenarios requiring **`highly efficient multi-threaded lookups`**
* => when multiple threads need to read/write key-value pairs simultaneously (_Ex:  Storing active user sessions in a web application_)

```cs
using System.Collections.Concurrent;

ConcurrentDictionary<int, string> userSessions = new ConcurrentDictionary<int, string>();
userSessions.TryAdd(1, "User1");
Console.WriteLine(userSessions[1]); // Output: User1
```

### SortedSet: The SortedSet<T> 
* a `sorted collection` of `unique elements`. 
* It provides efficient operations for adding, removing, and searching elements in sorted order.

### SortedList<TKey,TValue> 
* is an array that consists of `key/value pairs` that are `sorted in an order` 
* `Every key` must be `unique`
* Non-generic của nó là **`SortedList`**

### Hashtable
* consists of `key/value pairs` that are managed using the `hash code` of the particular key
* The key cannot be null however a value can be null
* We can perform dynamic allocation of the memory inside Hashtable

## 'System.String' or 'System.Text.StringBuilder' classes
* offer similar functionality for accessing characters, determining string lengths, and performing common string operations
### System.String
* System.String objects are **`immutable`**, 
* meaning that once created, their values `cannot be changed` 
* Any operation that appears to modify a string actually creates a new string

### System.Text.StringBuilder
* System.Text.StringBuilder objects are **`mutable`** 
* `allow to modify` the contents of the string without creating a new instance (_performance and memory efficiency for frequent changes_)
* provides additional methods and properties specifically for `string manipulation`: _Append, Insert, Remove, Replace,..._

## "using" statement
### Purpose
* ensures that the `resources allocated to an object` are properly `released and disposed` of when they are no longer needed, even if an exception is thrown during the execution of the code
* particularly with objects that implement the **`IDisposable`** interface 

### Mechanism
* It allows you to create a **`block of code`** where the object is instantiated and used
* **`Dispose() method`** is `automatically called` when exiting the block (by reaching the `end of the block` or an `exception` being thrown)

```cs
using (FileStream fileStream = new FileStream("data.txt", FileMode.Open))
{
    // Use the fileStream object to read data from the file
    // ...
}
// The fileStream object is no longer in scope and has been properly disposed
```

### IDisposable 
* provides a mechanism to release unmanaged resources such as `file handles, database connections, or network sockets`

### The "using" directive
```
using System;
using System.IO;
```
* is used for **`namespace import`**
* It allows you to bring `types and members` from a `particular namespace` into your code file
* making them accessible without fully qualifying their names. 
* used at the beginning of a file or within a namespace scope
