## Liskov Substitution Principle (LSP)
The Liskov Substitution Principle (LSP) is the third principle of SOLID, and it states:

Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

In simpler terms, a subclass should be able to stand in for its parent class without breaking the behavior of the program.

### Why is LSP important?
- It ensures the substitutability of derived classes.
- It promotes code reusability and robustness by avoiding unexpected behaviors when using subclasses.

### Example: 
Scenario:
You have a Book class, and you want to create a specialized subclass called EBook. Let’s see how violating or adhering to LSP affects the design.

### Violating LSP
Here’s a base Book class and a derived EBook class that violates LSP:

```csharp
public class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }

    public virtual void Print()
    {
        Console.WriteLine($"Printing the book: {Title}");
    }
}

public class EBook : Book
{
    public override void Print()
    {
        throw new NotImplementedException("EBooks cannot be printed.");
    }
}
```
### Why does this violate LSP?
- The EBook class overrides the Print method but throws an exception.
- If a method works with a Book object and expects to print it, substituting it with an EBook will break the program.

For example:
```csharp
public void PrintBookDetails(Book book)
{
    book.Print(); // Will throw an exception if 'book' is an 'EBook'
}

// Usage
Book ebook = new EBook();
PrintBookDetails(ebook); // Throws NotImplementedException
```

### Fixing the Violation (Adhering to LSP)
To follow LSP, ensure that the EBook class behaves in a way that is consistent with the expectations of the Book class. A better design is to separate the responsibilities of physical books and e-books using proper inheritance.

```csharp
// Base class
public abstract class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }

    public abstract void DisplayDetails();
}

// PhysicalBook class
public class PhysicalBook : Book
{
    public override void DisplayDetails()
    {
        Console.WriteLine($"Physical Book: {Title} by {Author}");
    }

    public void Print()
    {
        Console.WriteLine($"Printing the book: {Title}");
    }
}

// EBook class
public class EBook : Book
{
    public string DownloadLink { get; set; }

    public override void DisplayDetails()
    {
        Console.WriteLine($"E-Book: {Title} by {Author}");
    }

    public void Download()
    {
        Console.WriteLine($"Downloading the e-book from: {DownloadLink}");
    }
}
```
