# ASP.NET_New_Project_Creation
A cheat sheet I created for starting a new full stack ASP.NET MVC from scratch


Here's how I build a new ASP.NET MVC web application! (Cheat sheet for me, hope it helps for you)

In terminal:
```
dotnet new mvc --no-https -o *YOURAPPNAME*
cd *YOURAPPNAME*
code .
```
This will build an MVC boilerplate with the following folder structure:


![image](https://user-images.githubusercontent.com/24249474/115764412-6150c700-a35a-11eb-80a0-31f7d860c423.png)


If you ran ```dotnet run``` now, you would see the template in localhost:5000. 

To use Entity Framework Core, run this in the terminal (assuming dotnet-ef has been installed globally already):
```
dotnet add package Pomelo.EntityFrameworkCore.MySql --version 3.1.1
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.1.5
```

Open Startup.cs and change a few things:
Line 10, add:

```
using Microsoft.EntityFrameworkCore;
using YOURAPPNAME.Models;
```

On line 25, in the ConfigureServices function, you will want to use DB Context, Mvc, and Session (most likely).
```
public void ConfigureServices(IServiceCollection services)
{
      services.AddDbContext<MyContext>(options => options.UseMySql(Configuration["DBInfo:ConnectionString"]));  
      services.AddMvc(options => options.EnableEndpointRouting = false);
      services.AddSession();  
}

```

On Line 33, you will need to change the Configure function to the following: 
```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }
    app.UseStaticFiles();
    app.UseSession();
    app.UseMvc();
}
```

Startup is ready to go. The next step is to create your models. Of course, this will vary based on the project you are building, but here is the template:
```
using System;
using System.ComponentModel.DataAnnotations;

namespace YOURAPPNAME.Models
{
    public class *FIRSTMODEL*
    {
        [Key]
        public int *FIRSTMODEL*Id { get; set; }
        public DateTime CreatedAt { get; set; } = DateTime.Now;
        public DateTime UpdatedAt { get; set; } = DateTime.Now;
    }
}
```

Next, create a MyContext.cs file inside your Models folder. Upon creation, enter this code:
```
using Microsoft.EntityFrameworkCore;
namespace YOURAPPNAME.Models
{
    public class MyContext : DbContext
    {
        public MyContext(DbContextOptions options) : base(options) { }
        public DbSet<FIRSTMODEL> FIRSTMODELS { get; set; }
    }
}   
```

You will then set up your connection string to connect the app to MySQL database. Open appsettings.json and update the AllowedHosts:
```{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
    "DBInfo":
    {
        "Name": "MySQLconnect",
        "ConnectionString": "server=localhost;userid=root;password=YOURMYSQLPASSWORD;port=3306;database=YOURDBNAME;SslMode=None"
    }
}
```

The next step is to set up the Dependency Injection in HomeController, as well as import Microsoft Entity Framework Core. 
Line 9:

```
using Microsoft.EntityFrameworkCore;
```

Replace the code in the HomeController Class on line 13 with this:
```
public class HomeController : Controller
{
    private MyContext _context;

    public HomeController(MyContext context)
    {
        _context = context;
    }

    [HttpGet("")]
    public IActionResult Index()
    {
        return View();
    }

}

```

It wasn't easy, but your application should now work. 

```dotnet watch run```

Open up localhost:5000 and check it out. It should look something like this!

![image](https://user-images.githubusercontent.com/24249474/115765098-2ef39980-a35b-11eb-9c6a-f8d47e0838eb.png)

