---
layout: single
title: SOLID - Single Responsibility Principle
permalink: /solid/single-responsibility-principle/
---

***Single Responsibility Principle (SRP)*** is the **first** amongst the **five** design principles stipulated by **SOLID**.

## Single Responsibility Principle
> A class should have one and only **one reason** to **change**, meaning that a class should only have one job.

## Let's understand better with an example
Lets take an example of a journal. We could write code to implement a personal journal as follows...

{% highlight C# %}
using System;
using System.Collections.Generic;

namespace SOLIDPrinciples.SingleResponsibilityPrinciple
{
    public class Journal
    {
        private readonly List<string> entries = new List<string>();

        private static int count =0;

        public int AddEntry(string text)
        {
            System.Console.WriteLine($"{++count}: {text}");
            return count;
        }

        public void RemoveEntry(int index)
        {
            entries.RemoveAt(index);
        }

        public override string ToString()
        {
            return string.Join(Environment.NewLine, entries);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Journal journalEntries = new Journal();
            journalEntries.AddEntry("I had a good time yesterday with family");
            journalEntries.AddEntry("Today was a busy day filled with work");
            System.Console.WriteLine(journalEntries.ToString());
        }
    }
}
{% endhighlight %}

As shown in the code, our class `Journal` has methods to add and remove entries from it. So far our `Journal` class is behaving as required - with a **single responsibility**.
Now, consider the case where we want to extend this by being able to save the journal entries in a file (persist data). We'll add the following 2 methods to the `Journal` class.

{% highlight C# %}
//Extending the class
public void SaveToFile(string fileName)
{
    File.WriteAllText(fileName, ToString());
}

public Journal Load(string fileName)
{
    //Code to read from file, populate the Journal object and return it
}
{% endhighlight %}

Now these 2 methods definitely help adding a persistence layer, however they **violate** the SRP principle. Here in addition to saving and removing entries from the `Journal`, the `Journal` has to also manage persistence. This means that there are more than one reasons for a change to this class which violates the SRP principle (as per which there should be only one reason for change in a class).

Hence, to adhere to SRP we can tackle the above in a different way.
We will create a new class `Persistence` as follows...

{% highlight C# %}
public class Persistence
{
    public void SaveToFile(string fileName, Journal journal, bool overwrite = false)
    {
        if(overwrite || !string.IsNullOrEmpty(fileName))
            File.WriteAllText(fileName, journal.ToString());
    }
}
{% endhighlight %}

This class can now be instantiated and called from the main method as follows...

{% highlight C# %}
class Program
{
    static void Main(string[] args)
    {
        Journal journalEntries = new Journal();
        journalEntries.AddEntry("I had a good time yesterday with family");
        journalEntries.AddEntry("Today was a busy day filled with work");
        System.Console.WriteLine(journalEntries.ToString());

        string fileName = "c:\file.txt";
        Persistence persist = new Persistence();
        persist.SaveToFile(fileName, journalEntries, true);

    }
}
{% endhighlight %}

As is now evident, henceforth if any changes are required in the persistence layer it will not require the `Journal` class to be modified and hence SRP will not be violated.

### **References**  
- [Wikipedia - SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))
- [Udemy - Dmitri Nesteruk - Design Patterns in C# and .Net](https://www.udemy.com/design-patterns-csharp-dotnet/learn/v4/overview)
