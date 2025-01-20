## Interface Segregation Principle (ISP)
The interface segregation principle states that clients should not be forced to implement interfaces or methods they do not use.This principle is fairly similar to the single responsibility principle (SRP). But it’s not just about a single interface doing only one thing – it’s about breaking the whole codebase into multiple interfaces or components.

### Why is ISP important?
- Prevents implementing unnecessary methods that a class doesn’t need.
- Promotes separation of concerns by creating smaller, focused interfaces.
- Makes the codebase easier to maintain and understand.

### Example:
Scenario:
You’re building a system with different types of books: PhysicalBook and EBook. Both share some common functionality, but each has unique features.

Here’s an interface that violates ISP:

```csharp
public interface IBook
{
    string Title { get; set; }
    string Author { get; set; }
    decimal Price { get; set; }

    void Print();
    void Download();
}
```
### Why is this bad?
- The Print method is not relevant to EBook.
- The Download method is not relevant to PhysicalBook.
- Classes are forced to implement methods they don’t need.

```csharp
// PhysicalBook is forced to implement Download()
public class PhysicalBook : IBook
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }

    public void Print()
    {
        Console.WriteLine($"Printing physical book: {Title}");
    }

    public void Download()
    {
        throw new NotImplementedException("Physical books cannot be downloaded.");
    }
}

// EBook is forced to implement Print()
public class EBook : IBook
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }

    public void Print()
    {
        throw new NotImplementedException("E-books cannot be printed.");
    }

    public void Download()
    {
        Console.WriteLine($"Downloading e-book: {Title}");
    }
}
```
- Violating ISP leads to meaningless methods (NotImplementedException), confusing developers and increasing the chance of errors.

### Refactored Code (Adhering to ISP)
To follow ISP, split the IBook interface into smaller, more specific interfaces.

```csharp
// General interface for all books
public interface IBook
{
    string Title { get; set; }
    string Author { get; set; }
    decimal Price { get; set; }
}

// Interface for printable books
public interface IPrintable
{
    void Print();
}

// Interface for downloadable books
public interface IDownloadable
{
    void Download();
}
```
### Updated Classes:

```csharp
// PhysicalBook implements IBook and IPrintable
public class PhysicalBook : IBook, IPrintable
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }

    public void Print()
    {
        Console.WriteLine($"Printing physical book: {Title}");
    }
}

// EBook implements IBook and IDownloadable
public class EBook : IBook, IDownloadable
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }

    public void Download()
    {
        Console.WriteLine($"Downloading e-book: {Title}");
    }
}
```
### Usage Example:
```csharp
// Print logic only applies to printable books
public void PrintBook(IPrintable printableBook)
{
    printableBook.Print();
}

// Download logic only applies to downloadable books
public void DownloadBook(IDownloadable downloadableBook)
{
    downloadableBook.Download();
}

// Usage
PhysicalBook physicalBook = new PhysicalBook { Title = "C# Basics", Author = "John Doe", Price = 100m };
EBook ebook = new EBook { Title = "Advanced C#", Author = "Jane Smith", Price = 50m };

PrintBook(physicalBook); // Works
DownloadBook(ebook);     // Works
```
