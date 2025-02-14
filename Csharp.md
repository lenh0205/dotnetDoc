## 3. Different between class, struct, record ?

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

## 16. Anonymous type in C#? 

## 2. Boxing and Unboxing ?

## 3. 'string' is reference type or value type?

## 1. IEnumerable
* **`System.Collections.Concurrent`**

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
* an array is a fixed-size collection 
* have specific type

#### Multidimensional arrays

#### Jagged arrays

#### Implicitly typed arrays

### ArrayList (legacy)
* -> giống mảng nhưng **`size không cố định`** 
* -> các phần tử đều sẽ được lưu với kiểu là **`object`**
* => nên có thể thêm, xoá, sửa phần tử với các kiểu khác nhau; nhưng không thể support LINQ queries

### List<T> 
* ->  **`a generic collection`**
* -> ensuring **`type safety`** at compile time
* -> hỗ trợ nhiều tính năng

### Collection<T>
* Generic collection
* provides a basic collection of elements without any specific ordering guarantees

### Dictionary -The Dictionary<TKey, TValue> 
* a collection of `KeyValuePair<TKey, TValue>`
* allows efficient lookup and retrieval of values based on `a unique key`

### LinkedList - LinkedList<T> 
* a `doubly linked list` of objects of type T. 
* It allows efficient insertion and removal of inserting or removing items `at any positions`

### Stack: The Stack<T> 
* a `last-in, first-out` (LIFO) collection of objects.
* Elements are added and removed from the top of the stack
* Non-generic của nó là **`Stack`**

### Queue - The Queue<T> 
* a `first-in, first-out` (FIFO) collection of objects. 
* Elements are added to the end of the queue and removed from the beginning
* Non-generic của nó là **`Queue`**

### HashSet - The HashSet<T> 
* an `unordered collection` of `unique elements`
* It provides fast insertion, removal, and lookup operations

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

## System.String or System.Text.StringBuilder classes
* offer similar functionality for accessing characters, determining string lengths, and performing common string operations
### System.String
* System.String objects are **`immutable`**, 
* meaning that once created, their values `cannot be changed` 
* Any operation that appears to modify a string actually creates a new string

### System.Text.StringBuilder
* System.Text.StringBuilder objects are **`mutable`** 
* `allow to modify` the contents of the string without creating a new instance
* provides additional methods and properties specifically for `string manipulation`: _Append, Insert, Remove, Replace,..._

## "using" statement
### Purpose
* used for `resource management` 
* particularly with objects that implement the **`IDisposable`** interface 
* ensures that the `resources allocated to an object` are properly `released and disposed` of when they are no longer needed, even if an exception is thrown during the execution of the code

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
