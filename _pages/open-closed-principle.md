---
layout: single
title: SOLID - Open-Closed Principle
permalink: /solid/open-closed-principle/
---

***Open-Closed Principle (OCP)*** is the **second** amongst the **five** design principles stipulated by **SOLID**.

## Open-Closed Principle
> Objects or entities should be **open** for **extension**, but **closed** for **modification**.

## What it means?
**Open for extension** means that we should be able to add new features or components to the application without breaking existing code.

**Closed for modification** means that we should not introduce breaking changes to existing functionality, because that would force you to refactor a lot of existing code 

## Let's understand better with an example
Consider, that you work for a company which has a portal to sell its products. These products might have various attributes to classify them like colour, size etc. One of the features of this portal is to allow customers to filter products based on these attributes.

We will try and design a simple console application that demonstrates how this can be done. After this we'll first see how **Open-Closed principle** might get violated and ways to overcome this.

We'll create the following Enums to showcase the product attributes.

{% highlight csharp %}
public enum Colour
{
    Red,
    Green,
    Blue
}

public enum Size
{
    Small,
    Medium,
    Large
}
{% endhighlight %}

Let's now create a `Product` class to denotes the Product in our example.

{% highlight csharp %}
public class Product
{
    public string Name { get; set; }
    public Colour Colour { get; set; }
    public Size Size { get; set; }

    public Product(string name, Colour colour, Size size)
    {
        this.Name = name;
        this.Colour = colour;
        this.Size = size;
    }
}
{% endhighlight %}

We'll now come up with a `ProductFilter` class which will help use assign filters based on the Product attributes.

{% highlight csharp %}
public class ProductFilter
{
    public IList<Product> FilterByColour(IList<Product> products, Colour colour)
    {
        return products.Where(p => p.Colour == colour).ToList<Product>();
    }

    public IList<Product> FilterBySize(IList<Product> products, Size size)
    {
        return products.Where(p => p.Size == size).ToList<Product>();
    }
}
{% endhighlight %}

Now that we have the core stuff in place, lets see how we can put the above to use in our `Main` method of our Console application.

{% highlight csharp %}
static void Main(string[] args)
{
    Product bicycle = new Product("Raleigh", Colour.Blue, Size.Medium);
    Product motorbike = new Product("Suzuki", Colour.Red, Size.Medium);
    Product car = new Product("Toyota", Colour.Green, Size.Large);

    IList<Product> products = new List<Product>() { bicycle, motorbike, car };

    var filter = new ProductFilter();

    Console.WriteLine("Blue coloured products:");
    foreach (var product in filter.FilterByColour(products, Colour.Blue))
    {
        Console.WriteLine($"- {product.Name}");
    }
}
{% endhighlight %}

As can be seen we have created a few products and are trying to simulate filtering them by colour (in this case - Blue).
Output is...

{% highlight powershell %}
Blue coloured products:
- Raleigh
Press any key to continue . . .
{% endhighlight %}

Similarly, we could also filter these products based on size using the other filter method `FilterBySize` in our `Main` method.
All good so far!

### Violating the Open-Closed Principle
Now, supposing your boss comes up to you and states that a new filter be available whereby customer can filter by both colour and size. This is not possible with the existing implementation. To make this possible we'll have to update the `ProductFilter` class with a new method `FilterByColourAndSize` as ...
{% highlight csharp %}
public IList<Product> FilterByColourAndSize(IList<Product> products, Colour colour, Size size)
{
    return products.Where(p => p.Colour == colour &&  p.Size == size).ToList<Product>();
}
{% endhighlight %}

We can then get this working in the `Main` method using ...

{% highlight csharp %}
Console.WriteLine("Green coloured Large products:");
foreach (var product in filter.FilterByColourAndSize(products, Colour.Green, Size.Large))
{
    Console.WriteLine($"- {product.Name}");
}
{% endhighlight %}

If you were observant enough, you must have noticed that we **violated** the **Open-Closed Principle** while updating the `ProductFilter` class above.

### Refactoring and aligning to the Open-Closed Principle
To set this right, we can use and **Enterprise** pattern called the **Specification** pattern. We'll define a specification interface with a single method as ...
{% highlight csharp %}
public interface ISpecification<T>
{
    bool IsSatisfied(T t);
}
{% endhighlight %}

The `IsSatisfied` method will be used to actually validate the conditions applicable on the objects properties. We'll also define a filter interface, to apply these specifications to the object in consideration, in this case - `Product` class.
{% highlight csharp %}
public interface IFilter<T>
{
    IList<T> Filter(IList<T> items, ISpecification<T> spec);
}
{% endhighlight %}

Now, using both these interfaces let's attempt to rewrite our earlier approach to apply the colour filter.

{% highlight csharp %}
public class ColourSpecification : ISpecification<Product>
{
    private Colour colour;

    public ColourSpecification(Colour colour)
    {
        this.colour = colour;
    }

    public bool IsSatisfied(Product t)
    {
        return this.colour == t.Colour;
    }
}
{% endhighlight %}

We'll also re-write the `ProductFilter` class and rename it as `ProductFilterImprovised` class with the refactored design as ...
{% highlight csharp %}
public class ProductFilterImprovised : IFilter<Product>
{
    public IList<Product> Filter(IList<Product> items, ISpecification<Product> spec)
    {
        return items.Where<Product>(p => spec.IsSatisfied(p)).ToList<Product>();
    }
}
{% endhighlight %}

We can now apply the colour filter on Products from the `Main` method as ..

{% highlight csharp %}
Console.WriteLine("Green coloured products using improvised filter:");
var improvised_filter = new ProductFilterImprovised();

foreach (var product in improvised_filter.Filter(products, new ColourSpecification(Colour.Green)))
{
    Console.WriteLine($"- {product.Name}");
}
{% endhighlight %}

This will give us the same output as earlier, however, with a much more extensible codebase, whereby, we no longer have to violate the **Open-Closed Principle** to add additional filters.
To prove this, lets try to add a filter to both the colour and size attributes as earlier, but, with the new design.
Let's first create a specification for the size on the same lines as colour.
{% highlight csharp %}
public class SizeSpecification : ISpecification<Product>
{
    private Size size;

    public SizeSpecification(Size size)
    {
        this.size = size;
    }

    public bool IsSatisfied(Product t)
    {
        return this.size == t.Size;
    }
}
{% endhighlight %}

 Now, since we need to use both colour and size to apply the filter, we'll create a `AndSpecification` class which would help combine both size and colour as ...
 {% highlight csharp %}
public class AndSpecification<T> : ISpecification<T>
{
    ISpecification<T> first, second;

    public AndSpecification(ISpecification<T> first, ISpecification<T> second)
    {
        this.first = first;
        this.second = second;
    }

    public bool IsSatisfied(T t)
    {
        return first.IsSatisfied(t) && second.IsSatisfied(t);    
    }
}
 {% endhighlight %}

We can now use this in the `Main` method as ....
{% highlight csharp %}
Console.WriteLine("Green coloured Large products using improvised filter:");

foreach (var product in improvised_filter.Filter(products, new AndSpecification<Product>(new ColourSpecification(Colour.Green), new SizeSpecification(Size.Large))))
{
    Console.WriteLine($"- {product.Name}");
}
{% endhighlight %}

Run the application and you'll get the desired output...

{% highlight csharp %}
Green coloured Large products using improvised filter:
- Toyota
Press any key to continue . . .
{% endhighlight %}

The best part was that we did not have to touch any of the original classes for Specification nor the Filter class to achieve this, thus aligning to the **Open-Closed Principle**.
You may download the code from my github repository [here](https://github.com/sundeepkamath/SOLIDPatterns)

Hope this was useful!!

### **References**  
- [Wikipedia - SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))
- [S.O.L.I.D The first 5 principles of Object Oriented Design with JavaScript](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa)
- [Wikipedia - Open–closed principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)
- [Udemy - Dmitri Nesteruk - Design Patterns in C# and .Net](https://www.udemy.com/design-patterns-csharp-dotnet/learn/v4/overview)