#### Naive Implementation

```
const curry = (callback) => {
	return (a) => {
		return (b) => {
			callback(a,b);
		}
	}
}
```

This is a naive implementation of currying as it limits the arguments to just two. This implementation can be used as follows :

```
const curriedSum = curry((a,b) => console.log(a+b));
// Returns a higher order function that has the callback (a,b) => console.log(a+b)
// closed and saved within the scope

const curriedSumWithFour = curriedSum(4); 
// This closes variable a as 4 and returns a function that takes in b and calls the callback above

curriedSumWithFour(5); // prints out 4+5 = 9
```

#### Generalized Implementation

```
const curry = (callback) => {
	return curried = (...args) => {
		if (args.length >= callback.length) return callback.apply(this, args);
		else return (...args2) => {
			return curried.apply(this, args.concat(args2)); // apply lets you use array
		}
	}
}
```

The generalized implementation allows for any number of parameters except for parameters with spread operator (indeterminate length).

```
const curriedSum = curry((a, b, c) => (a+b+c));

console.log(curriedSum(1)(2,3)); // prints 6
console.log(curriedSum(1)(2)(3));
```