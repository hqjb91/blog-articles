# Currying in JavaScript

An overview of currying in JavaScript, including a naive implementation and a generalized implementation for handling multiple arguments.

---

## Naive Implementation

### Code Example
```javascript
const curry = (callback) => {
  return (a) => {
    return (b) => {
      callback(a, b);
    };
  };
};
```

### Explanation
The naive implementation of currying restricts the arguments to exactly two. Here's how it works:

1. **First Call**: Returns a higher-order function with the `callback` enclosed in its scope.
2. **Second Call**: The first argument is closed as a variable, returning a new function that takes the second argument and executes the `callback`.

### Usage
```javascript
const curriedSum = curry((a, b) => console.log(a + b));
// Returns a higher-order function that encapsulates the callback.

const curriedSumWithFour = curriedSum(4);
// Closes the first argument (`a`) as 4.

curriedSumWithFour(5); // Outputs 9 (4 + 5)
```

---

## Generalized Implementation

### Code Example
```javascript
const curry = (callback) => {
  return curried = (...args) => {
    if (args.length >= callback.length) return callback.apply(this, args);
    else return (...args2) => {
      return curried.apply(this, args.concat(args2));
    };
  };
};
```

### Explanation
This implementation generalizes currying to support functions with any number of arguments (except for those with indeterminate length via the spread operator). 

1. **Initial Call**: Captures arguments provided so far.
2. **Recursive Calls**: Checks if the number of arguments matches the original function's parameters:
   - If yes, executes the `callback`.
   - If no, returns a new function to capture more arguments.

### Usage
```javascript
const curriedSum = curry((a, b, c) => a + b + c);

console.log(curriedSum(1)(2, 3)); // Outputs 6
console.log(curriedSum(1)(2)(3)); // Outputs 6
```