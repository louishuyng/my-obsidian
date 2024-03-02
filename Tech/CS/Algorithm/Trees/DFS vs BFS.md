---
tags: Algorithm, Generic
---

# Depth-First Search

Depth First Search (DFS) is a way of traversing binary search trees that prioritizes depth rather than breadth.

The idea is to keep traversing down either the left subtree or the right subtree until there are no more nodes left. There are various methods under which depth-first search is performed. These methods visit the nodes - `root, left_child, right_child` in different orders. These three methods are:

- Inorder
- Preorder
- Postorder

Depth first search is best implemented using recursion. Again, you could use a stack and do it iteratively but it is a lot more complicated.

Take a tree with the nodes `[4,3,6,2,null,5,7]` going from left to right.


## Inorder Traversal

An inorder traversal prioritizing left before right will visit the `left-child` first, then the parent node and then the `right-child`. Inorder traversal will result in the nodes being visited in a sorted order.

```javascript
function inorder(root) {
    if (root == null) {
        return;
    }
    inorder(root.left);
    console.log(root.val);
    inorder(root.right);
}
```

The order in which these nodes will be visited is `[2,3,4,5,6,7]`, which is sorted. It is important to note that this only works when left is prioritized before right. If we prioritize right before left, we will end up with a reverse sorted array.

> The reason the nodes will print in a sorted order is because of the BST property. Since we know all values to the left of a node are smaller, this means we won't hit our base case until we reach the left-most node which is also the smallest node. After visiting this, we will traverse up, visit the parent and then visit the right-subtree. The visual below shows this process.

The order in which the nodes are visited is represented by the numbers in blue next to the node.

![In-order Traversal](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/e8717d2e-69c7-4ec2-ce9c-6d8753d3cc00/sharpen=1)

## Preorder Traversal

A preorder traversal prioritizing left before right will visit the parent, the left-child and the right-child, in that order.

```javascript
function preorder(root) {
    if (root == null) {
        return;
    }
    console.log(root.val);
    preorder(root.left);
    preorder(root.right);
}
```

The nodes are visited in the following order `[4,3,2,6,5,7]`

![In-order Traversal](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/9388095e-8f09-4725-fc1d-27988a291c00/sharpen=1)

## Postorder Traversal

A post-order traversal prioritizing left before right will visit the `left_child`, the `right_child` and the parent, in that order.

```javascript
function postorder(root) {
    if (root == null) {
        return;
    }  
    postorder(root.left);
    postorder(root.right);
    console.log(root.val);
}
```

The order in which these nodes will be visited is: `[2,3,5,7,6,4]`

![In-order Traversal](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/1abfa778-c56d-4563-9860-5f58bcee6c00/sharpen=1)

---

## Time Space Complexity

We know that we have to visit every node in the tree, and if there are n nodes in the tree, the algorithm will run in O(n).

Another interesting point is that we could actually sort an array by making use of a binary tree. First, we would need to build the tree and insert each value. We know that there are n values and inserting a value in the binary tree takes log n time. Therefore, the whole process of building the tree will be O(n log n). Traversing the tree will only take n steps, so in total it will be O(n+n log n).

We have mentioned before that we do not take constants into consideration. We also know that O(n log n) will grow faster than O(n), so we can set our upper bound to O(n log n).


# Breadth First Search

---

## Concept

In depth-first search, we prioritized depth. For breath-first serach, we prioritize breadth. We focus visiting all the nodes on one level before moving on to the next level.

Generally, breadth-first search is implemented iteratively and that is the implementation we will be covering in this course. You can write it recursively but it is a lot more challenging.

BFS makes use of a queue data structure, more specifically, a deque, allowing us to remove elements both from the head and the tail in O(1) time. This will make sense soon.

The pseudocode for BFS is shown below.

```javascript
function bfs(root) { 
    let queue = [];
    if (root != null) {
        queue.push(root);
    }    
    let level = 0;
    while(queue.length > 0) {
        console.log("level " + level + ": ");
        let levelLength = queue.length;
        for (let i = 0; i < levelLength; i++) {
            let curr = queue.shift(); 
            console.log(curr.val + " ");
            if(curr.left != null) {
                queue.push(curr.left);  
            }
            if(curr.right != null) {
                queue.push(curr.right);
            }  
        }
        level++;
        console.log();
    }
}
```


Let's take an example of a binary tree, `[4,3,6,2,null,5,7]` and apply the BFS algorithm. Remember, our goal is to visit all the nodes on one level before moving to the next.

If we append our `root` node to our queue and loop through the queue such that at any given time, our queue only holds the nodes on a certain level, we will ensure that we visit the levels in order and that we don't mix up the levels either. This is exactly what the code inside of the while loop achieves. As long as our queue is not empty, we remove the node(s) that is present in our queue and add its children to the queue (which would be the next level). Therefore, when we remove `root`, we add its children, which are `3,6` to the queue. Next, we remove `3` and add its child `2`. We then remove `6` and add `5,7` to the queue. Because of the FIFO nature of a queue, we ensure that we visit the nodes from left to right.

Our queue becomes empty once we have visited all of the nodes.

The visual below demonstrates what the state of the queue at every level of the tree would look like.

![bfs](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/ba15b069-dd3c-4224-41c1-d9accf16d700/sharpen=1)

---

## Time Complexity

Technically, the total work done is c∗n where n is the number of nodes in the tree and c is the amount of work we perform at each node. We performed a total of three operations per node - printing the node, appending the node, and removing it. This is what the c represents. For the case of asymptotic analysis, we can drop this constant, meaning the algorithm belongs to O(n)**.