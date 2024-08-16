#### Naive implementation

At its very core, a Singleton is an object that can only be **instantiated once** in an entire application and every user can only access that one unique instance.

Hence, at its most basic form, a singleton class must :
1. Not allow the creation of instances by **setting the constructor private**
2. Store the **unique instance as a private field** accessible only by the getter method
3. **Initialize the unique instance in the field (eagerly create)** or initialize the unique instance **only in the getter method (lazily create)**
4. **Getter method and field storing singleton must be static** as we are not using constructor to initialize

Example for an eagerly created Singleton :
```
public class Singleton {
    private static Singleton _instance = new();
    private string _value;
    private Singleton() { }
    public static Singleton getInstance() {
        return _instance;
    }
    public string getValue() {
        return _value;
    }
    public void setValue(string value){
        _value = value;
    }
}
```

However an eagerly created Singleton is potentially resource-intensive. If the initialization of the class is resource intensive (e.g. takes up significant computing resource/network traffic) we may wish to only create it when requested. Hence, we can opt for the lazily created approach.

Example for a lazily created Singleton :
```
public class Singleton {
    private static Singleton _instance;
    private string _value;
    private Singleton() { }
    public static Singleton getInstance() {
        if (_instance is null) _instance = new();
        return _instance;
    }
    public string getValue() {
        return _value;
    }
    public void setValue(string value){
        _value = value;
    }
}
```

The above implementation is sufficient for Single-Threaded application. However, unlike the eagerly created Singleton, the lazily created Singleton is not thread safe. 

Imagine the case where two separate threads access the getter method for the first time. They will create two different instances of the Singleton class and hence defying the uniqueness of the Singleton design pattern.
#### Thread Safe Lazily Initialized Singleton

One way to turn the above implementation thread safe would be to use a lock on the getter method :

```
public class Singleton {
    private static Singleton _instance;
    private string _value;
    private static object singletonLock = new object();
    private Singleton() { }
    public static Singleton getInstance() {
	    lock (singletonLock)
	    {
	        if (_instance is null) 
	        {
		        _instance = new();
		    }
	    }
        return _instance;
    }
    public string getValue() {
        return _value;
    }
    public void setValue(string value){
        _value = value;
    }
}
```

The above implementation works and is thread safe. However, performing the lock is expensive as we can imagine the multiple threads trying to access the Singleton object via the getter method having to wait for one another.

We improve the performance by using the double-checked locking :

```
public class Singleton {
    private static Singleton _instance;
    private string _value;
    private static object singletonLock = new object();
    private Singleton() { }
    public static Singleton getInstance() {
	    if  (_instance is null) 
	    {
	    	lock (singletonLock)
		    {
		        if (_instance is null) 
		        {
			        _instance = new();
			    }
		    }
	    }
        return _instance;
    }
    public string getValue() {
        return _value;
    }
    public void setValue(string value){
        _value = value;
    }
}
```

In the above pattern the first null check in the getter method is only for performance reasons as a gatekeeper to avoid the getter method being unnecessarily locked. Whereas the second inner null check is the actual check that being locked only one unique instance shall be created.