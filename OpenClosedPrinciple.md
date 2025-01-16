The Open/Closed Principle (OCP) is the second principle of SOLID, and it states:

A class should be open for extension but closed for modification.

This means that you should be able to add new functionality to a class without modifying its existing code. It helps make your code more maintainable and less error-prone.

Why is OCP important?
- Reduces the risk of introducing bugs when adding new functionality.
- Promotes flexibility and scalability in your codebase.

Example: 
Suppose you have a Book class that calculates the price of a book with a fixed discount. 
The following code violates the open-closed principle.

```csharp
public class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }

    public decimal GetDiscountedPrice(string type)
    {
        if (type == "Regular")
            return Price * 0.9m; // 10% discount for regular books
        else if (type == "Premium")
            return Price * 0.8m; // 20% discount for premium books
        else if (type == "Exclusive")
            return Price * 0.7m; // 30% discount for exclusive books
        else
            return Price;
    }
}

```

Why does this violate OCP?
- Every time a new type of book is introduced, you must modify the GetDiscountedPrice method.
- This makes the class harder to maintain and increases the risk of bugs when changes are made.

Refactored Code (Following OCP)
To follow the Open/Closed Principle, we can use polymorphism to allow extensions without modifying the existing Book class. Here's how:
- Create an abstract base class or interface for the discount logic.
- Implement different discount strategies for each book type.

```csharp
// Define the DiscountStrategy interface
public interface IDiscountStrategy
{
    decimal ApplyDiscount(decimal price);
}

// Concrete strategies for different book types
public class RegularDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(decimal price)
    {
        return price * 0.9m; // 10% discount
    }
}

public class PremiumDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(decimal price)
    {
        return price * 0.8m; // 20% discount
    }
}

public class ExclusiveDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(decimal price)
    {
        return price * 0.7m; // 30% discount
    }
}

// Updated Book class
public class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }
    private readonly IDiscountStrategy _discountStrategy;

    public Book(IDiscountStrategy discountStrategy)
    {
        _discountStrategy = discountStrategy;
    }

    public decimal GetDiscountedPrice()
    {
        return _discountStrategy.ApplyDiscount(Price);
    }
}
```


Usage Example:
```csharp
Book regularBook = new Book(new RegularDiscountStrategy())
{
    Title = "C# Basics",
    Author = "John Doe",
    Price = 100m
};

Book premiumBook = new Book(new PremiumDiscountStrategy())
{
    Title = "Advanced C#",
    Author = "Jane Smith",
    Price = 200m
};

Console.WriteLine($"Regular Book Price: {regularBook.GetDiscountedPrice()}"); // 90
Console.WriteLine($"Premium Book Price: {premiumBook.GetDiscountedPrice()}"); // 160
```

