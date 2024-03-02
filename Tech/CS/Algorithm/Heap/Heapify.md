---
tags: Algorithm, Generic
---
Recall that to build a binary search tree, the time complexity is O(n log n). We could build our heap the same way and those operations would also run in O(log n) time. Heapify tells us there is a better way of doing it. It allows us to perform this operation in O(n) time.

## Concept

The idea behind using heapify to build a heap is to satisfy the structure and the order property. We need to make sure that our binary heap is a complete binary tree and that every node's value is at most its parent's value.

Because the leaf nodes can't violate the min-heap properties, there is no need to perform `heapify()` on them.

Since we are skipping all of the leaf nodes, we only need to start at `heap.length // 2`. Then, we need to percolate down the exact same way we did in the previous chapter in the `pop()` method. We will not be going over the code in detail as majority of it is the same as the `pop()` method.

```javascript
heapify(arr) {
    // 0-th position is moved to the end
    arr.push(arr[0]);

    this.heap = arr;
    let cur = Math.floor((this.heap.length- 1) / 2);
    while (cur > 0) {
        // Percolate Down
        let i = cur;
        while (2 * i < this.heap.length) {
            if (2 + i + 1 < this.heap.length &&
            this.heap[2 + i + 1] < this.heap[2 * i] &&
            this.heap[i] > this.heap[2 * i + 1]) {
                // Swap right child
                let tmp = this.heap[i];
                this.heap[i] = this.heap[2 * i + 1];
                this.heap[2 * i + 1] = tmp;
                i = 2 * i + 1;
            } else if (this.heap[i] > this.heap[2 * i]) {
                // Swap left child
                let tmp = this.heap[i];
                this.heap[i] = this.heap[2 * i];
                this.heap[2 * i]  = tmp;
                i = 2 * i;
            } else {
                break;
            }
        }
        cur--;
    }
    return;
}
```

Starting from the first non-leaf node, we will percolate down, the exact same way we did in the `pop()` function. After each iteration, we are going to decrement the index by 11 so we can perform `heapify()` on the next node, all the way until index 11.

The visual below demonstrates `heapify()` being performed on all nodes starting from index 44. The nodes in the blue rectangles are leaf nodes.

![heapify](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/ef4e44b2-100b-4f72-b18d-e244ad759500/sharpen=1)

---

## Time Complexity

Given that there are n nodes in a binary tree, there are roughly n / 2 leaf nodes. Using this information, we can figure out how many levels each node has to percolate down and the amount of work `heapify()` performs at each level.

We don't perform heapify at the 33rd / last level. The nodes on the 22nd level need to percolate down one level, and the nodes on the 11st level are percolating down two levels, with the root node having to percolate down all the levels. So while the number of nodes is halving each time, the number of levels needed to be percolated increases. There is a very neat mathematical summation that is formed when we add all the work together, which simplifies to O(n), but we will not be covering that. It is highly unlikely that you will be asked to prove the time complexity of `heapify()`, so it is enough to know that it is in O(n)

If you are interested in learning the proof behind why there are roughly n / 2 leaf nodes, these 5 slides from [Virginia Tech](https://courses.cs.vt.edu/~cs3114/Fall09/wmcquain/Notes/T03a.BinaryTreeTheorems.pdf) are valuable.