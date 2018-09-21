---
layout: single
title: SOLID - Interface Segregation Principle
permalink: /solid/interface-segregation-principle/
---

***Interface Segregation Principle (ISP)*** is the **fourth** amongst the **five** design principles stipulated by **SOLID**.

## Interface Segregation Principle
> Clients should not be forced to depend on methods they don't use. 

## What it means?
**Interfaces** should be designed such that clients only have to know about **methods** that are of interest to them.

## Let's understand better with an example
Let's suppose that we have a document and we wish to carry out various activities with it, like print, scan and fax it. While programming for this, we can create an interface `IMachine` which will expose these functionalities on the document represented by `Document` class as ...
{% highlight csharp %}
public class Document
{

}

public interface IMachine
{
    void Print(Document d);
    void Scan(Document d);
    void Fax(Document d);
}
{% endhighlight %}

Now, if we wish to have a multi-function printer that can perform all of the above, we can define the same as ...
{% highlight csharp %}
public class MultiFunctionPrinter : IMachine
{
    public void Print(Document d)
    {
        //custom implementation to print document
    }

    public void Scan(Document d)
    {
        //custom implementation to scan document
    }

    public void Fax(Document d)
    {
        //custom implementation to fax document
    }
}
{% endhighlight %}

All good so far. Now lets look at how the **ISP principle** might get violated.

### Violating the Interface Segregation Principle
Now, suppose we wish to have an old fashioned printer which only needs to perform one job of printing documents. If in this case all we have is the `IMachine` interface that we defined above, then ....

{% highlight csharp %}
public class OldFashionedPrinter : IMachine
{
    public void Print(Document d)
    {
        //custom implementation to print document
    }

    public void Scan(Document d)
    {
        throw new NotImplementedException();
    }

    public void Fax(Document d)
    {
        throw new NotImplementedException();
    }
}
{% endhighlight %}

Here, as can be seen we end up exposing all the 3 functionalities, when in fact we would be implementing only the printing functionality. This also means that we will have to document the fact that the remaining two methods are not implemented and should not be used.

### Refactoring and aligning to the Interface Segregation Principle
To set this right, we can use the **Interface Segregation Principle** and as part of it divide the above interface into smaller interfaces whereby each interface handles a single concern.

{% highlight csharp %}
public interface IPrinter
{
    void Print(Document d);
}

public interface IScanner
{
    void Scan(Document d);
}

public interface IFax
{
    void Fax(Document d);
}
{% endhighlight %}

Now, if we wish to implement our old fashioned printer we can do so by just implementing the `IPrinter` interface above.
{% highlight csharp %}
public class OldFashionedPrinter : IPrinter
{
    public void Print(Document d)
    {
        //provide custom implementation
    }
}
{% endhighlight %}
As can be seen, the printer has just as much implementation to get the document printed.

If we wish to have a multi-function printer, we can still have a high level interface which can inherit from smaller interfaces as ...
{% highlight csharp %}
public interface IMultiFunctionPrinter : IPrinter, IScanner, IFax
{

}

public class MultiFunctionPrinter : IMultiFunctionPrinter
{
    public void Print(Document d)
    {
        //print documents
    }

    public void Scan(Document d)
    {
        //scan documents
    }

    public void Fax(Document d)
    {
        //fax documents
    }
}
{% endhighlight %}

There could also be another scenario, whereby you already have custom implementation for printer, scanner and fax in your codebase. In this case you could use the same in the above class as ...

{% highlight csharp %}
public class MultiFunctionPrinter : IMultiFunctionPrinter
{
    private IPrinter printer;
    private IScanner scanner;
    private IFax fax;

    public MultiFunctionPrinter(IPrinter printer, IScanner scanner, IFax fax)
    {
        this.printer = printer;
        this.scanner = scanner;
        this.fax = fax;
    }

    public void Print(Document d)
    {
        printer.Print(d);
    }

    public void Fax(Document d)
    {
        fax.Fax(d);
    }

    public void Scan(Document d)
    {
        scanner.Scan(d);
    }
}
{% endhighlight %}

Here, we are implementing the interface, however delegating to the existing implementations of each capability. This is all there is to the **Interface Segregation Principle (ISP)**.

Hope this was useful!

### **References**  
- [Wikipedia - SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))
- [S.O.L.I.D The first 5 principles of Object Oriented Design with JavaScript](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa)
- [Wikipedia - Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle)
- [Udemy - Dmitri Nesteruk - Design Patterns in C# and .Net](https://www.udemy.com/design-patterns-csharp-dotnet/learn/v4/overview)
