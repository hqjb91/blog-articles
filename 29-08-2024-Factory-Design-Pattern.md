# Factory Design Patterns

## Introduction

The Factory family of patterns is central to designing modular, scalable, and loosely coupled systems. Below is a breakdown of the **Factory Pattern**, **Factory Method Pattern**, and **Abstract Factory Pattern**, illustrated with examples.

---

## Factory Design Pattern

### Concept:
- The Factory Pattern decouples object creation from the client. 
- Instead of creating objects directly using constructors, the client uses a factory method.

### Problem without Factory:
Direct instantiation ties the client to concrete classes, violating the **Open-Closed Principle** (OCP). Adding new object types requires modifying existing client code.

#### Example Without Factory:
```csharp
Cake chocolateCake = new ChocolateCake(); // Tightly coupled
```

#### With Factory Pattern:
The factory centralizes the creation logic.

```csharp
public class CakeFactory
{
    public static Cake CreateCake(string cakeType)
    {
        return cakeType.ToLower() switch
        {
            "chocolate" => new ChocolateCake(),
            "vanilla" => new VanillaCake(),
            "cheesecake" => new Cheesecake(),
            _ => throw new ArgumentException("Invalid cake type.")
        };
    }
}
```

#### Advantages:
- Centralized instantiation logic.
- Easier to modify instantiation without changing the client.

---

## Factory Method Pattern

### Concept:
- A **creational design pattern** that provides an interface for creating objects but lets subclasses alter the type of objects that will be created.
- Solves the **Open-Closed Principle** issue of the Factory Pattern.

### Problem with Basic Factory:
Adding new object types still requires modifying the factory (e.g., `CakeFactory`). This violates OCP for the factory class itself.

### Implementation:

#### Abstract Factory Class:
Defines the factory method, allowing concrete factories to provide their own implementations.
```csharp
public abstract class CakeFactory
{
    public abstract Cake CreateCake();
}
```

#### Concrete Factories:
Each concrete factory creates a specific type of cake.
```csharp
public class ChocolateCakeFactory : CakeFactory
{
    public override Cake CreateCake() => new ChocolateCake();
}

public class VanillaCakeFactory : CakeFactory
{
    public override Cake CreateCake() => new VanillaCake();
}
```

#### Usage:
```csharp
CakeFactory factory = new ChocolateCakeFactory();
Cake chocolateCake = factory.CreateCake();
chocolateCake.Prepare();
```

### Advantages:
- Adding new types of cakes involves only creating a new factory class.
- Existing factories remain unchanged (adheres to OCP).

---

## Abstract Factory Pattern

### Concept:
- An Abstract Factory is a super-factory that creates other factories.
- It is useful when dealing with multiple families of related objects.

### Problem with Factory Method:
What if there are multiple families of products (e.g., Cakes and Steaks)? The client would still need to know about different factories, increasing complexity.

### Solution:
Use an abstract factory to produce related product families.

#### Abstract Factory Class:
```csharp
public abstract class AbstractFactory
{
    public abstract Cake CreateCake(string cakeType);
    public abstract Steak CreateSteak(string steakType);
}
```

#### Concrete Factories:
Factories for specific product families (Cakes and Steaks).
```csharp
public class CakeFactory : AbstractFactory
{
    public override Cake CreateCake(string cakeType)
    {
        return cakeType.ToLower() switch
        {
            "chocolate" => new ChocolateCake(),
            "vanilla" => new VanillaCake(),
            "cheesecake" => new Cheesecake(),
            _ => throw new ArgumentException("Invalid cake type.")
        };
    }

    public override Steak CreateSteak(string steakType) => null;
}

public class SteakhouseFactory : AbstractFactory
{
    public override Cake CreateCake(string cakeType) => null;

    public override Steak CreateSteak(string steakType)
    {
        return steakType.ToLower() switch
        {
            "ribeye" => new RibeyeSteak(),
            "sirloin" => new SirloinSteak(),
            _ => throw new ArgumentException("Invalid steak type.")
        };
    }
}
```

#### Factory Producer:
Centralized factory selector.
```csharp
public class FactoryProducer
{
    public static AbstractFactory CreateFactory(string factoryType)
    {
        return factoryType.ToLower() switch
        {
            "cake" => new CakeFactory(),
            "steak" => new SteakhouseFactory(),
            _ => throw new ArgumentException("Invalid factory type.")
        };
    }
}
```

#### Usage:
```csharp
AbstractFactory cakeFactory = FactoryProducer.CreateFactory("cake");
Cake vanillaCake = cakeFactory.CreateCake("vanilla");

AbstractFactory steakFactory = FactoryProducer.CreateFactory("steak");
Steak sirloinSteak = steakFactory.CreateSteak("sirloin");
```

### Advantages:
- Encapsulates the creation of related product families.
- Client is decoupled from the specific factories and product implementations.