#### Debounce
```
const debounce = (callback, delay) => {
    let timeout;
    return (...args) => {
        clearTimeout(timeout);
        timeout = setTimeout(() => {
            callback(...args);
        }, delay);
    }
}

const debouncedConsoleLog = debounce((text) => {
    console.log(text);
}, 3000);

debouncedConsoleLog('hello');
```
#### Throttle
```
const throttle = (callback, delay) => {
	let beingThrottled = false;
	return (...args) => {
		if (!beingThrottled) { // If it is not being throttled we run the callback
			callback(...args);
			beingThrottled = true; // Block for delay after running
			setTimeout(() => {
				beingThrottled = false; // Unblock after delay
			}, delay);
		}
	}
}

const throttledConsoleLog = throttle((text) => {
    console.log(text);
}, 3000);

throttledConsoleLog('hello');
```