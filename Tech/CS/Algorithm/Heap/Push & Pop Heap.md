---
tags: Algorithm, Generic
---
## Push

Taking the same binary heap from before: `[14,19,16,21,26,19,68,65,30,null,null,null,null,null,null]`, let's say we wish to push `17`. We need to make sure we push `17` such that we maintain our structure and order property.

Since a binary heap is a complete binary tree, and we are required to fill nodes in a contigious fashion, pushing `17` should happen at the 1010th index. However, this might violate the min heap property, which means we will have to percolate `17` up the tree until we find its correct position.

In this case, because `17` is greater than its parent, `26`, it needs to percolate up until it is no longer greater than its parent. So, we swap `17` with `26` and now `17`'s parent is `19`, which again violates the min-heap property. We perform another swap and now `17` is greater than its parent, which is `14`. `17` is also smaller than all of its descendents because `19` was already smaller than all of its descendents. The resulting min-heap would look like the following.

![dsa-heaps-pushandpop](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/62bed605-429f-4c70-6eeb-5d88328c7300/sharpen=1)

```javascript
push(val) {
    this.heap.push(val);
    let i = this.heap.length - 1;

    // Percolate up
    while (i > 1 && this.heap[i] < this.heap[Math.floor(i / 2)]) {
        let tmp = this.heap[i];
        this.heap[i] = this.heap[Math.floor(i / 2)];
        this.heap[Math.floor(i / 2)] = tmp;
        i = Math.floor(i / 2);
    }
}
```

> The "//" indicates taking the floor of the resulting answer so we can round down, e.g. 5.55.5 becomes 55 since indices are whole numbers.

Since we know the tree will always be balanced, the time complexity of the push operation is O(log n).

---

## Pop

### The obvious way

Popping from a heap is more complicated than the push operation. One way that you might have already thought about is pop the root node and replace it with `min(left_child, right_child)`. The issue here is that while the order property is intact, we have violated the structure property. Taking the tree from before, popping `14`, and swapping it with `16` - `min(left_child, right_child)` would require `19` to replace `16`. Now, level 22 has a missing node i.e `19` is missing a left child.

### The correct way

The correct way is to take the right-most node of the last level and swap it with the root node. We have now maintained the structure property. However, the order property is violated. To fix the order property, we have to make sure that `30` finds its place. To do so, we will run a loop and swap `30` with `min(left_child, right_child)`. We swap `30` with `16`, then `19` with `30`. The resulting tree will look like the following.

![dsa-heaps-pushandpop](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/c9b06f3d-0661-4fce-f281-3922384e3100/sharpen=1)

```javascript
pop() {
    if (this.heap.length == 1) {
        // Normally we would throw an exception if heap is empty.
        return -1;
    }
    if (this.heap.length == 2) {
        return this.heap.pop();
    }

    let res = this.heap[1];
    // Move last value to root
    this.heap[1] = this.heap.pop();
    let i = 1;
    // Percolate down
    while(2 * i < this.heap.length) {
        if (2*i + 1 < heap.length &&
        this.heap[2 * i + 1] < this.heap[2 * i] &&
        this.heap[i] > this.heap[2 * i + 1]) {
            // Swap right child
            let tmp = this.heap[i];
            this.heap[i]= this.heap[2 * i + 1];
            this.heap[2 * i + 1] = tmp;
            i = 2 * i + 1;
        } else if (this.heap[i] > this.heap[2 * i]) {
            // Swap left child
            let tmp = this.heap[i];
            this.heap[i] = this.heap[2 * i];
            this.heap[2 * i] = tmp;
            i = 2 * i;
        } else {
            break;
        }
    }
    return res;
}
```

The pseudocode shown above might seem daunting at first so let's go over it. If our `heap` is empty, there is nothing to pop, hence the `return null`. Our heap also could have just one node, in which case, we will just pop that node and don't need to make any adjustments. If the above two statements have not executed, it must be the case that we have children, meaning we need to perform a swap.

We store our `14` into a variable called `res` so that we don't lose it. Then, we can pop from our heap, and replace `30` to be at the root node.

Our while loop runs as long as we have a left child and we determine this by making sure `2 * i` is not out of bounds. Then, there are three cases we concern ourselves with:

1. The node has no children
2. The node _**only**_ has a left child
3. The node has two children

> When considering a binary heap, it is not possible to have only a right child because then it no longer is a complete binary tree and violates the structure property.

Because we are guarenteed to have a left child in the while loop, we need to now check if the node also has a right child, which we check by `2 * i + 1`. We also make sure that the current node is greater than its children because of the order property. We replace the node with the minimum of its two children.

If no right child exists and the current node's value is greater than its left child, we swap it with the left child.

If none of the above cases exectue, then it must be the case that our node is in the proper position already, satisfying both the order and the structural property.