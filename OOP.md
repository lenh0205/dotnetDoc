## 1. What is Object-oriented programming
* **`OOP`** is a programming pattern that is built around class and objects (entities)

* **`Classes`** - is the blueprint to create objects have common attribute and behavior

* **`Objects`** - an entity that exists in the real world

* **`Methods`** - behavior of object

* **`Attributes`** - represent the state of an object. Objects contain data stored in the attribute field

### 4 pillars of OOP:
#### Encapsulation
* separate information, logic into groups and control how each group is accessed and what actions can be performed on them. 
* triển khai bằng **`access specifiers`**: public, private, protected, internal

#### Abstraction:
* hiding certain details and showing only essential information to the user
* ensure that client only use a consistence information without caring about what happen under the hood
* achieved with either **`abstract classes`** or **`interfaces`**

#### Inheritance
* all about "reusing consistence code" - means that when classA inherit classB, it may use the available logic it need from class B without rewrite these logic
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
* **`public`** - allows a member to be accessed from any other code in the **same assembly** or **any other assembly that references it**
* **`protected`** - allows a member to be accessed only within the **same class** or **in derived classes even if they are in a different assembly**
* **`internal`** - allows a member to be accessed from any code within the **same assembly** but not from another assembly (unless using `InternalsVisibleTo`)
* **`protected internal`** - a member is accessible in the **same assembly** and accessible **in derived classes even if they are in a different assembly** 
* **`private protected`** - a member can be accessed only within **`the same class or by derived classes that are in the same assembly`**
* **`private`** - restricts access to the member within the **same class**; it is the **`default access modifier`** for class members if no access modifier is specified

* _there is some things worth noting: in c# 11, **`file`** is a special modifier (not access modifier) that applies only to **classes, structs, interfaces, and delegates** to **restricts access to the same source file** (not just the same assembly)_

```cs
// -----> using "internal"

// project: MyLibrary.dll
internal class InternalClass
{
    public void ShowMessage()
    {
        Console.WriteLine("This is an internal class!");
    }
}
public class PublicClass
{
    internal void InternalMethod()
    {
        Console.WriteLine("Internal method called");
    }
}

// project: ConsoleApp.exe (Different Assembly)
using MyLibrary;

class Program
{
    static void Main()
    {
        InternalClass obj = new InternalClass(); // ❌ ERROR: InternalClass is inaccessible

        PublicClass obj = new PublicClass();
        obj.InternalMethod(); // ❌ ERROR: InternalMethod is inaccessible
    }
}

// -----> using "InternalsVisibleTo" - specific another assembly (that reference to origin) that is allow to access internal members

// AssemblyInfo.cs (MyLibrary.dll)
using System.Runtime.CompilerServices;
[assembly: InternalsVisibleTo("TestProject")]

// InternalClass (MyLibrary.dll)
internal class InternalClass
{
    public string GetData() => "Internal Data";
}

// TestProject (Another Assembly)
class Test
{
    static void Main()
    {
        InternalClass obj = new InternalClass(); // ✅ Now accessible due to InternalsVisibleTo
        Console.WriteLine(obj.GetData());
    }
}
```

## 15. "sealed" keyword ?
* -> prevents the **`class from being inherited`**
* -> prevents **`further overriding of a 'inherited virtual members' (method or property)`** in derived classes
* (_Ex: only apply to methods that are overriding a virtual method; prevent method overriding only, not overloading_

```cs
// -----> class
sealed class MyClass
{
}
class DerivedClass : MyClass // throw compile-time error
{
}

// -----> inherited virtual members
class BaseClass
{
    public virtual void Show()
    {
        Console.WriteLine("BaseClass Show");
    }
}
class DerivedClass : BaseClass
{
    public sealed override void Show() // Sealed prevents further overrides
    {
        Console.WriteLine("DerivedClass Show");
    }
}

class AnotherDerivedClass : DerivedClass
{
    public override void Show() // compile time error: cannot override because it's sealed
    {
    }
}
```

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

## 6. what is SOLID, which case to use it ?
*  are five key design principles in object-oriented programming that help create maintainable, scalable, and testable software

### S - Single Responsibility Principle (SRP)
* -> a class should have only one reason to change
* _it should have only one responsibility (e.g., handling business logic, not also handling file operations)_
```cs
// from - a class has two responsibilities:
public class Report
{
    public void GenerateReport() 
    {
        Console.WriteLine("Generating report...");
    }

    public void SaveToFile(string filename) 
    {
        File.WriteAllText(filename, "Report content");
    }
}

// refactoring to - 2 class with 2 responsibilities:
public class Report
{
    public string GenerateReport() 
    {
        return "Report content";
    }
}

public class FileManager
{
    public void SaveToFile(string filename, string content) 
    {
        File.WriteAllText(filename, content);
    }
}
```

### O - Open/Closed Principle (OCP)
* -> a class should be open for extension but closed for modification.
* _we should be able to add new functionality without modifying existing code, typically **`using abstraction`** (interfaces, inheritance, or composition)_
```cs
// from - requiring to modify this class every time a new customer type is added
public class DiscountCalculator
{
    public double CalculateDiscount(string customerType, double amount)
    {
        if (customerType == "Regular")
            return amount * 0.1;
        else if (customerType == "Premium")
            return amount * 0.2;
        else
            return amount * 0.05;
    }
}

// refactoring to - just need to add new "DiscountStrategy" when business changes without modyfying DiscountCalculator
public class DiscountCalculator
{
    // depend on abstraction
    public double CalculateDiscount(IDiscountStrategy discountStrategy, double amount)
    {
        return discountStrategy.ApplyDiscount(amount);
    }
}
public interface IDiscountStrategy
{
    double ApplyDiscount(double amount);
}

public class RegularCustomerDiscount : IDiscountStrategy
{
    public double ApplyDiscount(double amount) => amount * 0.1;
}
public class PremiumCustomerDiscount : IDiscountStrategy
{
    public double ApplyDiscount(double amount) => amount * 0.2;
}
```

### L - Liskov Substitution Principle (LSP)
* -> **Subclasses** should be **`replaceable`** for their **base classes** without affecting functionality.
* _a derived class should honor the contract of the base class (e.g., not remove expected behavior)_
```cs
// from - wrong behavior when try to inherit base class 
// -> có thể là vấn đề là do thằng base class đang bị viết sai; hoặc thằng kìa đang inherit sai base class
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("Bird is flying");
    }
}
public class Penguin : Bird
{
    public override void Fly()
    {
        throw new Exception("Penguins can't fly!");
    }
}

// refactor to - update the base class: 
public abstract class Bird { }

public class FlyingBird : Bird
{
    public void Fly()
    {
        Console.WriteLine("Bird is flying");
    }
}
public class Penguin : Bird
{
    public void Swim()
    {
        Console.WriteLine("Penguin is swimming");
    }
}
```

### I - Interface Segregation Principle (ISP)
* -> **clients** should **not be forced to depend on interfaces** they don't use.
* _instead of one large interface, create smaller, **`more specific interfaces`** tailored to the actual needs_
```cs
// from:
public interface IWorker
{
    void Work();
    void Eat();
}

public class Robot : IWorker
{
    public void Work()
    {
        Console.WriteLine("Robot is working...");
    }

    public void Eat()
    {
        throw new Exception("Robot doesn't eat!");
    }
}

// refactoring to:
public interface IWorkable
{
    void Work();
}
public interface IEatable
{
    void Eat();
}

public class Human : IWorkable, IEatable
{
    public void Work() => Console.WriteLine("Human is working...");
    public void Eat() => Console.WriteLine("Human is eating...");
}
public class Robot : IWorkable
{
    public void Work() => Console.WriteLine("Robot is working...");
}
```

### D - Dependency Inversion Principle (DIP)
* -> high-level modules should not depend on low-level modules; both should **`depend on abstractions`**
* _this promotes loose coupling and allows easier changes and testing_
```cs
// from:
public class LightBulb
{
    public void TurnOn() => Console.WriteLine("Light is ON");
    public void TurnOff() => Console.WriteLine("Light is OFF");
}

public class Switch
{
    private LightBulb _bulb = new LightBulb();

    public void Operate()
    {
        _bulb.TurnOn();
    }
}

// refactoring to:
public interface IDevice
{
    void TurnOn();
    void TurnOff();
}

public class LightBulb : IDevice
{
    public void TurnOn() => Console.WriteLine("Light is ON");
    public void TurnOff() => Console.WriteLine("Light is OFF");
}

public class Switch
{
    private readonly IDevice _device;

    public Switch(IDevice device)
    {
        _device = device;
    }

    public void Operate()
    {
        _device.TurnOn();
    }
}
```


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
// -----> value type
void ModifyValue(ref int number)
{
    number = 10;
}

int value = 5;
ModifyValue(ref value);
Console.WriteLine(value); // Output: 10

// -----> reference type
void ChangeObject(ref Person p)
{
    p = new Person { Name = "New Name" };
}

Person person = new Person { Name = "Old Name" };
ChangeObject(ref person);
Console.WriteLine(person.Name); // Output: "New Name"

// -----> without "ref"
// when we pass a reference type without using "ref" or "out", we are passing a copy of the reference, not the actual reference itself
void ChangeName(Person p)
{
    p.Name = "New Name";  // ✅ This works because we are modifying the object itself.
    p = new Person { Name = "Another Person" }; // ❌ This does NOT affect the original reference outside the method.
}
Person person = new Person { Name = "Old Name" };
ChangeName(person);
Console.WriteLine(person.Name); // Output: "New Name" (Original reference is still pointing to the same object)

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
