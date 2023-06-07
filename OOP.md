## 1. What is Object-oriented programming
* **`OOP`** is a programming pattern that is built around class and objects (entities)

* **`Classes`** - is the blueprint to create objects have common attribute and behavior

* **`Objects`** - an entity that exists in the real world

* **`Methods`** - behavior of object

* **`Attributes`** - represent the state of an object. Objects contain data stored in the attribute field

### 4 pillars of OOP:
#### Encapsulation
* monitor how the information is being accessed and what operations are performed on it
* triển khai bằng **`access specifiers`**: public, private, protected, internal

#### Abstraction:
* hiding certain details and showing only essential information to the user
* achieved with either **`abstract classes`** or **`interfaces`**

#### Inheritance
* inherit fields and methods from one class to another
* to inherit from a class, use the "derive" term - **`":"`**
* use the **`sealed`** keyword to prevent other classes to inherit 

#### Polymorphism
* means "many forms", perform a single action in different ways
* it occurs when we have many classes that are related to each other by inheritance

* differ in when the program decides which method to use -> Compile Time Polymorphism and Run-Time Polymorphism
##### Compile time Polymorphism
*  the decision on which method to execute is made by the compiler, during the compilation phase `base on number, type, and order of the arguments`

* **`Method overloading`** - having multiple methods with the same name but different parameters in the same class 
* Method Overloading is not possible by only changing the return type of method -> Compile Time Error: method is already defined
##### Runtime polymorphism
* decision on which method to execute is made at runtime, when the method is called on an instance of the derived class (`based on the actual type of the object`)

* **`Method Overriding`** occurs when a `derived class` provides its `own implementation` of a method that is already defined in its `base class`
* The method in the derived class must have the `same name, return type, and parameters` as the method in the base class

* By default, the `base class` method `overrides` the `derived class` method when they share the same name
* to override the base class method -> adding the **`virtual`** keyword to the method inside the base class; using the **`override`** keyword for each derived class methods

### Access specifier/ Access modifier
* **`public`** - allows a member to be accessed from any other code within the _`same assembly`_ or `from other assemblies` that reference it
* **`protected internal`** - allows a member to be accessed within the `same assembly` or `by derived classes in other assemblies`
* **`internal`** - allows a member to be accessed from any code within the _`same assembly`_ but not from other assemblies.
* **`protected`** - allows a member to be accessed within the _`same class` or `by derived classes`_ in any assembly
* **`private protected`** - allows a member to be accessed within the _`same class` or `by derived classes in the same assembly`_
* **`private`** - restricts access to the member within the `same class`; It is the **`default access modifier`** for class members if no access modifier is specified

### Abstract class vs Interface
#### Definition:
* An `abstract class` is `a class` that cannot be instantiated, can contain a mixture of `implemented and abstract members`
* An `interface` defines a contract; Any class or struct that implements that contract must provide an implementation of the members defined in the interface

#### Inheritance:
* A class can `only inherit from a single abstract class` using the _" : abstractClass syntax"_
* A class can `implement multiple interfaces` using the _" : interface1, interface2 syntax"_
* _a class can inherit from an abstract class and implement multiple interfaces simultaneously_

#### Members: 
* `An abstract class` can have `fields, properties, constructors, methods` (`both abstract and non-abstract`), events
* `An interface` can only have `properties, methods, events, and indexers` (`without any implementation`)

#### Default Implementation:
* `An abstract class` can provide `default implementations` for its members.
* `An interface` `cannot` provide any `default implementation` for its members. Implementing classes must define the implementation for all the interface members
 
#### Accessibility:
* `Members of an abstract class` can have `different access modifiers` (implicit or explicit)
* `Members of an interface` are `implicitly public` and cannot have explicit access modifiers

#### Usage:
* `Abstract classes` are commonly used when you want to provide a `common base implementation` and define common behavior among `related classes`.
* `Interfaces` are useful when you want to define a contract that `multiple unrelated classes` can implement. They allow for polymorphism and are helpful when you want to `achieve loose coupling`

## 10. What is the difference between public, static, and void
### static
* declare members (fields, methods, properties, events, etc.) that `belong to the type itself rather than to an instance of the type`

* Static Fields, Static Properties, Static Methods, Static Events, Static Constructors are associated with the class itself rather than individual instances
* tức là chúng có thể "accessed and modified"/ "call"/ "subscribed to or raised" / "initial the type itself" directly on the class without creating an instance

* All instances of the class can access and modify the same value of the static field
* A static constructor is `called only once`, before any instances of the class are created, and it is primarily used for `initializing static fields` or performing other `initialization tasks` related to the class

## 11. const - readonly
* used to declare constants
### const
* const fields are **implicitly `static`**, meaning they can be accessed using the type name without creating an instance of the class
* a const field represents a `compile-time constant` -> value is `evaluated and assigned at compile-time`
* value can be only `assigned` at the time of **`declaration`** of class and cannot be modified afterwards.
* const fields can only hold `simple value types` (numeric types, boolean, characters, strings, etc.) 
* const fields must be initialized with `a constant expression`
```
public const int MaxValue = 100;
```

### readonly:
* a readonly field represents a `runtime constant` -> value is `evaluated and assigned at runtime`
* value can be only `assigned` at the time of **`declaration`** or **`within the constructor`** of class and cannot be modified after the constructor finishes executing
* readonly fields can hold `any type of value`, including complex objects.
* readonly fields can be initialized with `different values for different instances of a class`
```
private readonly double _pi;
private readonly double _radius;

public Circle(double radius)
{
   _pi = 3.14159;
   _radius = radius;
}
```

## 12. ref - out
*  used to **`pass arguments by reference`** rather than by value
*  allow to modify the original variable inside the method and have the changes reflected outside of the method

### ref
* the `variable passed as an argument` **`must be initialized`** before being passed to the method. 
* Inside the method, we can read and modify the value of the variable. 
* Any changes made to the parameter inside the method will **`affect the original variable`**
```
void ModifyValue(ref int number)
{
    number = 10;
}

int value = 5;
ModifyValue(ref value);
Console.WriteLine(value); // Output: 10 
```

### out
* almost like **`ref`**
* but the `variable passed as an argument` does **`not need to be initialized`** before being passed to the method. 
* Inside the method, we **`must assign a value`** to the `out parameter` before the method returns
```
void InitializeValue(out int number)
{
    number = 10;
}

int value;
InitializeValue(out value);
Console.WriteLine(value);  // Output: 10
```
