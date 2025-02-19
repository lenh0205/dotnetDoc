## 2. What is the Common Language Runtime (CLR)
* -> CLR is the `.NET Runtime environment` (is the basic and Virtual Machine component of the .NET Framework for managing the **`execution of .NET programs`** (regardless of any .NET programming language)
* -> provides: garbage collection, security, and exception handling, `JIT`

### JIT (Just-In-Time) .NET compiler
* at runtime, it translate the **`intermediate code`** into **`native machine code just-in-time`** - optimized machine code specific to OS and processor architecture

### Process
* -> in build process, **`C# Compiler (csc.exe)`** compile **`C# code`** to **`intermediate code (CIL/MSIL)`** 
* -> and package it along with **`metadata`** (information about types, methods, references, ... for .NET Runtime to load and execute the assembly) into **`assemblies`** (like `.exe or .dll files`)

 ->  when running application, lúc này **`CLR`** mới tham dự, nó sử dụng **`JIT Compiler`** để convert intermediate code into **`machine code`** -> executed by the processor (CPU)

* _CIL - Common Intermediate Language_
* _MSIL - Microsoft Intermediate Language_

## 7. What are the advantages of ASP.NET Core over ASP.NET
* `Cross-platform compatibility`: ASP.NET Core is designed to be cross-platform and runs on Windows, macOS, and Linux, 
* `Open-source and community-driven`: ASP.NET Core is an open-source framework

* `Unified MVC and Web API frameworks` - ASP.NET Core combines the functionality of MVC and Web API frameworks into a single programming model

* `Middleware pipeline`: ASP.NET Core introduces a middleware pipeline that allows developers to easily configure and customize the request processing pipeline
* `Dependency injection`: ASP.NET Core has built-in support for dependency injection
* `Cloud-ready and container support`: ASP.NET Core is designed to work well in cloud environments and supports modern deployment techniques such as containerization with Docker

## 3. what is middleware
* a component or software layer that sits between the server and the application
* acts as a bridge, processing and manipulating the incoming HTTP requests and outgoing responses

## Pipeline in MVC


## 9. Nuget package 
* **`NuGet`** is a package manager for the Microsoft development platform, primarily used in conjunction with the .NET Framework and .NET Core
* **`A NuGet package`** is a way to distribute and share code components with other developers - contains compiled code (assemblies) along with other related files (_such as configuration files, documentation, and content files_)

## 10. Garbage collection
* https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals
* -> serves as an **`automatic memory manager`** - manages the **allocation and release of memory** for an application 
* => so that developer don't have to perform memory management tasks - eliminate common problems such as forgetting to free an object and causing a memory leak or attempting to access freed memory for an object that's already been freed

### reason
* -> each time we create a new object, the common language runtime **allocates memory for the object from the managed heap**
* -> as long as address space is available in the managed heap, the **runtime continues to allocate space for new objects**
* -> however, **`memory is not infinite`**, eventually the garbage collector **must perform a collection in order to free some memory**

* -> the **garbage collector's optimizing engine** determines the best time to perform a collection, based upon the allocations being made
* -> when the garbage collector performs a collection, it **`checks for objects in the managed heap that are no longer being used by the application`** and **`performs the necessary operations to reclaim their memory`**

### Garbage collection occurs when
* -> **`the system has low physical memory`** - the memory size is detected by either the low memory notification **from the operating system** or low memory as **indicated by the host**
* -> **`the memory that's used by allocated objects on the managed heap surpasses an acceptable threshold`**, this threshold is continuously adjusted as the process runs.
* -> **`the 'GC.Collect' method is called`** - in almost all cases, we don't have to call this method because the garbage collector runs continuously (_this method is primarily used for unique situations and testing_)

### Force garbage collector to run
* https://softwareengineering.stackexchange.com/questions/276585/when-is-it-a-good-idea-to-force-garbage-collection
```cs
// explicit force garbage collector to run (but not recommended)
GC.Collect();
GC.WaitForPendingFinalizers();
```

## 25. DI in .NET ?
* -> a design pattern to achieve **`loose coupling`** between classes - instead of object directly create instance and depend on them, they will receive instance from DI Container
* -> 3 main types of DI: Constructor Injection (Most common), Method Injection, Property Injection
* -> basic steps: define interface; define service implement interface; register to IServiceCollection and specific lifetime with AddScoped, AddSingleton, AddTransitent 

## 19. Import export excel - PDF xài thư viện gì ?
* EPPlus, iTextSharp

## 16. Blazor và Razor ?
