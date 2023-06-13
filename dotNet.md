## 2. What is the Common Language Runtime (CLR)
* CLR is the `run-time environment` in the `.NET`(is the basic and Virtual Machine component of the .NET Framework) 
* responsible for managing the execution of .NET programs (regardless of any .NET programming language)
* provides services: memory management, type safety, security, and exception handling, `JIT`

### Just-In-Time (JIT) compilation
* compiles the CIL code into machine code on the fly as the program runs, optimizing performance.

### Process
* Compiler compile C# code -> intermediate language: CIL, MSIL ->  When CIL code is executed, CLR compiles it into machine code -> executed by the processor

* _CIL - Common Intermediate Language_
* _MSIL - Microsoft Intermediate Language_

## 7. What are the advantages of ASP.NET Core over ASP.NET
* `Cross-platform compatibility`: ASP.NET Core is designed to be cross-platform and runs on Windows, macOS, and Linux, 
* `Middleware pipeline`: ASP.NET Core introduces a middleware pipeline that allows developers to easily configure and customize the request processing pipeline
* `Dependency injection`: ASP.NET Core has built-in support for dependency injection
* `Unified MVC and Web API frameworks` - ASP.NET Core combines the functionality of MVC and Web API frameworks into a single programming model
* `Open-source and community-driven`: ASP.NET Core is an open-source framework
* `Cloud-ready and container support`: ASP.NET Core is designed to work well in cloud environments and supports modern deployment techniques such as containerization with Docker

## 3. what is middleware
* a component or software layer that sits between the server and the application
* acts as a bridge, processing and manipulating the incoming HTTP requests and outgoing responses
## Pipeline in MVC


## 9. Nuget package 
* **`NuGet`** is a package manager for the Microsoft development platform, primarily used in conjunction with the .NET Framework and .NET Core
* **`A NuGet package`** is a way to distribute and share code components with other developers - contains compiled code (assemblies) along with other related files (_such as configuration files, documentation, and content files_)

## Import export excel - PDF xài thư viện gì ?
* EPPlus, iTextSharp

## 16. Blazor và Razor ?
