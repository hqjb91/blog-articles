# Debouncing and Throttling in JavaScript

A guide to implementing and using debounce and throttle functions to control the execution of callbacks in response to frequent events.

---

## Debounce

### Code Example
```javascript
const debounce = (callback, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      callback(...args);
    }, delay);
  };
};

const debouncedConsoleLog = debounce((text) => {
  console.log(text);
}, 3000);

debouncedConsoleLog('hello');
```

### Explanation
Debouncing ensures a function is executed only after a specified delay has passed since the last invocation.

1. **Timeout Management**: 
   - Cancels any previously scheduled execution using `clearTimeout`.
   - Reschedules the execution after the specified delay.
2. **Usage Scenario**: Useful for scenarios like search input where execution should happen only after the user has stopped typing.

---

## Throttle

### Code Example
```javascript
const throttle = (callback, delay) => {
  let beingThrottled = false;
  return (...args) => {
    if (!beingThrottled) {
      callback(...args); // Executes the callback
      beingThrottled = true; // Blocks further executions
      setTimeout(() => {
        beingThrottled = false; // Unblocks after the delay
      }, delay);
    }
  };
};

const throttledConsoleLog = throttle((text) => {
  console.log(text);
}, 3000);

throttledConsoleLog('hello');
```

### Explanation
Throttling ensures a function is executed at most once in every specified interval, regardless of how often it is triggered.

1. **Blocking Mechanism**: 
   - The callback executes immediately if it's not throttled.
   - Further executions are blocked for the specified delay.
2. **Usage Scenario**: Ideal for limiting frequent executions, like handling scroll or resize events.