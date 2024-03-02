---
tags: Algorithm, Generic
---

By now, you are probably getting tired of sorting. Don't worry, this is the last sorting algorithm we will cover, and it is a pretty important one. It is not as popular or widely used as the previous algorithms we have covered. Bucket sort works well when the dataset to be sorted has values **within a specific range**.


## Concept

Imagine we have an array of size 6 and it contains values of an inclusive range of 0−2. The idea behind bucket sort is to create a "bucket" for each one of the numbers and map them to their respective buckets.

There will be a bucket for 0, 1 and 2. This bucket, which is just a position in a specified array will contain the frequencies of each one of the values within the range. For the sake of this example, we only have three values and accordingly we will have three buckets.

> The term bucket will really just translate into a position in an array where we will map these frequencies.

Once each one of the buckets is filled with the frequency of each one of the values, we will overwrite all the values in the original array such that they end up in the sorted order. This makes more sense looking at the pseudocode below:

```javascript
function bucketSort(arr) {
    // Assuming arr only contains 0, 1 or 2
    const counts = [0, 0, 0];

    // Count the quantity of each val in arr
    for (let i = 0; i < arr.length; i++) {
        counts[arr[i]] += 1;
    }

    // Fill each bucket in the original array
    let i = 0;
    for (let n = 0; n < counts.length; n++) {
        for (let j = 0; j < counts[n]; j++) {
            arr[i] = n;
            i++;
        }
    }
    return arr;
}
```

The first part of the pseudocode, right before we do `i = 0`, corresponds to filling up each one of the buckets. Let's explain the second part of the code a bit more.

```javascript
let i = 0;
for (let n = 0; n < counts.length; n++) {
    for (let j = 0; j < counts[n]; j++) {
        arr[i] = n;
        i++;
    }
}
```

- The `i` pointer will keep track of the next insertion position for our original array, `arr`.
- The `n` pointer keeps track of the current position of the `counts` array.
- The `j` pointer keeps track of the number of times that `counts[n]` has appeared.

So, knowing that, we have our `counts` array which is `[2,1,3]`. And, our original input array is `[2,1,2,0,0,2]`.

At the first iteration, n=0, which corresponds to 2 in `counts`. 

Our inner loop will run two times, overwriting `arr[0]` and `arr[1]` to be `0`. This makes perfect sense because we had two zeros and putting them in `arr` in an adjacent manner will result in them being sorted. This process will continue for each number and the ultimate state of `arr` will be `[0,0,1,2,2,2]` which is the ultimate goal.

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/1521e7e2-4f63-4326-38cd-f32bcd9d3400/sharpen=1)
