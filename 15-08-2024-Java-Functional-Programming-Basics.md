#### Introduction

Java is notably an object-oriented programming language. However, with the current trend towards functional programming, Java 8 introduces Lambda Expressions, Functional Interfaces and Method References in order to make Java compatible with the simplicity of Functional Programming.

This allows us to write code that provides "single functionality" as is, without the need for a class. In essence, we can mimic functional languages such as Javascript and write functions that run as is or anonymous functions. How Java 8 reconciles this with OOP is by introducting Functional Interfaces.

#### Functional Interfaces

Functional Interfaces are nothing but an interface that contains only one abstract method we call the Single Abstract Method(SAM) and can have any number of default methods. It is recommended to annotate Functional Interfaces with @FunctionalInterface.

Functional interfaces are especially useful when we have a method that takes in a functional interface as a parameter, because we can then use either the Anonymous class, Functional Interface or Method Reference to pass into the parameter to greatly shorten the boilerplate code.

Consumer Interface has a SAM of accept :

```
void accept(T t);
default Consumer andThen(Consumer< ? super T > after);
```

We can then utilise the consumer interface as such e.g the Iterable interface has a forEach method :

```
void forEach(Consumer< ? super T > action)
```

So that instead of using the for loop version of printing a Collection of Strings we can use the forEach :

```
for (String animal: animals) {
	System.out.println(animal);
}

animals.forEach(animal -> {
	System.out.println(animal);
});
```

where,

```
animal -> {
	System.out.println(animal);
}
```

Is the lambda function, that represents an implemented Consumer interface with the animal as the parameter of the SAM and the statements inside the {} as the body of the SAM.

E.g. the lambda function is equivalent to the following anonymous class :

```
Consumer< String > printConsumer = new Consumer< String >() {
    public void accept(String animal) {
        System.out.println(animal);
    };
};
```

The lambda function is also equivalent to the following method reference:

```
System.out::println
```

A method reference can replace a lambda expression when we only have a single method call because sometimes the lambda expression does nothing but call an existing method. We can then just refer to the existing method by name.

Supplier Interface has a SAM of get :

```
T get();
```

Predicate Interface has a SAM of test with a few default and static methods :

```
default Predicate< T > and(Predicate< ? super T > other);
default Predicate< T > or(Predicate< ? super T > other);
static < T > Predicate< T > isEquals(Object targetRef);
default Predicate< T > negate();
```

Function Interface has a SAM of apply with a few default and static methods :

```
R apply(T t);
default < V > Function< T, V > 
andThen(Function after);
default < V > Function< V, R > 
compose(Function< ? super V, ? extends T > before);
static < T > Function< T, T > identity();
```