The open-closed principle states that classes, modules, and functions should be open for extension but closed for modification.

The following code violates the open-closed principle.

```csharp
using System;

class Animal
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Type { get; set; }

    public Animal(string name, int age, string type)
    {
        Name = name;
        Age = age;
        Type = type;
    }

    public void GetSpeed()
    {
        switch (Type)
        {
            case "cheetah":
                Console.WriteLine("Cheetah runs up to 130mph");
                break;
            case "lion":
                Console.WriteLine("Lion runs up to 80mph");
                break;
            case "elephant":
                Console.WriteLine("Elephant runs up to 40mph");
                break;
            default:
                throw new ArgumentException($"Unsupported animal type: {Type}");
        }
    }
}

// Example usage
class Program
{
    static void Main(string[] args)
    {
        Animal animal1 = new Animal("Lion", 4, "lion");
        animal1.GetSpeed(); // Lion runs up to 80mph
    }
}

```

Here’s how we can refactor the code to fix the problem:

```csharp
using System;

// Base SpeedRate class
abstract class SpeedRate
{
    public abstract int GetSpeed();
}

// Derived classes for specific animal speeds
class CheetahSpeedRate : SpeedRate
{
    public override int GetSpeed()
    {
        return 130;
    }
}

class LionSpeedRate : SpeedRate
{
    public override int GetSpeed()
    {
        return 80;
    }
}

class ElephantSpeedRate : SpeedRate
{
    public override int GetSpeed()
    {
        return 40;
    }
}

// Animal class
class Animal
{
    public string Name { get; set; }
    public int Age { get; set; }
    public SpeedRate SpeedRate { get; set; }

    public Animal(string name, int age, SpeedRate speedRate)
    {
        Name = name;
        Age = age;
        SpeedRate = speedRate;
    }

    public int GetSpeed()
    {
        return SpeedRate.GetSpeed();
    }
}

// Program class to demonstrate usage
class Program
{
    static void Main(string[] args)
    {
        var cheetah = new Animal("Cheetah", 4, new CheetahSpeedRate());
        Console.WriteLine($"{cheetah.Name} runs up to {cheetah.GetSpeed()} mph");

        var lion = new Animal("Lion", 5, new LionSpeedRate());
        Console.WriteLine($"{lion.Name} runs up to {lion.GetSpeed()} mph");

        var elephant = new Animal("Elephant", 10, new ElephantSpeedRate());
        Console.WriteLine($"{elephant.Name} runs up to {elephant.GetSpeed()} mph");
    }
}
```
