Quick Sort
- Time Complexity
	- O(nlogn)
	- Divide and conquer, imagine completing half at each step
		- e.g. if you have 8 elements
			- 1st step 4 batches of 2 elements sorted
			- 2nd step 2 batches of 4 elements sorted
			- 3rd step 1 batch of 8 elements sorted
			- 3 steps = log2(8)
			- i.e. 2^3 = 8
- Advantages
	- Even though quick sort and merge sort are both O(nlogn) on average case quick sort is faster
	- Worst time complexity is actually O(n^2)
- Disadvantages
	- Not stable i.e. elements of the same ordinals may be sorted differently

Code in Javascript :

```
const testInput = [1,9,8,3,6,5,7,7];

const quickSort = (input) => {
	if (input.length < 2) return input; // Base case

	const pivot = input[0]; // Choose pivot to be left most element
	const left = []; // Initialize left and right arrays
	const right = [];
	for (let i=1; i<input.length; i++) { // Loop through rest of elements
		if (input[i] < pivot) { // Place all the elements lesser to left
			left.push(input[i]);
		} else {
			right.push(input[i]);
		}
	}
	return [...quickSort(left), pivot, ...quickSort(right)]; 
	// Where recursive leap of faith magick happens
}

const testAnswer = quickSort(testInput);
console.log(testAnswer);
```

Merge Sort
-  Time Complexity
	- O(nlogn)
- Advantages
	- Stable i.e. elements of the same ordinals are sorted equally
- Disadvantages
	- Average time complexity slower than quick sort

Code in Javascript :

```
const testInput = [1,9,8,3,6,5,7,7];

const mergeSort = (input) => {
	if (input.length < 2) return input; // Base case

	const middle = Math.floor(input.length/2);
	const left = input.slice(0, middle);
	const right = input.slice(middle);
	
	return merge(mergeSort(left), mergeSort(right)); 
	// Require helper function to perform recursive merge magick
}

// Helper function that merges 2 sorted arrays (We just assume they are)
const merge = (left, right) => { 
	const result = [];
	while (left.length > 0 && right.length > 0) {
		if (left[0] <= right[0]) result.push(left.shift());
		if (left[0] > right[0]) result.push(right.shift());
	}
	
	return [...result, ...left, ...right];
}

const testAnswer = mergeSort(testInput);
console.log(testAnswer);

```