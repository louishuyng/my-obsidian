---
tags: Algorithm, Generic
---
The idea behind quicksort is to pick an index, which is called the `pivot`, and partition the array such that every value to the left is less than or equal to the `pivot` and every value to the right is greater than the pivot.

### Picking a pivot value

Generally, there are several tested and tried options when it comes to picking a pivot:

- Pick the first index
- Pick the last index
- Pick the median (pick the first, last and the middle value and find their median and perform the split at the median)
- Pick a random pivot

You may be asking which pivot to pick? This is dependent on the data itself, both the size and the initial order. For coding interviews we can keep things simple, so in this chapter we will use the last index as our pivot.

### Implementation

We will pick a `pivot` if we haven't already hit the base case which is array of size `1` and pick a `left` pointer, which will point to the left-most element of the current subarray to begin with. Then, we will iterate through our array and if we find an element smaller than our `pivot` element, we will swap the current element with the element at our `left` pointer and increment the `left` pointer.

Once this condition terminates, we will bring the `left` element to the end of the array and bring the pivot element to the `left` position. This makes sense because once we have exhausted all the elements that are smaller than the `pivot` element, and put them to the left of the pivot, `left` will now be at the first element that is greater than the pivot. And, every element after it will also be greater than the pivot. This is why we move `left` element to the end and then bring the pivot element to the `left`.

This results in all the elements less than or equal to the `pivot` to be on the left and the rest on the right.

Let's take the array `[6,2,4,1,3]` to sort.

### Performing a partition

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/4807fbac-d636-4b43-654d-345988be0500/sharpen=1)

As seen above, in the resulting array, we will have sorted the array such that all elements to the left are smaller than the `pivot` with the rest being on the right.

> We are not going to visualize all of the steps but this process will be run recursively until we hit the base-case. It is important to notice that we are not allocating any new memory for these partitions. We are just moving around pointers to work on a smaller part of the original array each time until we end up with a sorted array.

The pseudocode for this looks like the following.

```javascript
function quickSort(arr, s, e) {
    if (e - s + 1 <= 1) {
        return arr;
    }

    let pivot = arr[e];
    let left = s;       // pointer for left side

    // Partition: elements smaller than pivot on left side
    for (let i = s; i < e; i++) {
        if (arr[i] < pivot) {
            let tmp = arr[left];
            arr[left] = arr[i];
            arr[i] = tmp;
            left++;
        }
    }

    // Move pivot in-between left & right sides
    arr[e] = arr[left];
    arr[left] = pivot;

    // Quick sort left side
    quickSort(arr, s, left - 1);

    // Quick sort right side
    quickSort(arr, left + 1, e);

    return arr;
}
```