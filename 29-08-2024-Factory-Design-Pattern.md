# Factory Design Patterns

## Introduction

The Factory family of patterns is central to designing modular, scalable, and loosely coupled systems. Below is a breakdown of the **Factory Pattern**, **Factory Method Pattern**, and **Abstract Factory Pattern**, illustrated with examples.

---

The Factory Design Pattern is a Creational design pattern to decouple the code that creates the objects from the object itself.

### Without using the Factory Pattern

E.g. This restaurant class below relies on the object's constructor to create and each new object will require a change to the restaurant class violating the open-close principle :

```
public class Program 
{ 
	public static void Main() 
	{ 
		// Manually creating different types of cakes 
		Cake chocolateCake = new ChocolateCake(); 
		chocolateCake.Prepare(); 
		chocolateCake.Bake(); 
		chocolateCake.Decorate(); 
		
		Cake vanillaCake = new VanillaCake(); 
		vanillaCake.Prepare(); 
		vanillaCake.Bake(); 
		vanillaCake.Decorate(); 
		
		Cake cheeseCake = new CheeseCake(); 
		cheeseCake.Prepare(); 
		cheeseCake.Bake(); 
		cheeseCake.Decorate(); 
	} 
}
```

This exposes the instantiation logic to the client as the client code is tightly coupled to the concrete classes VanillaCake, ChocolateCake and CheeseCake. If the client news to introduce a new cake e.g. TiramisuCake, the client code needs to be modified (violates Open-Closed principle for the client class). We can use the Factory Pattern to resolve this :

### Using the Factory Pattern

```
public class CakeFactory 
{ 
	public static Cake CreateCake(string cakeType) 
	{ 
		switch (cakeType.ToLower()) 
		{ 
			case "chocolate": return new ChocolateCake(); 
			case "vanilla": return new VanillaCake(); 
			case "cheesecake": return new Cheesecake(); 
			default: throw new ArgumentException("Invalid cake type."); 
		} 
	} 
}

public class Program 
{ 
	public static void Main() 
	{ // Using the factory to create different types of cakes 
		Cake chocolateCake = CakeFactory.CreateCake("chocolate");
		chocolateCake.Prepare(); 
		chocolateCake.Bake(); 
		chocolateCake.Decorate(); 
		
		Cake vanillaCake = CakeFactory.CreateCake("vanilla");
		vanillaCake.Prepare(); 
		vanillaCake.Bake(); 
		vanillaCake.Decorate(); 
		
		Cake cheesecake = CakeFactory.CreateCake("cheesecake");
		cheesecake.Prepare(); 
		cheesecake.Bake(); 
		cheesecake.Decorate(); 
	} 
}
```

 The client depends on the `CakeFactory` class, which declares a `CreateCake()` method. The client can create cakes without knowing their concrete classes, promoting loose coupling between the objects and the client class.

However, this still violates the Open-Close principle (Open for extension but Closed for Modification) as every time you add a new Cake you would have to modify the CakeFactory class (which should be Closed for Modification). To overcome this we can use the Factory Method Pattern :

### Factory Method Pattern

#### Implementation

1. **Abstract Base Factory Class**: We'll create an abstract `CakeFactory` class with an abstract `CreateCake` method.
2. **Concrete Factory Classes**: Each specific cake type will have its own factory class (`ChocolateCakeFactory`, `VanillaCakeFactory`, etc.) that overrides the `CreateCake` method to create the appropriate cake object.

```
public abstract class CakeFactory
{
    public abstract Cake CreateCake();

    public void MakeCake()
    {
        Cake cake = CreateCake(); // Factory Method
        cake.Prepare();
        cake.Bake();
        cake.Decorate();
    }
}

public class ChocolateCakeFactory : CakeFactory
{
    public override Cake CreateCake()
    {
        return new ChocolateCake();
    }
}

public class VanillaCakeFactory : CakeFactory
{
    public override Cake CreateCake()
    {
        return new VanillaCake();
    }
}

public class CheesecakeFactory : CakeFactory
{
    public override Cake CreateCake()
    {
        return new Cheesecake();
    }
}

public class Program
{
    public static void Main()
    {
        // Using the factory method pattern to create cakes
        CakeFactory chocolateCakeFactory = new ChocolateCakeFactory();
        chocolateCakeFactory.MakeCake();

        CakeFactory vanillaCakeFactory = new VanillaCakeFactory();
        vanillaCakeFactory.MakeCake();

        CakeFactory cheesecakeFactory = new CheesecakeFactory();
        cheesecakeFactory.MakeCake();
    }
}
```

This adheres to the Open-Closed principle as you can easily introduce new types of cakes by adding new factory subclasses without modifying existing factory.

However, imagine if we were to want to introduce another family of object (e.g. Steaks) and we want the client to be independent of newly introduced families of object. We should then use Abstract Factory Pattern which can be thought of as a super factory which creates other factories :

### Abstract Factory Pattern

```
// Abstract Factory Class
public abstract class AbstractFactory
{
    public abstract Cake CreateCake(string cakeType);
    public abstract Steak CreateSteak(string steakType);
}

public class CakeFactory : AbstractFactory
{
    public override Cake CreateCake(string cakeType)
    {
		switch (cakeType.ToLower()) 
		{ 
			case "chocolate": return new ChocolateCake(); 
			case "vanilla": return new VanillaCake(); 
			case "cheesecake": return new Cheesecake(); 
			default: throw new ArgumentException("Invalid cake type."); 
		} 
    }

    public override Steak CreateSteak(string steakType)
    {
        return null;
    }
}

public class SteakhouseFactory : AbstractFactory
{
    public override Cake CreateCake(string cakeType)
    {
        return null;
    }

    public override Steak CreateSteak(string steakType)
    {
    	switch (steakType.ToLower()) 
		{ 
			case "ribeye": return new RibeyeSteak(); 
			case "sirloin": return new SirloinSteak(); 
			default: throw new ArgumentException("Invalid steak type.");
		} 
    }
}

public class FactoryProducer
{
	public static AbstractFactory CreateFactory(string factoryType)
	{
	    switch (steakType.ToLower()) 
		{ 
			case "cake": return new CakeFactory(); 
			case "steak": return new SteakhouseFactory(); 
			default: throw new ArgumentException("Invalid factory type.");
		} 
	}
}

public class Program
{
    public static void Main()
    {
        // Create a Cake Factory
        CakeFactory cakeFactory = FactoryProducer.CreateFactory("cake");
        VanillaCake vanillaCake = cakeFactory.CreateCake("vanilla");
        vanillaCake.Prepare();
        vanillaCake.Bake();
        vanillaCake.Decorate();

        // Create a Steakhouse Factory
        SteakhouseFactory steakhouse = new FactoryProducer.CreateFactory("steak");
        SirloinSteak sirloinSteak = steakhouse.CreateSteak("sirloin");
        sirloinSteak.Season();
        sirloinSteak.Cook();
        sirloinSteak.Serve();
    }
}
```
