---
layout: single
title: "REST API Versioning in AspNet Core - Part 1"
date: 2018-04-09 22:28:00 +0530
permalink: /posts/rest-api-versioning-in-aspnet-core/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Blog
tags:
- AspNet Core
- REST 
- Web API
- Versioning
---

These days **REST APIs** are used everywhere. As applications begin to mature, there are numerous times when APIs need to be updated. These need to be done in a systematic manner, ensuring that the existing consumer clients continue to work as usual without breaking the existing functionality. At the same time, it is necessary to update the new APIs and publish them. 

This is where **API Versioning** plays a crucial role. If done properly, it can help avoid chaos and maintain consistency across different API integrations. As part of this article, I'll explain the different versioning approaches that can be used for API versioning in **ASP.Net Core**.

There are 4 out of the box approaches to version APIs...
1. Query string parameter
2. URL path segment
3. HTTP header
4. Media type parameter

Of these, ***query string parameter*** is the **default** approach of versioning. One can also combine API versioning approaches together or define ones own custom method of API versioning.
We'll check each of these one by one. Also, we'll use the default template of web api project that comes with aspnet core to understand each of these.

### Creating a Web API project using ASPNet Core

Assuming you have AspNet Core installed on your machine, we'll create a folder `RESTAPIVersioning` and create a web api project within this using 

{% highlight powershell %}
dotnet new webapi
{% endhighlight %}

Open the `ValuesController.cs` file in this project. We'll demonstrate the **Query Parameter API** versioning approach using this controller. To make this simpler, we'll keep only the `Get` action method in this controller and remove all the others.

![Values controller before changes]({{site.url}}/assets/images/blogs/1ValuesControllerTemplate.jpg)

Now, that this is done, we'll start with the first approach of versioning, using Query string parameters.

### 1. API versioning using Query String Parameter

We'll first install the NuGet package `Microsoft.AspNetCore.Mvc.Versioning` which will help in API versioning.

{% highlight powershell %}
dotnet add .\QueryStringParam.csproj package Microsoft.AspNetCore.Mvc.Versioning
{% endhighlight %}

To enable API versioning, add the following in `ConfigureServices` method in `Startup.cs`.

{% highlight csharp %}
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    //Check https://github.com/Microsoft/aspnet-api-versioning/wiki/API-Versioning-Options for details on these properties
    services.AddApiVersioning(options =>
    {
        options.ReportApiVersions = true;
        options.AssumeDefaultVersionWhenUnspecified = true;
        options.DefaultApiVersion = new ApiVersion(1,0);
    });
}
{% endhighlight %}

Now, ASP.Net Core provides a interface `IApiVersionReader` which defines the behaviour of how an API version is read in its RAW unparsed form from the current HTTP request. For the Query string approach ASPNet Core provides `QueryStringApiVersionReader` implementation out of the box which expects the query string parameter name as `api-version`.
The default case can be configured in `ConfigureServices` method as ....

{% highlight csharp %}
services.AddApiVersioning(options =>
{
    options.ApiVersionReader = new QueryStringApiVersionReader();
});
{% endhighlight %}

If you plan to provide a custom api version parameter, say `v` then,

{% highlight csharp %}
services.AddApiVersioning(options =>
{
    options.ApiVersionReader = new QueryStringApiVersionReader("v");
});
{% endhighlight %}

We can now split the controllers by version numbers to support each api version.

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    [ApiVersion("1.0")]
    [Route("api/values")]
    public class ValuesV1Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v1-value1", "v1-value2" };
        }
    }

    [ApiVersion("2.0")]
    [Route("api/values")]
    public class ValuesV2Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v2-value1", "v2-value2" };
        }
    }
}
{% endhighlight %}

We can now test the APIs using the query parameter `api-version` as ...

Both URLs, `http://localhost:5000/api/values` and `http://localhost:5000/api/values?api-version=1.0` give the response

{% highlight javascript %}
[
    "v1-value1",
    "v1-value2"
]
{% endhighlight %}

This is because if `api-version` is not specified the default version of `1.0` is considered.

While `http://localhost:5000/api/values?api-version=2.0` gives the response

{% highlight javascript %}
[
    "v2-value1",
    "v2-value2"
]
{% endhighlight %}

### 2. API versioning using URL path segment
In this approach, versioning is explicitly specified in the URL path itself. Here, it is not possible to have a default API version for a URL segment.
Lets create a new controller, to demonstrate this, say `ColoursController.cs`.

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by Query Parameters
    [ApiVersion("1.0")]
    [Route("api/v{ver:apiVersion}/colours")]
    public class ColoursV1Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v1-red", "v1-orange" };
        }
    }

    [ApiVersion("2.0")]
    [Route("api/v{ver:apiVersion}/colours")]
    public class ColoursV2Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v2-red", "v2-orange" };
        }
    }
}
{% endhighlight %}

Now, calling the API `http://localhost:5000/api/v1.0/colours` would give the response

{% highlight javascript %}
[
    "v1-red",
    "v1-orange"
]
{% endhighlight %}

while, calling the API `http://localhost:5000/api/v2.0/colours` would give

{% highlight javascript %}
[
    "v2-red",
    "v2-orange"
]
{% endhighlight %}

We could slightly update the above and also support version 3 in the 2nd controller (i.e. version the Action method) as ...

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by URL path segments
    [ApiVersion("1.0")]
    [Route("api/v{ver:apiVersion}/colours")]
    public class ColoursV1Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v1-red", "v1-orange" };
        }
    }

    [ApiVersion("2.0")]
    [ApiVersion("3.0")]
    [Route("api/v{ver:apiVersion}/colours")]
    public class ColoursV2Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v2-red", "v2-orange" };
        }

        [HttpGet, MapToApiVersion("3.0")]
        public IEnumerable<string> GetV3()
        {
            return new string[] { "v3-red", "v3-orange" };
        }
    }
}
{% endhighlight %}

With this, calling API `http://localhost:5000/api/v3.0/colours` would give response...

{% highlight javascript %}
[
    "v3-red",
    "v3-orange"
]
{% endhighlight %}

### 3. API versioning using HTTP Header

In this case, we'll have to provide the api version parameter as part of the HTTP request headers. Lets, understand this better using the CarsController as follows...

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by HTTP headers
    [ApiVersion("1.0")]
    [Route("api/cars")]
    public class CarssV1Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v1-bmw", "v1-mercedes" };
        }
    }

    [ApiVersion("2.0")]
    [Route("api/cars")]
    public class CarsV2Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v2-bmw", "v2-mercedes" };
        }
    }
}
{% endhighlight %}

Now to enable HTTP header api versioning add the following in the `ConfigureServices` method in `Startup.cs`

{% highlight csharp %}
services.AddApiVersioning(options =>
{
    options.ApiVersionReader = new HeaderApiVersionReader( "api-version" );
});
{% endhighlight %}

Unlike, Query parameter approach there is no standard or default HTTP header here. You will have to **explicitly** state this.

Now, calling the API `http://localhost:5000/api/cars` with `api-version = 1.0` in Request HTTP header would give the response

{% highlight javascript %}
[
    "v1-bmw",
    "v1-mercedes"
]
{% endhighlight %}

while, calling the API `http://localhost:5000/api/cars` with `api-version = 2.0` in Request HTTP header would give

{% highlight javascript %}
[
    "v2-bmw",
    "v2-mercedes"
]
{% endhighlight %}

### 4. API versioning using Media type parameter

Here we'll have to pass the versioning parameter along with the media types for content negotiation. Lets create a `MangoesController.cs` file to demonstrate this. 

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by HTTP headers
    [ApiVersion("1.0")]
    [Route("api/mangoes")]
    public class MangoesV1Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v1-alphanso", "v1-kesar" };
        }
    }

    [ApiVersion("2.0")]
    [Route("api/mangoes")]
    public class MangoesV2Controller : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "v2-alphanso", "v2-kesar" };
        }
    }
}
{% endhighlight %}

Now to enable Media Type api versioning add the following in the `ConfigureServices` method in `Startup.cs`

{% highlight csharp %}
services.AddApiVersioning(options =>
{
    options.ApiVersionReader = new MediaTypeApiVersionReader();
});
{% endhighlight %}

Now, calling the API `http://localhost:5000/api/mangoes` without any media type header gives the values..

{% highlight javascript %}
[
    "v1-alphanso",
    "v1-kesar"
]
{% endhighlight %}

while, calling the API `http://localhost:5000/api/cars` with media type set to `accept: text/plain;v=1.0` in Request HTTP header would give the same response.

{% highlight javascript %}
[
    "v1-alphanso",
    "v1-kesar"
]
{% endhighlight %}

Also, calling it with media type set to `accept: text/plain;v=2.0;` would give...

{% highlight javascript %}
[
    "v2-alphanso",
    "v2-kesar"
]
{% endhighlight %}

With this we have covered all the widely used API versioning approaches. 
Hope this was useful!

### References
* [Microsoft REST API Versioning Guidelines](https://github.com/Microsoft/aspnet-api-versioning/wiki)
* [Code Project - Versioning ASP.NET Core 2.0 Web API](https://www.codeproject.com/Articles/1204149/Versioning-ASP-NET-Core-Web-API)

