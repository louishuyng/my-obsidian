---
tags: Algorithm, Generic
---

# BST
## Difference between Binary Trees and Binary Search Trees

Binary Search Trees (BST) are a variation of binary trees with a sorted property to them. The property tells us that every left child must be smaller than its parent and every right child must be greater than its parent. BSTs _do not_ allow duplicates.

> It should be noted that the BST property applies to subtrees as well. So while a node which has a value smaller than root will be on the left subtree, it is important to determine where exactly in the left subtree that value will be inserted.


## Motivation

The question here is why bother with BSTs if we have sorted arrays? With binary search, we can search values in O(log n) time and if BST is offering the same functionality, can't we just use an array? All of this is correct. However, BST shines when we are trying to insert or delete a value. Both of these operations run in O(log n) time for a BST, but O(n) time with an array.

So, while BSTs don't offer benefits over sorted arrays for the search functionality, they are better for insertion and deletion. In this chapter, we will focus specfically on the search operation.

---

## BST Search

Trees are best traversed using recursion. You could traverse iteratively, however, that requires maintaining a stack, which is a lot more complicated. For recursion, as discussed before, we need a base case and the function calling itself.

Let's take the tree `[2,1,3,null,null,null,4]` for example, and search for `target = 3`.

In binary search, if the current element was greater than the target, we went left and if the current element was smaller than the target, we went right. A similar approach can be taken here. We know all nodes to the left are smaller than our current node and all nodes to the right are greater than our current node. Knowing this, we can go right if our current node is smaller than the target and go left if the current node is greater than the target.

If the target exists in the tree, we will return true. Otherwise, we return false.

In the case of the example, we first start by comparing the root value against the `target`. `2` is too small, so our target must be on the right, meaning we can eliminate the left-subtree. When we go right, the first node is `3`, which equals `target`, so we return true from the recursive call, meaning our target does exist in the tree.

This is demonstrated by the pseudocode and the visual below.

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/9c42ae26-33f4-4110-e49b-509c7dae3600/sharpen=1)

```javascript
function search(root, target) {
    if (root == null) {
        return false;
    }

    if (target > root.val) {
        return search(root.right, target);
    } else if (target < root.val) {
        return search(root.left, target);
    } else {
        return true;
    }    
}
```


---

## Time Complexity

If we have a balanced binary tree, our search algorithm will run in )O(log n) time. Balanced binary tree means that the height of the left-subtree is equal to the height of the right-subtree, or there is a difference of 11. In a balanced tree, we can eliminate half the nodes each time, which results in O(log n), for reasons we discussed in the merge sort chapter.

If we had a skewed binary tree, this would result in a time complexity of O(n). This is also the worst case scenario.

# # Insertion and Removal from a Binary Search Tree

---

We mentioned previously that the main benefit of using BSTs over sorted arrays is that we can perform removal and insertion in O(log n) time, assuming that the tree is balanced. Let's dig a little deeper into the insertion and deletion.

---

## Insertion

If we wish to insert a new node into the BST, we first have to traverse the BST to find the right position, and then insert this node.

If we have a BST `[4]`, and wish to insert 66, we could either end up with `[4,null,6]` or `[6,4,null]`. Both of these would be valid BSTs. In the first example, we added the 66 as a leaf node, which is an easier process than the second example.

Let's add `5` to previously resulting tree `[4,null,6]`, which results in `[4,null,6,5,null]`.

This process is demonstrated by the pseudocode below.

```javascript
// Insert a new node and return the root of the BST.
function insert(root, val) {
    if (root == null) {
        return new TreeNode(val);
    }

    if (val > root.val) {
        root.right = insert(root.right, val);
    } else  if (val < root.val) {
        root.left = insert(root.left, val);
    }
    return root;
}
```


![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/3058308c-aaea-4f7c-c312-7001bbd0f800/sharpen=1)The visual above demonstrates how insertion is done. 66 is greater than the root node, so it ends up in the right sub-tree. 55 is greater than the root node but smaller than 66 so it ends up in the left subtree of the tree rooted at 66.

---

## Removal

Before removing a node from a BST, we need to consider two cases:

1. The target node has 0 or 1 child
2. The target node has 2 children

### Case 1 - The target node has one child or no children

If we wish to delete node `2`, which has no children, the `left_child` pointer of `3` now points to `null`.

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/b8f1af6e-2fe6-4429-8765-abf1b92e7a00/sharpen=1)

If we wish to delete node `3`, which has one child, the `left_child` pointer of the root node will point to `2` instead of `3`.

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/2fa45c14-0ff4-4fdb-20de-795041eeb800/sharpen=1)

### Case 2 - The target node has two children

If we wanted to delete a node with two children, say, `6`, we replace the node with its **in-order successor.**

The in-order successor is the left-most node in the right subtree of the target node. Another way of looking at it is that it is the smallest node among all the nodes that are greater than the target node. This will ensure that the resulting tree is still a valid binary search tree.

The visual below shows process of deletion of nodes with two children.

![alt text](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/a4669802-eadd-483f-34e7-52d4c7f8ad00/sharpen=1)

```javascript
// Return the minimum value node of the BST.
function minValueNode(root) {
    let curr = root;
    while(curr != null && curr.left != null) {
        curr = curr.left;
    }
    return curr;
}

// Remove a node and return the root of the BST.
function remove(root, val) {
    if (root == null) {
        return null;
    }
    if (val > root.val) {
        root.right = remove(root.right, val);
    } else if (val < root.val) {
        root.left = remove(root.left, val);
    } else {
        if (root.left == null) {
            return root.right;
        } else if (root.right == null) {
            return root.left;
        } else {
            let minNode = minValueNode(root.right);
            root.val = minNode.val;
            root.right = remove(root.right, minNode.val);
        }
    }
    return root;
} 
```

---

## Time Complexity

If the given tree is a balanced binary tree, the height will be in O(log n), for reasons that are very similar to what we discussed in merge sort. However, it is a possibility that in the worst case, the tree provided is either left-skewed or right-skewed. In that case, the height of the tree will be in O(n) and the total work done for all the operations described above is O(n).