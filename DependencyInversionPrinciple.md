## Dependency Inversion Principle (DIP)
The principle states that high-level modules should not depend on low-level modules. Instead, they should both depend on abstractions. Additionally, abstractions should not depend on details, but details should depend on abstractions. In simpler terms:
- Classes should depend on interfaces or abstract classes rather than concrete implementations.
- This reduces coupling and makes the system more flexible and testable.

### Why is DIP important?
- Promotes loose coupling between classes.
- Makes it easier to swap implementations without changing the dependent code.
- Improves code maintainability and testability.

### Example:
Scenario:
You’re building a system that saves book details to a storage system (e.g., a database or a file).

### Violating DIP
Here’s a design where the BookManager class directly depends on a concrete implementation of a DatabaseBookSaver class.

```csharp
// Low-level class: Handles database operations
public class DatabaseBookSaver
{
    public void Save(Book book)
    {
        Console.WriteLine($"Saving {book.Title} to the database.");
    }
}

// Book class
public class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
}

// High-level class: BookManager depends directly on DatabaseBookSaver
public class BookManager
{
    private readonly DatabaseBookSaver _bookSaver;

    public BookManager()
    {
        _bookSaver = new DatabaseBookSaver();
    }

    public void SaveBook(Book book)
    {
        _bookSaver.Save(book);
    }
}
```

### Why does this violate DIP?
- The BookManager class depends directly on the DatabaseBookSaver class.
- If you want to save the book to a file or another storage system, you must modify the BookManager class.
- This tight coupling makes the system inflexible and harder to test.

### Refactored Code (Adhering to DIP)
To adhere to DIP:
- Create an abstraction (e.g., an interface) for saving books.
- Both DatabaseBookSaver and FileBookSaver implement the abstraction.
- The BookManager depends on the abstraction, not the concrete implementation.

```csharp
// Abstraction for saving books
public interface IBookSaver
{
    void Save(Book book);
}

// Low-level class: DatabaseBookSaver implements IBookSaver
public class DatabaseBookSaver : IBookSaver
{
    public void Save(Book book)
    {
        Console.WriteLine($"Saving {book.Title} to the database.");
    }
}

// Low-level class: FileBookSaver implements IBookSaver
public class FileBookSaver : IBookSaver
{
    public void Save(Book book)
    {
        Console.WriteLine($"Saving {book.Title} to a file.");
    }
}

// Book class
public class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
}

// High-level class: Depends on IBookSaver abstraction
public class BookManager
{
    private readonly IBookSaver _bookSaver;

    public BookManager(IBookSaver bookSaver)
    {
        _bookSaver = bookSaver;
    }

    public void SaveBook(Book book)
    {
        _bookSaver.Save(book);
    }
}
```

### Usage Example:
```csharp
Book book = new Book { Title = "C# Basics", Author = "John Doe" };

// Save to the database
IBookSaver databaseSaver = new DatabaseBookSaver();
BookManager dbManager = new BookManager(databaseSaver);
dbManager.SaveBook(book);

// Save to a file
IBookSaver fileSaver = new FileBookSaver();
BookManager fileManager = new BookManager(fileSaver);
fileManager.SaveBook(book);
```
