# AspNetCore-Options-Pattern
Sample codes showing how to implement the options pattern in ASP.NET Core

In ASP.NET Core, the option pattern provides the facility to implement strongly typed objects, to retrieve its values from appSettings.json, which can be registered with the built-in dependency injection (IoC) container and make these objects accessible throughout the application.

To learn more about options pattern in ASP.NET Core, visit the following URL:
https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-5.0

# About the sample codes
This is an ASP.NET Core 5.0 MVC project, using dependency injection and options pattern to retrieve a set of values from appSettings.json, and display them.

Here are the entries added to the project's appSettings.json file:
```
    "AccountConfig": {
        "Company": "Amazing Pizza",
        "Username": "John Smith",
        "Region": "California",
        "SalesRep": "Luis Markem"
    }
```

### AccountConfigOptions.cs
```
    public class AccountConfigOptions
    {
        public string Company { get; set; }
        public string Username { get; set; }
        public string Region { get; set; }
        public string SalesRep { get; set; }
    }
```
### Register AccountConfigOptions with ConfigureServices() in the Startup class
```
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<AccountConfigOptions>(Configuration.GetSection("AccountConfig"));

    services.AddControllersWithViews();
}
```

### Inject AccountConfigOptions into HomeController via the constructor
```
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;
    private readonly IOptions<AccountConfigOptions> _options;

    public HomeController(ILogger<HomeController> logger, IOptions<AccountConfigOptions> options)
    {
        _logger = logger;
        _options = options;
    }
. . .

```

### Add "ShowAccountConfig()" action method to HomeController
```
public IActionResult ShowAccountConfig()
{
    return View(_options.Value);
}
```
### Add view, ShowAccountConfig.cshtml, for the ShowAccountConfig() action method
```
@model AspNetCore.Options.Models.AccountConfigOptions

@{
    ViewData["Title"] = "ShowAccountConfig";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h1>ShowAccountConfig</h1>

<div>
    <h4>AccountConfigOptions</h4>
    <hr />
    <dl class="row">
        <dt class = "col-sm-2">
            @Html.DisplayNameFor(model => model.Company)
        </dt>
        <dd class = "col-sm-10">
            @Html.DisplayFor(model => model.Company)
        </dd>
        <dt class = "col-sm-2">
            @Html.DisplayNameFor(model => model.Username)
        </dt>
        <dd class = "col-sm-10">
            @Html.DisplayFor(model => model.Username)
        </dd>
        <dt class = "col-sm-2">
            @Html.DisplayNameFor(model => model.Region)
        </dt>
        <dd class = "col-sm-10">
            @Html.DisplayFor(model => model.Region)
        </dd>
        <dt class = "col-sm-2">
            @Html.DisplayNameFor(model => model.SalesRep)
        </dt>
        <dd class = "col-sm-10">
            @Html.DisplayFor(model => model.SalesRep)
        </dd>
    </dl>
</div>
<div>
    @Html.ActionLink("Edit", "Edit", new { /* id = Model.PrimaryKey */ }) |
    <a asp-action="Index">Back to List</a>
</div>

```



