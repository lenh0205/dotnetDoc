# MVC - Model View Controller
* `Design pattern` for efficiently relating the `user interface` to underlying data models

## Workflow of MVC architecture
* The client (the browser) sends a request to server (internet information server) 
* the Server finds a Route specified by the browser in its URL 
* through Route, The request goes to a specific Controller 
* the controller communicates with the Model to fetch/store any records 
* Views are populated with model properties
* controller gives a response to the IIS7 server, as a result server shows the required page in the browser.

## Model
* responsible for `maintaining the data and behavior` of the application - can be called the **`Business Layer`**
* The `classes in Models` have properties and methods which `represents the application state` and rules; and are called Entity classes or Domain classes
* Models also contain the following type of logics: Business logic, Data access logic, Validation logic
### POCOs - Plain Old CLR Objects
* The classes that we create in Model folder, are called `POCOs`
* contain only the state and behavior of the application and they don’t have the persistence logic of the application
* The main benefits of POCO's are really used when you start to use things like the repository pattern, ORM's and dependency injection

* VD: when we create an ORM (let's say EF) which pulls back data from somewhere (db, web service, etc.), then passes this data into objects (POCO's). Then if one day we decide to switch over to nHibernate from EF, we should not have to touch your POCO's at all, the only thing that should need to be changed is the ORM

## View
* This layer represents the HTML markup that we display to the user - can be called `Display Layer`
* View is the `user interface` in which we render the Model in the form of interaction. 

* In the Views folder, for every Controller there is a single view folder and for each action method in the controller there is a view in that view folder
* also a Shared folder represents the existence of Layout and Master Page in ASP.NET MVC.

## Controller
* When the user interacts with the user interface; Controller’s responsibility is to handle the generated HTTP request
* This layer can also `handle the input to the database` or `fetches the data from the database records` - can be called **`Input layer`**

* The name of the controller ends with the word Controller 
* Every controller inherits from `ControllerBase` class which implements the `IController` interface (System.Web.Mvc) 
* There is `Execute` method in IController class that gets executed when the request comes to controller
* the method takes an object of `RequestContext` class, 
* _RequestContext class_ encapsulates the information about the HTTP request that matches the defined route, with the help of **`HttpContext`** and **`RouteData`** properties

## Separation of Concerns (SoC)
* a design principle for separating a computer program into distinct sections -> each section addresses a separate concern
* MVC implements the principle of SoC as it separates the Model, View and Controller

## Violation of SoC
### View
* When we use **`business logic in View`** then it is a violation because are only for displaying the HTML markup to the browser, not to execute the logic
```
@if (user.Role == "Admin") { }  
else { }   
```
### Controller
* Controllers are just to `handle the incoming HTTP request`
* When we use **`business logic in Controller`**, like when controller executes the database logic to fetch/store the information in the database, it is a violation of single responsibility principle as well
* Use **`Service layer`** `to implement this business logic` in the web application
```
publicActionResult Index() {  
    If(ModelState.IsValid) {  
        dbContext.Employees.Add(model);  
    }  
    Return RedirectToAction(“Saved”);  
}  
```
### Model
* Models contain `classes of the domain or business`
* When we `use ViewBag and ViewData` to pass the data from controller to view instead of using ViewModels to display data in view (**`ViewModels`** are classes that bind **`strongly typed views`**)
* Use **`Repository pattern`** to access the data from the database
```
publicActionResult Contact() {  
    ViewBag.Message = "Your contact page.";  
    return View();  
}  
```

# Pipeline in MVC

# Model in ASP.NET Core MVC
## Domain Model (Data Model, Entity Model)
* represents an Entity in the database. 
* Domain models typically have a one-to-one relationship with a table in the database. 
* The Domain Model is related to the data access layer (DAL) in the application.
* receives data from a database or somewhere stored by the data access layer (DAL)

## ViewModel
* ViewModel is used in MVC, MVP, and MVVM design patterns

* `separating the presentation logic` from the data and business logic.
* provides a representation of the data tailored to the view's needs
* ViewModels are used to `shape multiple entities` from one or more models into a `single object`

* Its main tasks include:
* `transforming data` from the Model into a suitable format for presentation, 
* providing properties and methods for data binding with the View, 
* implementing view-specific logic
* performing data validation

### Edit Model / Input Model
* represents the data needed for the user to change or add

# Passing Data From Controller To View
* Controller is the class whose responsibility is to handle all the HTTP requests and then execute appropriate View

## Strongly typed data (ViewModel)
* make a `model class`
* then populate its properties with some data
* then pass that data to the strongly typed view
* And a strongly typed View can be made by using **`@model`** declaration in the View file having .cshtml extension
```
// in Controller:
List<Employee> emp = new List<Employee> {...};
return View(emp);  

// in View:
@model IEnumerable<Models.Employee> // use IEnumerable interface because of passing a list of data 
...
@foreach (var employee in Model) {...}
```

##  With ViewData
* ViewData is a `dictionary` for the view data having the type of `ViewDataDictionary`
* it is one of the properties of `ControllerBase` class

* ViewDataDictionary that contains the `Key-Value pair` where the key must be a `string`

* data can only be passed from Controller to its corresponding View, not in backward direction -> when redirection occurs (from View to Controller) then the value of the dictionary becomes null => it is only valid in the current request

* it is necessary to do **`typecasting`** for complex and strongly typed data, it always check null values to avoid errors
```
List<Employee> emp = new List<Employee> {...};
ViewData["EmployeeData"] = emp;  
return View(); 

@using DataCtoV.Models  
@model IEnumerable<DataCtoV.Models.Employee> 
@foreach (var employee in (IEnumerable<Employee>)ViewData["EmployeeData"]) {...}
```
### Ways of Casting ViewData
* ViewData has type ViewDataDictionary and this Dictionary (ViewData["EmployeeData"]) has type `"object"`, 
* the foreach loop cannot accept variables having type "object"
* To iterate the data of Employee’s collection properly, we have to cast it to IEnumerable
```
// Ta cũng có thể làm như này:
@{  
    ViewBag.Title = "Employee Data";  
    var employeeData = (IEnumerable<Employee>)ViewData["EmployeeData"];  
} 
@foreach (var employee in employeeData)  {...}
```

### Drawbacks of ViewData
* we have to pass a string as Key. 
* -> If we change it in the Controller then we need to remember to change it in the View otherwise we’ll get a `null exception`
* ViewData is the old and first concept used to pass the data in MVC. 
* But to improve this Microsoft has introduced ViewBag which is of dynamic type

## With ViewBag
* ViewBag is a `dynamic property/object` 
* **ViewBag is a wrapper around the ViewData**
* can pass both strongly and weekly typed data using ViewBag. 
* one of the properties of `"ControllerBase"` class 
* doesn’t require any typecasting for complex and strongly typed data.

* First, make a model class -> populate its properties with some data -> pass that data to ViewBag with the help of a property -> in the View, we can access the data of model class by using ViewBag with the pre-defined property
```
ViewBag.employee = emp;    
return View();  

@model IEnumerable<DataCtoV.Models.Employee>
@foreach (var employee in ViewBag.employee)
```
### ViewData and ViewBag call the same Key’s value
* As ViewBag wraps the ViewData so it stores the data into ViewDataDictionary 
* the Key of ViewData and magic property of ViewBag are the aliases of each other -> Both will print the same output
```
ViewData["exm"] = "Shah";   
ViewBag.exm = "Zain";

<br/>  
@ViewBag.exm  // "Zain"
<br/>  
@ViewData["exm"] // "Zain"
```

# Razor View Engine 
* default view engine for the ASP.NET MVC framework -> to write HTML and server-side code in web pages using C# 
* simplify view development and to improve developer productivity while providing a clean view infrastructure

## View 
```
namespace DataCtoV.Models  
{  
    public class Employee  
    {  
        public byte EmployeeId { get; set; }  
        [Display(Name = "Name")]
        public string EmployeeName { get; set; }  
        public string Address { get; set; }  
        public string Phone { get; set; }  
    }  
}  

@model IEnumerable<DataCtoV.Models.Employee>

<table class="table table-condensed table-hover">  
    <thead>  
        <tr>  
            // @Html.DisplayName to generate the display name for a specified model property (@model IEnumerab...)
            <th>@Html.DisplayNameFor(e => e.EmployeeId)</th>  // Tức là khi ra view nó sẽ hiển thị chữ EmployeeId
            <th>@Html.DisplayNameFor(e => e.EmployeeName)</th>  // nó sẽ hiện thì "Name" vì có Data Annotation
            <th>@Html.DisplayNameFor(e => e.Address)</th>  
            <th>@Html.DisplayNameFor(e => e.Phone)</th>  
        </tr>  
    </thead>  
    <tbody>  
        @foreach (var employee in Model)  
        {  
            <tr>  
                <td>@employee.EmployeeId</td>  
                <td>@employee.EmployeeName</td>  
                <td>@employee.Address</td>  
                <td>@employee.Phone</td>  
            </tr>  
        }  
    </tbody>  
</table>  
```

# Difference between View and Partial View in Asp.net MVC
## layouts
* to provide `consistent webpage sections` and `reduce code repetition`
* Layouts often contain the header, navigation and menu elements, and the footer. 
* The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets. 
* Layouts help you avoid this boilerplate markup in your views.
```
<!DOCTYPE html>
<html>
<head>
    <title>My Web Application</title>
    <link href="styles.css" rel="stylesheet">
    <script src="script.js"></script>
</head>
<body>
    <header>...</header>
    <div class="content">
        @RenderBody() 
    </div>
    <footer>...</footer>
</body>
</html>

// use in individual views:
@layout "_Layout"

<h1>Welcome to My Web Application</h1>
<p>This is the home page.</p>
```

## Views
* is a **`self-contained HTML template`** that represents the presentation logic for a `specific action or page` in an MVC application. 
* associated with a specific **`action method in a controller`** and provides the final output (**`complete HTML page`**) to the client.

## Partial views 
* Represents a **`reusable component or portion of HTML markup`** -> modularizing and reusing UI components
* Can be embedded within other **`views, layouts, another Partial views`** (`doesn't generate a complete HTML` page)
* are `rendered directly` using **`@Html.Partial or @{ Html.RenderPartial(); }`**

* useful to reuse `common UI components` across different pages or modularize your views
```
// _ProfileCard.cshtml (Partial View):
<div class="profile-card">
    <h3>@Model.Name</h3>
    <p>@Model.Email</p>
</div>

// Index.cshtml:
...
<body>
    <h1>Welcome to the Home Page!</h1>
    @Html.Partial("_ProfileCard", new ProfileModel { Name = "John Doe", Email = "john@example.com" })
</body>
...
```

## View components 
* ViewComponents are **`completely self-contained objects`** that consistently render html from a razor view
* **View Component class** - derived from the **`ViewComponent`** base class or implementing the **`IViewComponentResult`** interface
* **View Component view** - `a Razor view file` (.cshtml) for rendering the view which is associated with the view component class `through naming conventions`
* To include the View Component in a view -> use the **`@Component.InvokeAsync("ComponentName") directive`**

* contains logic for `rendering the view component, perform data retrieval, database interaction, other operations required for rendering`
* View components aren't limited to model binding in order to produce webpage output

* Partial Views are simpler, lightweight components for reusing UI markup within views, while View Components allowing for more complex scenarios and better reusability (encapsulate both logic and views)
```
// RecentArticlesViewComponent.cs: (View Component)
public class RecentArticlesViewComponent : ViewComponent
{
    private readonly IArticleRepository _articleRepository;

    public RecentArticlesViewComponent(IArticleRepository articleRepository)
    {
        _articleRepository = articleRepository;
    }

    public async Task<IViewComponentResult> InvokeAsync()
    {
        var articles = await _articleRepository.GetRecentArticlesAsync();
        return View(articles);
    }
}

// Views/ViewComponents/RecentArticles/RecentArticles.cshtml:
<ul>
    @foreach (var article in Model)
    {
        <li>@article.Title</li>
    }
</ul>

// use in View Component Index.cshtml:
...
<h1>Welcome to the Home Page!</h1>
@await Component.InvokeAsync("RecentArticles")
</body>
...
```

## 4. what is ASP.NET Core MVC request life cycle ? 
* **`Routing`**: When a request is received, the `routing system` maps the incoming URL to a specific controller and action method that will handle the request. The `routing configuration` is usually defined in the Startup class.
* **`Controller instantiation`**: Once the routing determines the appropriate controller and action method, an instance of the `corresponding controller` is created. The controller is responsible for processing the incoming request and generating the response.
* **`Model binding`**: The `model binding` process binds the incoming request data (such as form data, query string parameters, or route values) to the parameters of the action method. This allows the action method to access and work with the provided data.
* **`Action method execution`**: The `action method` is executed, performing the necessary business logic and interacting with any required services or data repositories. It may also generate a view or return other types of responses.
* **`View rendering`**: If the action method returns a `view`, the view rendering process takes place. The view combines the model data with the corresponding Razor view template, resulting in an HTML representation of the response.
* **`Response generation`**: The final step is generating the response to be sent back to the client. This includes `serializing` the rendered view (or other response data) into an HTTP response format and setting appropriate headers and status codes

