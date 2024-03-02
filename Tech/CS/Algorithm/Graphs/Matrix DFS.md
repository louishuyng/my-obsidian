---
tags: Algorithm, Generic
---
Suppose we are given the following question to solve:

> Q: Count the unique paths from the top left to the bottom right. A single path may only move along 0's and can't visit the same cell more than once.

```javascript
let matrix = [[0, 0, 0, 0],
            [1, 1, 0, 0],
            [0, 0, 0, 1],
            [0, 1, 0, 0]];
```


In this problem, it is all a matter of choices. You might think of this as similar to backtracking and you would be right. We have mentioned before that DFS is recursive in nature and and we will be using recursion for this. Firstly, we need to think of our base case(s). Well, we know that we can move in all four directions except diagonally. This means that if we go out of bounds, we can return zero.

We know that this will be a brute-force DFS with backtracking since at any point in our path, we might not have a valid way to reach the bottom right, in which case, we will have to backtrack.

For starters, let's establish about our base cases. Since we are trying to find the number of unique paths, we need to keep count of the valid paths from each vertex.

---

## The base cases

#### 1. A unique path does not exist

Since we are allowed to move in all four directions, it is possible that during our traversal, we end up going out of bounds. This means either our column, `c`, or our row, `r` becomes negative, or goes beyond the length of our matrix. It does not matter which of `r` and `c` goes out of bounds because we need a valid `c` **AND** a valid `r` to perform our search. We cannot perform a search on `matrix[-1][3]`.

If we have already visited a coordinate, or the current coordinate is 1, then a valid path does not exist through that coordinate.

So because a valid path does not exist in all of the aforementioned cases, we will return 0, which denotes absence of a unique path. We shall see this in our code soon.

#### 2. A unique path does exist

If we have not returned 00 from the first case, and we have reached the right-most column and the bottom-most row, it must be the case that we have found a valid path. Remember, our definition of a valid path is if a path exists from `matrix[0][0]` to `matrix[3][3]`. We can now return 11 and this will increment our count for the number of unique paths.

---

## Implementation

To ensure that we don't visit a coordinate more than once, we add it to a global HashSet, once visited.

Then, at any given coordinate, we can recursively perform our DFS on `r+1`, `r-1`, `c+1` and `c-1`. If our recursive call returns 11, our `count` will be incremented and if it returns 00, adding it to count will make no difference.

At each recursive call, we can remove all of the rows and columns that led us to an invalid path. This way, we can ensure to visit them again, but take a different direction and explore if a valid path exists from that direction.

In the code below, we have our aforementioned base cases set up. We then add the current row and column to our set. Our count is initialized with 00 because we need to keep track of all the possible unique paths we have to our destination at any given vertex. Once our recursive calls return, we can remove the visited vertices from our set. Again, this is because they can be part of another unique path, just from a different source. So, when we backtrack, we can visit them again.

```javascript
// Count paths (backtracking)
function dfs(grid, r, c, visit) {
    let ROWS = grid.length, COLS = grid[0].length;

    if (Math.min(r, c) < 0 || r == ROWS || c == COLS ||
        visit[r][c] == 1 || grid[r][c] == 1) {
        return 0;
    }
    if (r == ROWS - 1 && c == COLS - 1) {
        return 1;
    }
    visit[r][c] = 1;

    let count = 0;
    count += dfs(grid, r + 1, c, visit);
    count += dfs(grid, r - 1, c, visit);
    count += dfs(grid, r, c + 1, visit);
    count += dfs(grid, r, c - 1, visit);

    visit[r][c] = 0;
    return count;
}
```

To visualize the above on our matrix, we can break down our algorithm into finding the initial unique path, and then backtracking to find another potential unique path.

### 1. Find the first unique path

![recursive-dfs](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/7717e227-5da3-4b91-fe73-b25065d78c00/sharpen=1)

### 2. Backtrack to find another potential unique path

> The red dotted line represents another unique path which is reached from `matrix[0][3]`.

![recursive-dfs](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/c1fd94d4-7a68-4e8d-4f91-5a5f3ec92c00/sharpen=1)

Our function returns 22, denoting there exist 22 unique paths from `(0,0)` to `(3,3)`.

---

## Time Complexity

By now, we know that we only consider the worst case. In the worst case, we might need to visit every single row and column. At every coordinate, we can move in all four directions. Each one of the coordinates that are reached by moving in each those four directions will also be able to move up, down, left or right. We have four options from each position. If we are to create a decision tree out of this, each node will have at most four children. The tree has a branching factor of 44 and the height of the tree is the size of the matrix which is just n∗m, where n is the number of rows and m is the number of columns.

Therefore, we get 4nm.
