# Is Graph Bipartite?

There is an  **undirected**  graph with  `n`  nodes, where each node is numbered between  `0`  and  `n - 1`. You are given a 2D array  `graph`, where  `graph[u]`  is an array of nodes that node  `u`  is adjacent to. More formally, for each  `v`  in  `graph[u]`, there is an undirected edge between node  `u`  and node  `v`. The graph has the following properties:

-   There are no self-edges (`graph[u]`  does not contain  `u`).
-   There are no parallel edges (`graph[u]`  does not contain duplicate values).
-   If  `v`  is in  `graph[u]`, then  `u`  is in  `graph[v]`  (the graph is undirected).
-   The graph may not be connected, meaning there may be two nodes  `u`  and  `v`  such that there is no path between them.

A graph is  **bipartite**  if the nodes can be partitioned into two independent sets  `A`  and  `B`  such that  **every**  edge in the graph connects a node in set  `A`  and a node in set  `B`.

Return  `true` _if and only if it is  **bipartite**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg)

	Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
	Output: false
	Explanation: There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg)

	Input: graph = [[1,3],[0,2],[1,3],[0,2]]
	Output: true
	Explanation: We can partition the nodes into two sets: {0, 2} and {1, 3}.

**Constraints:**

-   `graph.length == n`
-   `1 <= n <= 100`
-   `0 <= graph[u].length < n`
-   `0 <= graph[u][i] <= n - 1`
-   `graph[u]` does not contain `u`.
-   All the values of  `graph[u]`  are  **unique**.
-   If  `graph[u]`  contains  `v`, then  `graph[v]`  contains  `u`.

**Solution:**

This problem asks if there are 2 disjoint sets A,B such that 
each vertex in set A connects with all vertexes in set B and vice versa,
but vertex in set A does not connect to one another and
vertex in set B does not connect to one another

If we label all vertexes in set A with a value, e.g. "1", 
and all vertexes in set B with a different value, e.g. "-1",
then we can start the search at any vertex such that: We label that vertex "1", 
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