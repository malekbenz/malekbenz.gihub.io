---
layout: post
title: "First Asp.Net Core MVC application"
date: 2016-08-10
author: Malekbenz
comments: true
category: Asp.Net
tags : ['Asp.Net', '.Net']
categories: ['Asp.Net', '.Net']
---
## ASP.NET Core MVC  


This post will guide you to Build a your First MVC application with ASP.NET Core. I also wanted to do this completely on Linux.

In order to install .NET Core on Ubuntu or Linux Mint you can see [Install .Net Core on linux](/blog/2016/08/01/Install-dotnet-core-linux).

If you don't kown how to create a simple web server you can see [Create a First web application with .Net Core ](/blog/2016/08/05/First-web-application-dotnet-core-linux).

## Create .Net Core project

Create a `mvcapp` directory to hold your application.

```javascript
    $ mkdir  mvcapp
    $ cd mvcapp

```

## Add the Kestrel & MVC packages

Update the project.json file to add the Kestrel HTTP server & MVX packages as a dependency:

```csharp
    {
    "version": "1.0.0-*",
    "buildOptions": {
        "debugType": "portable",
        "emitEntryPoint": true
    },
    "dependencies": {},
    "frameworks": {
        "netcoreapp1.0": {
        "dependencies": {
            "Microsoft.NETCore.App": {
            "type": "platform",
            "version": "1.0.0"
            },
            "Microsoft.AspNetCore.Server.Kestrel": "1.0.0",
            "Microsoft.AspNetCore.Mvc" : "1.0.0"
        },
        "imports": "dnxcore50"
        }
    }
    }
```

![CMD](/images/aspnet/project.json.png){:class="img-responsive" :max-width="80%"}


and run `dotnet restore`

```
    $ dotnet restore
```

## Update  Program.cs:

Update the code in Program.cs to setup and start the Web host:

```csharp
using System;
using Microsoft.AspNetCore.Hosting;

namespace mvcapp
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseKestrel()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}
```
![CMD](/images/aspnet/Program.cs.png){:class="img-responsive" :max-width="80%"}


## Create a Conroller:

In order to create a controller, you must create a `Controllers` folder.

Under the `Controllers` folder create `HomeController` file:  

```csharp
using Microsoft.AspNetCore.Mvc;

namespace mvcapp
{
    public class HomeController : Controller
    {
        public string Index()
        {
            return "Helloo From my MVC APP";
        }
    }
}
```

![CMD](/images/aspnet/Startup.Mvc.png){:class="img-responsive" :max-width="80%"}

run the app: 

```
    $ dotnet run
```

![CMD](/images/aspnet/404.error.cs.png){:class="img-responsive" :max-width="80%"}

And It doesn't work! we got 404 error, Because the app doesn't know how to route a request.   

## Routing 

Routing is used to map requests to route handlers. Routes are configured when the application starts.

ASP.NET Core MVC is built on top of ASP.NET Core’s routing, a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs. This enables you to define your application’s URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized. You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.


```
    routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");

```
![CMD](/images/aspnet/Startup.routes.png){:class="img-responsive" :max-width="80%"}

run the app again: 

```
    $ dotnet run
```

![CMD](/images/aspnet/run.with.routes.png){:class="img-responsive" :max-width="80%"}

```csharp
    app.UseFileServer(enableDirectoryBrowsing: true);
```


>
> ## Serve a static content with .NET Core
>