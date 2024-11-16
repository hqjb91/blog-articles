# Singleton Design Pattern

## Introduction

A **Singleton** is a design pattern that ensures a class has only one instance throughout the application's lifecycle. The singleton instance is globally accessible, making it useful for managing shared resources like configuration settings, logging, or thread pools.

To achieve this, a Singleton class must:
1. **Restrict instance creation** by making the constructor private.
2. **Store the unique instance** as a private field accessible only through a public getter method.
3. Initialize the unique instance either:
   - **Eagerly** (when the class is loaded), or
   - **Lazily** (when first accessed).
4. Ensure the **getter method and instance field are static** to avoid using a constructor.

---

## Naive Singleton Implementations

### Eagerly Created Singleton

An **eager Singleton** initializes the instance as soon as the class is loaded:

```csharp
public class Singleton {
    private static Singleton _instance = new Singleton();
    private string _value;

    private Singleton() { }

    public static Singleton GetInstance() {
        return _instance;
    }

    public string GetValue() {
        return _value;
    }

    public void SetValue(string value) {
        _value = value;
    }
}
```

### Benefits
- Simple to implement.
- Thread-safe by design since the instance is created during class loading.

### Drawbacks
- Resource-intensive if initialization is costly and the instance is not always used.

---

### Lazily Created Singleton

A **lazy Singleton** initializes the instance only when requested:

```csharp
public class Singleton {
    private static Singleton _instance;
    private string _value;

    private Singleton() { }

    public static Singleton GetInstance() {
        if (_instance == null) {
            _instance = new Singleton();
        }
        return _instance;
    }

    public string GetValue() {
        return _value;
    }

    public void SetValue(string value) {
        _value = value;
    }
}
```

### Benefits
- Avoids unnecessary resource allocation unless the instance is actually used.

### Drawbacks
- **Not thread-safe** in a multi-threaded environment. If two threads call `GetInstance()` simultaneously for the first time, multiple instances may be created, violating the Singleton guarantee.

---

## Thread-Safe Singleton Implementations

### Lock-Based Singleton

To ensure thread safety, we can synchronize the `GetInstance()` method using a lock:

```csharp
public class Singleton {
    private static Singleton _instance;
    private string _value;
    private static readonly object _singletonLock = new object();

    private Singleton() { }

    public static Singleton GetInstance() {
        lock (_singletonLock) {
            if (_instance == null) {
                _instance = new Singleton();
            }
        }
        return _instance;
    }

    public string GetValue() {
        return _value;
    }

    public void SetValue(string value) {
        _value = value;
    }
}
```

### Benefits
- Guarantees thread safety.

### Drawbacks
- Locking is **expensive**, and every call to `GetInstance()` involves acquiring a lock, even if the instance is already initialized.

---

### Double-Checked Locking Singleton

We can optimize the lock-based implementation using **double-checked locking**, reducing unnecessary locking:

```csharp
public class Singleton {
    private static Singleton _instance;
    private string _value;
    private static readonly object _singletonLock = new object();

    private Singleton() { }

    public static Singleton GetInstance() {
        if (_instance == null) { // First check
            lock (_singletonLock) {
                if (_instance == null) { // Second check
                    _instance = new Singleton();
                }
            }
        }
        return _instance;
    }

    public string GetValue() {
        return _value;
    }

    public void SetValue(string value) {
        _value = value;
    }
}
```

### How It Works
1. **First null check**: Ensures that the lock is only acquired when the instance is uninitialized, improving performance.
2. **Second null check**: Ensures only one instance is created, even if multiple threads pass the first check.

### Benefits
- Thread-safe.
- Efficient after the instance is initialized, as subsequent calls avoid locking.

### Drawbacks
- Slightly more complex than simpler implementations.

---

## Summary of Implementations

| Implementation Type       | Thread-Safe | Performance | Complexity |
|---------------------------|-------------|-------------|------------|
| Eager Singleton           | ✅           | High        | Simple     |
| Lazy Singleton            | ❌           | Medium      | Simple     |
| Lock-Based Singleton      | ✅           | Low         | Medium     |
| Double-Checked Singleton  | ✅           | High        | Advanced   |