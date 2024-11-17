# Sorting Algorithm Comparison and Implementation in Javascript

## Quick Sort

### Quick Sort : Time Complexity
- **Average case**: nlogn
  - Uses the divide-and-conquer strategy. Example with 8 elements:
    - Step 1: Split into 4 batches of 2 elements.
    - Step 2: Merge into 2 batches of 4 elements.
    - Step 3: Final merge into 1 batch of 8 elements.
    - Total steps = log2(8) = 3.
- **Worst case**: O(n^2) (occurs when the pivot selection is poor, e.g., sorted input with a bad pivot choice).

### Quick Sort : Advantages
- Faster than merge sort on average, with less overhead.
- Performs in-place sorting, requiring less memory compared to merge sort.

### Quick Sort : Disadvantages
- Not stable: Elements with the same value might not retain their relative order.
- Worst-case performance can degrade to O(n^2) with poor pivot choices.

### Quick Sort : Implementation in JavaScript
```javascript
const testInput = [1, 9, 8, 3, 6, 5, 7, 7];

const quickSort = (input) => {
    if (input.length < 2) return input; // Base case for recursion

    const pivot = input[0]; // Choose the first element as the pivot
    const left = input.slice(1).filter(el => el < pivot); // Elements less than pivot
    const right = input.slice(1).filter(el => el >= pivot); // Elements greater than or equal to pivot

    return [...quickSort(left), pivot, ...quickSort(right)]; // Recursively sort and combine
};

const sortedArray = quickSort(testInput);
console.log(sortedArray); // Output: [1, 3, 5, 6, 7, 7, 8, 9]
```

---

## Merge Sort

### Merge Sort : Time Complexity
- **Best, Worst, and Average case**: \(O(n\log n)\).
- Uses divide-and-conquer but requires additional memory for temporary arrays.

### Merge Sort : Advantages
- Stable: Retains the relative order of elements with equal values.
- Consistent \(O(n\log n)\) performance, even for worst-case inputs.

### Merge Sort : Disadvantages
- Slower than quick sort in practice due to additional memory usage.
- Requires extra space for temporary arrays, which makes it less efficient for large datasets.

### Merge Sort : Implementation in JavaScript
```javascript
const testInput = [1, 9, 8, 3, 6, 5, 7, 7];

const mergeSort = (input) => {
    if (input.length < 2) return input; // Base case for recursion

    const mid = Math.floor(input.length / 2); // Find the middle index
    const left = mergeSort(input.slice(0, mid)); // Recursively sort the left half
    const right = mergeSort(input.slice(mid)); // Recursively sort the right half

    return merge(left, right); // Merge the sorted halves
};

// Helper function to merge two sorted arrays
const merge = (left, right) => {
    const result = [];
    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift()); // Remove from left and push to result
        } else {
            result.push(right.shift()); // Remove from right and push to result
        }
    }
    return [...result, ...left, ...right]; // Concatenate remaining elements
};

const sortedArray = mergeSort(testInput);
console.log(sortedArray); // Output: [1, 3, 5, 6, 7, 7, 8, 9]
```