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

## Import export excel - PDF xài thư viện gì ?
* EPPlus, iTextSharp

## 16. Blazor và Razor ?
