Adapted from https://medium.com/swlh/implement-a-simple-promise-in-javascript-20c9705f197a
#### Naive Implementation

```
const state = {
	fulfilled: "FULFILLED",
	pending: "PENDING",
	rejected: "REJECTED"
}

class CustomPromise {

	_value = null;
	_state = state.pending;
	
	constructor CustomPromise(callback) {
		try {
			callback(_resolve, _reject);
		} catch(error) {
			_reject(error);
		}
	}

	_resolve(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.fulfilled;
		this._value = value;
	}
	
	_reject(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.rejected;
		this._value = value;
	}
	
	then(onFulfilled, onRejected) { // Callbacks after current promise is fulfilled/rej
		if (this._state === state.fulfilled) {
			onFulfilled(this._value);
			// Call the onFulfilled callback after promise is fulfilled and value is set
		} else if (this._state === state.rejected) {
			onRejected(this._value);
		}
	}
}
```

First naive implementation of Custom Promise
- Does not support asynchronous execution, the then method does not wait for the Promise to be fulfilled/pending before running 
	- e.g. if we have setTimeout to resolve the Promise the onFulfilled callback will not be called
	- when we call .then(), the promise is still pending
- No support of chaining for the then method

#### Async Support

```
const state = {
	fulfilled: "FULFILLED",
	pending: "PENDING",
	rejected: "REJECTED"
}

class CustomPromise {

	_value = null;
	_state = state.pending;
	_onFulfilledCallbacks = [];
	_onRejectedCallbacks = [];
	
	constructor CustomPromise(callback) {
		try {
			callback(_resolve, _reject);
		} catch(error) {
			_reject(error);
		}
	}

	_resolve(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.fulfilled;
		this._value = value;
		this._onFulfilledCallbacks.forEach(callback => callback(value));
	}
	
	_reject(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.rejected;
		this._value = value;
		this._onRejectedCallbacks.forEach(callback => callback(value));
	}
	
	then(onFulfilled, onRejected) { 
		// If current promise state is still pending we add to the callbacks for later
		if (this._state === state.pending) {
			this._onFulfilledCallbacks.push(onFulfilled);
			this._onRejectedCallbacks.push(onRejected);
		}

		// Callbacks after current promise is fulfilled/rejected
		if (this._state === state.fulfilled) {
			onFulfilled(this._value);
			// Call the onFulfilled callback after promise is fulfilled and value is set
		}
		
		if (this._state === state.rejected) {
			onRejected(this._value);
		}
	}
}
```

Added new arrays of onFulfilledCallbacks and onRejectedCallbacks to store the callbacks whilst the Promise is not yet resolved. 

Steps involved :
- Adding the onFulfilledCallbacks, onRejectedCallbacks as new fields to the class
- Modifying the then() method to store the callbacks in the fields if the state field is still pending
- Modifying the resolve() and reject() methods to loop through the onFulfilledCallbacks, onRejectedCallbacks and apply the stored functions to the resolved/rejected values, since this signifies that the state of the promise is no longer pending

#### Chaining Support

```
const state = {
	fulfilled: "FULFILLED",
	pending: "PENDING",
	rejected: "REJECTED"
}

class CustomPromise {

	_value = null;
	_state = state.pending;
	_onFulfilledCallbacks = [];
	_onRejectedCallbacks = [];
	
	constructor CustomPromise(callback) {
		try {
			callback(_resolve, _reject);
		} catch(error) {
			_reject(error);
		}
	}

	_resolve(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.fulfilled;
		this._value = value;
		this._onFulfilledCallbacks.forEach(callback => callback(value));
	}
	
	_reject(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.rejected;
		this._value = value;
		this._onRejectedCallbacks.forEach(callback => callback(value));
	}
	
	then(onFulfilled, onRejected) { 
		return new Promise((resolve, reject) => {
			if (this._state === state.pending) {
				this._onFulfilledCallbacks.push(() => {
					try {
						const fulfilledFromLastPromise = onFulfilled(this._value);
						resolve(fulfilledFromLastPromise);
					} catch(error) {
						reject(error);
					}
				});
	
				this._onRejectedCallbacks.push(() => {
					try {
						const rejectedFromLastPromise = onRejected(this._value);
						resolve(rejectedFromLastPromise);
					} catch(error) {
						reject(error);
					}
				});
			}
	
			if (this._state === state.fulfilled) {
				try {
					const fulfilledFromLastPromise = onFulfilled(this._value);
					resolve(fulfilledFromLastPromise);
				} catch(error) {
					reject(error);
				}
			}
			
			if (this._state === state.rejected) {
				try {
					const rejectedFromLastPromise = onRejected(this._value);
					resolve(rejectedFromLastPromise);
				} catch(error) {
					reject(error);
				}
			}
		})
	}
}
````

We need to modify the .then() method to return either a this object or a new promise. We are sticking with returning a new promise because we need to push the onFulfiiled and onRejected into the callbacks array. 

Steps are as follows :
- Right now .then() does not return anything, update it to return a new CustomPromise() object
- Move in the conditional logic checks for current promise state of either "pending", "fulfilled" or "rejected"
	- If current promise's state is "pending" rather than just pushing the onFulfilled and onRejected callback immediately into the callbacks array, we now :
		- Push a wrapper function instead that will :
			- Populate a constant variable called fulfilledFromLastPromise/rejectedFromLastPromise with the onFulfilled/onRejected callback applied on the current promise's value
			- Call resolve/reject on that constant variable
	- If current promise's state is "fulfilled"/"rejected" rather than just calling the onFulfilled/onRejected callback on the current promise's value we :
		- Populate a constant variable called fulfilledFromLastPromise/rejectedFromLastPromise with the onFulfilled/onRejected callback applied on the current promise's value
		- Call resolve/reject on that constant variable

#### Promise Resolution in Then

```
	then(onFulfilled, onRejected) { 
		return new Promise((resolve, reject) => {
			if (this._state === state.pending) {
				this._onFulfilledCallbacks.push(() => {
					try {
						const fulfilledFromLastPromise = onFulfilled(this._value);
						if (fulfilledFromLastPromise instance of CustomPromise) {
							fulfilledFromLastPromise.then(resolve, reject);
						} else {
							resolve(fulfilledFromLastPromise);
						}
					} catch(error) {
						reject(error);
					}
				});
	
				this._onRejectedCallbacks.push(() => {
					try {
						const rejectedFromLastPromise = onRejected(this._value);
						if (rejectedFromLastPromise instance of CustomPromise) {
							rejectedFromLastPromise.then(resolve, reject);
						} else {
							resolve(rejectedFromLastPromise);
						}
					} catch(error) {
						reject(error);
					}
				});
			}
	
			if (this._state === state.fulfilled) {
				try {
					const fulfilledFromLastPromise = onFulfilled(this._value);
					if (fulfilledFromLastPromise instance of CustomPromise) {
						fulfilledFromLastPromise.then(resolve, reject);
					} else {
						resolve(fulfilledFromLastPromise);
					}
				} catch(error) {
					reject(error);
				}
			}
			
			if (this._state === state.rejected) {
				try {
					const rejectedFromLastPromise = onRejected(this._value);
					if (rejectedFromLastPromise instance of CustomPromise) {
						rejectedFromLastPromise.then(resolve, reject);
					} else {
						resolve(rejectedFromLastPromise);
					}
				} catch(error) {
					reject(error);
				}
			}
		})
	}
```

There is the case we need to handle where a promise is returned inside of the .then() method

In the end the result is as such :

```
const state = {
	fulfilled: "FULFILLED",
	pending: "PENDING",
	rejected: "REJECTED"
}

class CustomPromise {

	_value = null;
	_state = state.pending;
	_onFulfilledCallbacks = [];
	_onRejectedCallbacks = [];
	
	constructor CustomPromise(callback) {
		try {
			callback(_resolve, _reject);
		} catch(error) {
			_reject(error);
		}
	}

	_resolve(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.fulfilled;
		this._value = value;
		this._onFulfilledCallbacks.forEach(callback => callback(value));
	}
	
	_reject(value) {
		if (this._state !== state.pending) return; // Ignore any further resolve/rejects
		this._state = state.rejected;
		this._value = value;
		this._onRejectedCallbacks.forEach(callback => callback(value));
	}
	
	then(onFulfilled, onRejected) { 
		return new Promise((resolve, reject) => {
			if (this._state === state.pending) {
				this._onFulfilledCallbacks.push(() => {
					try {
						const fulfilledFromLastPromise = onFulfilled(this._value);
						if (fulfilledFromLastPromise instance of CustomPromise) {
							fulfilledFromLastPromise.then(resolve, reject);
						} else {
							resolve(fulfilledFromLastPromise);
						}
					} catch(error) {
						reject(error);
					}
				});
	
				this._onRejectedCallbacks.push(() => {
					try {
						const rejectedFromLastPromise = onRejected(this._value);
						if (rejectedFromLastPromise instance of CustomPromise) {
							rejectedFromLastPromise.then(resolve, reject);
						} else {
							resolve(rejectedFromLastPromise);
						}
					} catch(error) {
						reject(error);
					}
				});
			}
	
			if (this._state === state.fulfilled) {
				try {
					const fulfilledFromLastPromise = onFulfilled(this._value);
					if (fulfilledFromLastPromise instance of CustomPromise) {
						fulfilledFromLastPromise.then(resolve, reject);
					} else {
						resolve(fulfilledFromLastPromise);
					}
				} catch(error) {
					reject(error);
				}
			}
			
			if (this._state === state.rejected) {
				try {
					const rejectedFromLastPromise = onRejected(this._value);
					if (rejectedFromLastPromise instance of CustomPromise) {
						rejectedFromLastPromise.then(resolve, reject);
					} else {
						resolve(rejectedFromLastPromise);
					}
				} catch(error) {
					reject(error);
				}
			}
		})
	}
}
```

We can also add the queueMicrotask function to wrap the onFulfilled/onRejected function as the behavior is right now not asynchronous

Also, quite confused about the scope of this in the then function. Need to clarify if my understanding is correct that only the resolve method called belongs to the new Promise scope, such that all the other states are based on the older and just resolved promise and then using those values we resolve it with the callback in the then method and then finally run the current promise's resolve on the previous resolved value that went through the callback.
#### Test Case 

```
const testPromise = new CustomPromise((resolve, reject) => {
	setTimeout(() => {
		resolve("Testing Promise 1 after 1 second");
	}, 1000);
}).then((result) => {
	console.log(result);
})
```