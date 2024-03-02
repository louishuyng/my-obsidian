---
tags: Algorithm, Generic
---
The concept behind merge sort is very simple. Keep splitting the array into halves until the subarrays hit a size of one, sort them, and merge the sorted arrays, which will result in an ultimate sorted array. You might have figured out that this sounds exactly like the fibonacci sequence using recursion, and you would be right! We can, and will be using recursion to perform this. More specifically, two branch recursion.

Let's take an array of size 55 as an example, `[3,2,4,1,6]`. Our job is to make sure that we sort this in an increasing, or non-decreasing order if we had duplicates. We will be splitting the array like the following.

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/55728d04-816e-4176-985c-e82da4112b00/sharpen=1)

As observed, we have two branches. Let's work on sorting and merging the left branch first. The work required here is that we will have to hit the base case first, after which we can begin sorting and merging the arrays together, achieving `[2,3,4]` as a result. Once our recursion reaches the base case, we have two subarrays, `[3]` and `[2]`. We need a way to compare these two elements to know where to put them in their original subarray, which is `[3,2]`. For this, copies of both the subarrays is created and using two-pointers, values are compared and put in the original subarray in the sorted order. This can be seen in the pseudocode below.

```javascript
function mergeSort(arr, l,  r) {
    if (l < r) {
        // Find the middle point of arr
        let m = Math.floor((l + r) / 2);

        mergeSort(arr, l, m);   // sort left half
        mergeSort(arr, m+1, r); // sort right half
        merge(arr, l, m, r);    // merge sorted halfs
    }
    return arr;
}
```


## Visualization and Pseudocode

### The `mergeSort()` recursive call

As we learned with two branch recursion, we solve both the branches and 'piece' back together the solutions to the subproblems to arrive at the ultimate solution. Once we have the subarray `[3,2]` sorted to `[2,3]` - this is the `mergeSort(arr, s, m)` part. Now, we can move on to sorting the `[4]`, which corresponds to the `mergeSort(arr, m + 1, e)`. It is important to note the sequence in which the calls are exectued. The `merge()` call will not be exectued until both the recursive `mergeSort()` calls have returned for the current subarray. The first visual below shows sorting and merging the left half. The second visual shows sorting and merging the second half to get the ultimate sorted array.

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/23d6fbdf-2fe5-4ff6-2c55-473b789a9600/sharpen=1)

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/d497f50f-b72f-4038-06e3-fbbf60ac1000/sharpen=1)

> For the sake of the example and making sure that the above visual is not confusing, the visual above shows how the final array, which is the resulting array, comes about. It visualizes what is going on inside the `merge()` function.

#### The `merge()` function and three pointers

As observed in the visual above, we have three pointers, `k`, `j` and `i`.

- `k` keeps track of where the next element in `arr` needs to be placed.
- `i` points to the element in the `leftSubarray` that is currently being compared to the `j` element in the `rightSubarray`.
- One of `i` or `j` will increment depending on which element in smaller.
- `k` will increment regardless because `arr` will have an element placed inside of it regardless of which one of `i` or `j` increments.

This is clear in the visual above and demonstrated in the `merge()` function pseudocode below.

```javascript
// Merges two subarrays of arr[].
// First subarray is arr[l..m]
// Second subarray is arr[m+1..r]
function merge(arr, l,  m, r) { 
    
    // Find lengths of two subarrays to be merged
    let length1 = m - l + 1;
    let length2 = r - m;

    // Create temp arrays 
    let L = new Array(length1);
    let R = new Array(length2);

    // Copy the sorted left & right halfs to temp arrays
    for (let i = 0; i < length1; i++) {
        L[i] = arr[l + i];
    }
    
    for (let j = 0; j < length2; j++) {
        R[j] = arr[m + 1 + j];
    }
    
    // initial indexes of left and right sub-arrays
    let i = 0; // index for left
    let j = 0; // index for right
    let k = l; // Initial index of merged subarray array

    // Merge the two sorted halfs leto the original array
    while (i < length1 && j < length2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    // One of the halfs will have elements remaining
    while (i < length1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    while (j < length2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}
```
