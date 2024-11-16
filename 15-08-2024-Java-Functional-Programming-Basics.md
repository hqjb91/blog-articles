# Introduction to Functional Programming in Java 8

Java is renowned as an object-oriented programming (OOP) language. However, to adapt to the growing trend of functional programming, Java 8 introduced **Lambda Expressions**, **Functional Interfaces**, and **Method References**. These features enable Java developers to write concise, functional-style code while maintaining compatibility with OOP principles.

With these features, developers can write "single functionality" code, akin to functional languages like JavaScript. Java reconciles this functional approach with its OOP paradigm through the concept of **Functional Interfaces**.

---

## Functional Interfaces

A **Functional Interface** is an interface that contains only one abstract method, known as the **Single Abstract Method (SAM)**. These interfaces may also include any number of `default` or `static` methods. Functional Interfaces are annotated with `@FunctionalInterface` to ensure adherence to this single-method contract.

Functional Interfaces are particularly useful for reducing boilerplate code. They allow us to pass functionality to methods using **Lambda Expressions**, **Anonymous Classes**, or **Method References**.

---

### Example: The Consumer Interface

The `Consumer` interface is a functional interface that defines the following methods:

```java
void accept(T t);
default Consumer<T> andThen(Consumer<? super T> after);
```

#### Traditional For-Loop Example:

Instead of using a traditional `for` loop to print a collection of strings:

```java
for (String animal : animals) {
    System.out.println(animal);
}
```

We can use the `Consumer` interface with the `forEach` method:

```java
animals.forEach(animal -> {
    System.out.println(animal);
});
```

#### Lambda Expression Breakdown:

The lambda expression in the above example:

```java
animal -> {
    System.out.println(animal);
}
```

- **`animal`** is the parameter of the SAM.
- The statements inside `{}` are the body of the SAM.

This is equivalent to the following **Anonymous Class** implementation:

```java
Consumer<String> printConsumer = new Consumer<String>() {
    public void accept(String animal) {
        System.out.println(animal);
    }
};
```

It is also equivalent to the following **Method Reference**:

```java
animals.forEach(System.out::println);
```

Method references simplify lambda expressions that merely invoke an existing method.

---

## Key Functional Interfaces

### 1. **Supplier Interface**
The `Supplier` interface is used to supply objects without any input. It has the following SAM:

```java
T get();
```

Example:

```java
Supplier<String> stringSupplier = () -> "Hello, World!";
System.out.println(stringSupplier.get());
```

---

### 2. **Predicate Interface**
The `Predicate` interface is used for conditional checks. It defines the following SAM and additional methods:

```java
boolean test(T t);
default Predicate<T> and(Predicate<? super T> other);
default Predicate<T> or(Predicate<? super T> other);
default Predicate<T> negate();
static <T> Predicate<T> isEqual(Object targetRef);
```

Example:

```java
Predicate<Integer> isEven = number -> number % 2 == 0;
System.out.println(isEven.test(4)); // Output: true
```

---

### 3. **Function Interface**
The `Function` interface is used for transformations. It defines the following SAM and additional methods:

```java
R apply(T t);
default <V> Function<T, V> andThen(Function<? super R, ? extends V> after);
default <V> Function<V, R> compose(Function<? super V, ? extends T> before);
static <T> Function<T, T> identity();
```

Example:

```java
Function<Integer, String> intToString = num -> "Number: " + num;
System.out.println(intToString.apply(5)); // Output: Number: 5
```
