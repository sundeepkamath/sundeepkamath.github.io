---
layout: single
title: "REST API Versioning in AspNet Core - Part 2"
date: 2018-04-18 21:32:00 +0530
permalink: /posts/rest-api-versioning-in-aspnet-core-part-2/
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

In the last post we saw the various ways in which we can version REST APIs. As part of this post we'll look at some additional important concepts in maintaining REST based APIs. 
These include ...
1. Deprecating an API version
2. Removing support for an API version
3. Opting out of versioning
4. Version advertisement

Let's understand each of these one at a time.

### 1. Deprecating an API version
There are times when we have to deprecate older APIs over time. Deprecating an API does not mean that it is immediately discontinued, only an indication that it will be unsupported after six months or more. It is however essential that the user is informed of this decision. Let's check how this can be advertised. Let's consider the same `ValuesController` example from part 1 of this article.

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by Query Parameters
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

When we try to access this API,

{% highlight javascript %}
[
    "v1-value1",
    "v1-value2"
]
{% endhighlight %}

Also, the response headers display 

{% highlight javascript %}
Content-Type →application/json; charset=utf-8
Date →Wed, 18 Apr 2018 17:04:04 GMT
Server →Kestrel
Transfer-Encoding →chunked
api-supported-versions →1.0, 2.0
{% endhighlight %}

Now, to deprecate the version 1.0 for example all we need to do is add and set the attribute `Deprecated=true` as ..
{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by Query Parameters
    [ApiVersion("1.0", Deprecated=true)]
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

Now, if we test the API we'll still get the same response, however the response headers will advertise the fact that API v1.0 has been deprecated.

{% highlight javascript %}
[
    "v1-value1",
    "v1-value2"
]
{% endhighlight %}

Also, the response headers display 

{% highlight javascript %}
Content-Type →application/json; charset=utf-8
Date →Wed, 18 Apr 2018 17:07:53 GMT
Server →Kestrel
Transfer-Encoding →chunked
api-deprecated-versions →1.0
api-supported-versions →2.0
{% endhighlight %}

### 2. Removing support for an API version
In the earlier `ValuesController` example, if after the period of deprecation say 6 months is over, we might want to remove support for the specific api version (v1.0 in this case) completely. For this, we can comment out the Route and API version for that controller as ...

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by Query Parameters
    //[ApiVersion("1.0", Deprecated=true)]
    //[Route("api/values")]
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

If the user now tries to access this API with version 1.0, the request will fail with HTTP 404 and response body as follows ...

{% highlight javascript %}
{
    "error": {
        "code": "UnsupportedApiVersion",
        "message": "The HTTP resource that matches the request URI 'http://localhost:5000/api/values?api-version=1.0' does not support the API version '1.0'.",
        "innerError": null
    }
}
{% endhighlight %}

The response headers will show that only version 2.0 is supported as ...

{% highlight javascript %}
Content-Type →application/json; charset=utf-8
Date →Wed, 18 Apr 2018 17:12:58 GMT
Server →Kestrel
Transfer-Encoding →chunked
api-supported-versions →2.0
{% endhighlight %}

### 3. Opting out of versioning
There are times when an API need not be versioned. In such cases the API route can be marked as version neutral. This would mean that if the API is called with any version, it would still continue to give the response and ignore the version. If we were to do this for the `ValuesController` ...

{% highlight javascript %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by Query Parameters
    [ApiVersionNeutral]
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
}
{% endhighlight %}

If you now try calling this API, you'll get the same response inspite of the api-version being passed...

{% highlight javascript %}
[
    "v1-value1",
    "v1-value2"
]
{% endhighlight %}

**Note:**
You cannot have API versions and API neutral option being supported in the same controller.

### 4. Version advertisement
Sometimes API's are split and deployed separately across versions, such as different run-time versions or traffic load balancing. In such cases its difficult to aggregate the implemented API versions across deployments.
In such cases, one can use the `AdvertiseApiVersions` attribute. The advertised and implemented API versions are always aggregated together.

This can be done as ...

{% highlight csharp %}
namespace RESTAPIVersioning.Controllers
{
    //Versioning by Query Parameters
    [ApiVersion("1.0")]
    [AdvertiseApiVersions("2.0")]
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
    [AdvertiseApiVersions("1.0")]
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

Here assumption is that both the API versions have been hosted separately. Now, if we test these we get both the versions advertised in the response headers as ...

{% highlight javascript %}
Content-Type →application/json; charset=utf-8
Date →Wed, 18 Apr 2018 17:47:47 GMT
Server →Kestrel
Transfer-Encoding →chunked
api-supported-versions →1.0, 2.0
{% endhighlight %}

Hope this was useful!

### References
* [Microsoft REST API Versioning Guidelines](https://github.com/Microsoft/aspnet-api-versioning/wiki)



