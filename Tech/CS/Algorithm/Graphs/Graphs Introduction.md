---
tags: Algorithm, Generic
---
## Graph Terminology

In graphs, nodes are referred to as **vertices** and the pointers connecting these nodes are referred to as **edges**. There are no restrictions in graphs when it comes to where the nodes are placed, and how the edges connect those nodes together.

It is also possible that the nodes are not connected by any edges and this would still be considered a graph - a null graph.

The number of edges, E, given the number of vertices V will always be less than or equal to V2. 2E<=V2. This is because each node can at most point to itself, and every other node in the graph. If we have a node `A`, `B` and `C`, `A` can point to itself, `B` and `C`. The same goes for `B` and `C`, so the rule checks out.

If the pointers connecting the edges together have a direction, we call this a **directed graph.** If there are edges but no direction, this is referred to as an **undirected graph**. For example, trees and linked lists are directed graphs because we had pointers like `prev`, `next` and `left_child`, `right_child`.

![hello](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/74daad67-5e27-476e-af6b-291ab5b80400/sharpen=1)

---

## Formats of Graphs in Interviews

A graph can be represented in different ways. It is an abstract concept that is made concrete using different data structures. Most commonly, graphs are represented using the following:

1. Matrix
2. Adjacency Matrix
3. Adjacency List

### Matrix

A matrix is a two-dimensional array with rows and columns, and a graph can be represented using a matrix. In the code below, each array, separated by commas, represents each row. Here we have four rows and four columns. Everything is indexed by 00 and the idea is that we should be able to access an arbitrary row, column, or a specific element given a specified row or column. Accessing the second row can simply be done by `grid[1]` and accessing the second column may be done by `grid[0][1]`.

```javascript
// Matrix (2D Grid)
let grid = [[0, 0, 0, 0],
            [1, 1, 0, 0],
            [0, 0, 0, 1],
            [0, 1, 0, 0]];
```

How can this be used to represent a graph? As we mentioned, graphs are abstract and can be defined in many ways. Let's say that all of the 0's in our grid are vertices. To traverse a graph, we are allowed to move up, down, left and right. If we are to connect the 0s together, using our edges, we would end up getting a bunch of connected zeroes, which are connected components, and that denotes a graph. We shall discuss matrix traversal in the next chapter.

![hello](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/f3c04f37-7656-4836-f263-3ae19258c100/sharpen=1)

---

### Adjacency Matrix

This is the less common input format. Here, the index of the array represents a vertex itself. Let's take an example:

```javascript
// Adjacency matrix
let adjMatrix = [[0, 0, 0, 0],
                 [1, 1, 0, 0],
                 [0, 0, 0, 1],
                 [0, 1, 0, 0]];
```

Because the indices themselves represent a vertex, 00 denotes that an edge does not exist between a given `v1, v2`, and 11 denotes the opposite. `adjMatrix[1][2] == 0` means there is no edge between vertex `1` and `2`. Also, `adjMatrix[2][1] == 0` means there is no edge between vertex `2` and `1`. We can conclude the following from this:

```javascript
// an edge exists from v1 to v2
adjMatrix[v1][v2] = 1;

// an edge exists from v2 to v1
adjMatrix[v2][v1] = 1;

// No edge exists from v1 to v2
adjMatrix[v1][v2] = 0;

// No edge exists from v2 to v1
adjMatrix[v2][v1] = 0;
```

To actualize the above adjacency matrix, we can look at the following visual.

![hello](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/02f2aedd-88d5-461d-9fdd-0bffc2e81400/sharpen=1)

The issue with this is that it is a giant memory hog. Because it is a square matrix, the time complexity is O(V2), where V is the number of vertices. This makes sense since the number of edges is also equal to the number of nodes.

---

### Adjacency List

These are typically the most common way of representing graphs. This is also a two dimensional array. The convenience here is that we can declare it using a class called `GraphNode` and it contains a list attribute called `neighbors`, using which we can access all of a given vertex's neighbor.

```javascript
class GraphNode {
    constructor(val) {
        this.val = val;
        this.neighbors = new Array();
    }
}
```

This ends up being more space efficient because we only care about nodes that are connected.

![hello](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/85b20b68-357a-4a6d-7c7a-e09256693200/sharpen=1)