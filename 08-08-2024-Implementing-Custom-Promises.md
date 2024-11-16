# Implementing a Simple Promise in JavaScript

This article demonstrates how to create a custom implementation of Promises in JavaScript. The implementation progresses through naive, asynchronous, and chaining support, leading to a fully functional Promise structure.

## Naive Implementation

### Code

```javascript
const state = {
	fulfilled: "FULFILLED",
	pending: "PENDING",
	rejected: "REJECTED"
};

class CustomPromise {
	_value = null;
	_state = state.pending;

	constructor(callback) {
		try {
			callback(this._resolve.bind(this), this._reject.bind(this));
		} catch (error) {
			this._reject(error);
		}
	}

	_resolve(value) {
		if (this._state !== state.pending) return;
		this._state = state.fulfilled;
		this._value = value;
	}

	_reject(value) {
		if (this._state !== state.pending) return;
		this._state = state.rejected;
		this._value = value;
	}

	then(onFulfilled, onRejected) {
		if (this._state === state.fulfilled) {
			onFulfilled(this._value);
		} else if (this._state === state.rejected) {
			onRejected(this._value);
		}
	}
}
```

### Analysis

- **Limitations**:
  - Lacks support for asynchronous execution.
  - The `then` method does not wait for the Promise to resolve/reject before invoking callbacks.
  - No support for chaining `then` calls.

---

## Adding Asynchronous Support

### Code

```javascript
const state = {
	fulfilled: "FULFILLED",
	pending: "PENDING",
	rejected: "REJECTED"
};

class CustomPromise {
	_value = null;
	_state = state.pending;
	_onFulfilledCallbacks = [];
	_onRejectedCallbacks = [];

	constructor(callback) {
		try {
			callback(this._resolve.bind(this), this._reject.bind(this));
		} catch (error) {
			this._reject(error);
		}
	}

	_resolve(value) {
		if (this._state !== state.pending) return;
		this._state = state.fulfilled;
		this._value = value;
		this._onFulfilledCallbacks.forEach((callback) => callback(value));
	}

	_reject(value) {
		if (this._state !== state.pending) return;
		this._state = state.rejected;
		this._value = value;
		this._onRejectedCallbacks.forEach((callback) => callback(value));
	}

	then(onFulfilled, onRejected) {
		if (this._state === state.pending) {
			this._onFulfilledCallbacks.push(onFulfilled);
			this._onRejectedCallbacks.push(onRejected);
		} else if (this._state === state.fulfilled) {
			onFulfilled(this._value);
		} else if (this._state === state.rejected) {
			onRejected(this._value);
		}
	}
}
```

### Enhancements

- Added `_onFulfilledCallbacks` and `_onRejectedCallbacks` arrays to queue callbacks while the promise is pending.
- Updated `then` to handle pending state by storing callbacks for later execution.

---

## Adding Chaining Support

### Code

```javascript
const state = {
	fulfilled: "FULFILLED",
	pending: "PENDING",
	rejected: "REJECTED"
};

class CustomPromise {
	_value = null;
	_state = state.pending;
	_onFulfilledCallbacks = [];
	_onRejectedCallbacks = [];

	constructor(callback) {
		try {
			callback(this._resolve.bind(this), this._reject.bind(this));
		} catch (error) {
			this._reject(error);
		}
	}

	_resolve(value) {
		if (this._state !== state.pending) return;
		this._state = state.fulfilled;
		this._value = value;
		this._onFulfilledCallbacks.forEach((callback) => callback(value));
	}

	_reject(value) {
		if (this._state !== state.pending) return;
		this._state = state.rejected;
		this._value = value;
		this._onRejectedCallbacks.forEach((callback) => callback(value));
	}

	then(onFulfilled, onRejected) {
		return new CustomPromise((resolve, reject) => {
			const callbackWrapper = (callback, value, resolve, reject) => {
				try {
					const result = callback(value);
					if (result instanceof CustomPromise) {
						result.then(resolve, reject);
					} else {
						resolve(result);
					}
				} catch (error) {
					reject(error);
				}
			};

			if (this._state === state.pending) {
				this._onFulfilledCallbacks.push(() =>
					callbackWrapper(onFulfilled, this._value, resolve, reject)
				);
				this._onRejectedCallbacks.push(() =>
					callbackWrapper(onRejected, this._value, resolve, reject)
				);
			} else if (this._state === state.fulfilled) {
				callbackWrapper(onFulfilled, this._value, resolve, reject);
			} else if (this._state === state.rejected) {
				callbackWrapper(onRejected, this._value, resolve, reject);
			}
		});
	}
}
```

### Enhancements

- Modified `then` to return a new `CustomPromise`.
- Added support for chaining `then` calls by resolving/rejecting the result of the callback.

---

## Test Case

```javascript
const testPromise = new CustomPromise((resolve, reject) => {
	setTimeout(() => {
		resolve("Testing Promise 1 after 1 second");
	}, 1000);
}).then((result) => {
	console.log(result);
});
```
