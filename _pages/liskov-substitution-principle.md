---
layout: single
title: SOLID - Liskov Substitution Principle
permalink: /solid/liskov-substitution-principle/
---

***Liskov Substitution Principle (LSP)*** is the **third** amongst the **five** design principles stipulated by **SOLID**. It was introduced by Barbara Liskov in 1987.

## Liskov Substitution Principle
> Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it. 

## What it means?
It basically means that every **subclass/derived** class should be substitutable by their **base/parent** class.

In other words, a **subclass** should **override** the **parent** class methods in a way that it does not break functionality from a client’s point of view.

## Let's understand better with an example
Consider a `Rectangle` class with basic properties of `Height` and `Width`. 
{% highlight csharp %}
public class Rectangle
{
    public int Width { get; set; }
    public int Height { get; set; }

    public Rectangle()
    {

    }

    public Rectangle(int width, int height)
    {
        this.Width = width;
        this.Height = height;
    }

    public override string ToString()
    {
        return $"Width: {Width}, Height: {Height}";
    }
}
{% endhighlight %}

Now, we can calculate the area of this rectangle in the following manner...
{% highlight csharp %}
class Program
{
    static int Area(Rectangle rectangle) => rectangle.Width * rectangle.Height; 

    static void Main(string[] args)
    {
        Rectangle rectangle = new Rectangle(3, 4);
        Console.WriteLine($"Area of rectangle ({rectangle}) is {Area(rectangle)}");
    }
}
{% endhighlight %}

The output of this program would be ...
{% highlight powershell %}
Area of rectangle (Width: 3, Height: 4) is 12
Press any key to continue . . .
{% endhighlight %}

All good so far. Now supposing we wish to also define a `Square` class. Geometrically, a square is a form of rectangle. Hence, we can define the `Square` class as a subclass of `Rectangle` as ..

{% highlight csharp %}
public class Square : Rectangle
{
    public new int Width
    {
        set
        {
            base.Width = base.Height = value;
        }
    }

    public new int Height
    {
        set
        {
            base.Width = base.Height = value;
        }
    }
}
{% endhighlight %}

The above code is self-explanatory, in the sense that for a square both height and width should be equal.
We can now  update the Main method as ..
{% highlight csharp %}
class Program
{
    static int Area(Rectangle rectangle) => rectangle.Width * rectangle.Height; 
    static void Main(string[] args)
    {
        Rectangle rectangle = new Rectangle(3, 4);
        Console.WriteLine($"Area of rectangle ({rectangle}) is {Area(rectangle)}");

        Square square = new Square();
        square.Width = 4;
        Console.WriteLine($"Area of square ({square}) is {Area(square)}");
    }
}
{% endhighlight %}

The output now comes up as expected as ...
{% highlight powershell %}
Area of rectangle (Width: 3, Height: 4) is 12
Area of square (Width: 4, Height: 4) is 16
Press any key to continue . . .
{% endhighlight %}

### Violating the Liskov Substitution Principle
Now as per **Liskov Substitution Principle**, we should be able to replace the `Square` class reference with `Rectangle` class reference in the `Main` method and still the area of the square should remain unchanged. Let's try this ...
{% highlight csharp %}
Rectangle square = new Square();
square.Width = 4;
Console.WriteLine($"Area of square ({square}) is {Area(square)}");
{% endhighlight %}

The output of this comes out as ...
{% highlight powershell %}
Area of rectangle (Width: 3, Height: 4) is 12
Area of square (Width: 4, Height: 0) is 0
Press any key to continue . . .
{% endhighlight %}

Ideally, the area should have remained unchanged as 16, however it has become 0. Hence, **Liskov Substitution Principle** is ***violated***.

### Refactoring and aligning to the Liskov Substitution Principle
To set this right, we need to override the `Weight` and `Height` properties in the base class with the ones in derived class. Hence, we should replace the `new` keyword in subclass with `override` and mark the respective properties in base class as `virtual`.

The refactored code would now look as ...
{% highlight csharp %}
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public Rectangle()
    {

    }

    public Rectangle(int width, int height)
    {
        this.Width = width;
        this.Height = height;
    }

    public override string ToString()
    {
        return $"Width: {Width}, Height: {Height}";
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set
        {
            base.Width = base.Height = value;
        }
    }

    public override int Height
    {
        set
        {
            base.Width = base.Height = value;
        }
    }
}

class Program
{
    static int Area(Rectangle rectangle) => rectangle.Width * rectangle.Height; 
    static void Main(string[] args)
    {
        Rectangle rectangle = new Rectangle(3, 4);
        Console.WriteLine($"Area of rectangle ({rectangle}) is {Area(rectangle)}");

        Rectangle square = new Square();
        square.Width = 4;
        Console.WriteLine($"Area of square ({square}) is {Area(square)}");
    }
}
{% endhighlight %}

If we now run the above code ...
{% highlight powershell %}
Area of rectangle (Width: 3, Height: 4) is 12
Area of square (Width: 4, Height: 4) is 16
Press any key to continue . . .
{% endhighlight %}

This time the output is as expected and aligned with the **Liskov Substitution Principle**. You may download the code from my github repository [here](https://github.com/sundeepkamath/SOLIDPatterns)

Hope this was useful!

### **References**  
- [Wikipedia - SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))
- [S.O.L.I.D The first 5 principles of Object Oriented Design with JavaScript](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa)
- [Wikipedia - Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
- [Udemy - Dmitri Nesteruk - Design Patterns in C# and .Net](https://www.udemy.com/design-patterns-csharp-dotnet/learn/v4/overview)
- [Robert Martin - Whitepaper SOLID principles](http://web.archive.org/web/20151128004108/http://www.objectmentor.com/resources/articles/lsp.pdf)