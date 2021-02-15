# Shortest Path in Binary Matrix

In an N by N square grid, each cell is either empty (0) or blocked (1).

A _clear path from top-left to bottom-right_ has length  `k`  if and only if it is composed of cells  `C_1, C_2, ..., C_k` such that:

-   Adjacent cells  `C_i`  and  `C_{i+1}`  are connected 8-directionally (ie., they are different and share an edge or corner)
-   `C_1`  is at location  `(0, 0)`  (ie. has value  `grid[0][0]`)
-   `C_k` is at location  `(N-1, N-1)`  (ie. has value  `grid[N-1][N-1]`)
-   If  `C_i`  is located at `(r, c)`, then  `grid[r][c]`  is empty (ie. `grid[r][c] == 0`).

Return the length of the shortest such clear path from top-left to bottom-right. If such a path does not exist, return -1.

**Example 1:**

**Input:** [[0,1],[1,0]]
![](https://assets.leetcode.com/uploads/2019/08/04/example1_1.png) 
**Output:** 2
![](https://assets.leetcode.com/uploads/2019/08/04/example1_2.png)

**Example 2:**

**Input:** [[0,0,0],[1,1,0],[1,1,0]]
![](https://assets.leetcode.com/uploads/2019/08/04/example2_1.png) 
**Output:** 4
![](https://assets.leetcode.com/uploads/2019/08/04/example2_2.png)

**Note:**

1.  `1 <= grid.length == grid[0].length <= 100`
2.  `grid[r][c]`  is  `0`  or  `1`

**Solution:**

This problem asks if there are 2 disjoint sets A,B such that 
each vertex in set A connects with all vertexes in set B and vice versa,
but vertex in set A does not connect to one another and
vertex in set B does not connect to one another

If we label all vertexes in set A with a value, e.g. "1", 
and all vertexes in set B with a different value, e.g. "-1",
then we can start the search at any vertex. 

We label that vertex "1", 
and all of its connected vertexes (neighbors) "-1", the neighbors of connected vertexes "1" and so on.
If at any step, a vertex has visited neighbors with the same label as itself, the graph is not bipartite

```go
func isBipartite(graph [][]int) bool {
    n := len(graph)
    if n == 0 { return false }
    
    // queue for vertexes, whose value is 0..n-1
    var queue []int
    // 3 possible values for label: 0 = unlabeled, 1 = setA, -1 = setB
    labels := make([]int,n)
    
    for i := 0; i < n; i++ {
        // no need to check the vertex that has been labeled and checked
        if labels[i] != 0 { continue }
        // new queue for all connected vertexes
        queue = []int{i}
        labels[i] = 1
        
        for len(queue) > 0 {
            u := queue[0]
            queue = queue[1:]
            
            for _,v := range graph[u] {
                // unvisited
                if labels[v] == 0 {
                    labels[v] = -labels[u]
                    queue = append(queue,v)
                }else if labels[v] == labels[u] {
                    return false
                }
            }
        }
    }
    
    return true
}
```

Time complexity: O(N^2) because we visit as most N^2 cells

Space complexity: O(N^2) for the queue, which contains at most N^2 cells