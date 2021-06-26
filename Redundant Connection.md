# Redundant Connection

In this problem, a tree is an  **undirected graph**  that is connected and has no cycles.

You are given a graph that started as a tree with  `n`  nodes labeled from  `1`  to  `n`, with one additional edge added. The added edge has two  **different**  vertices chosen from  `1`  to  `n`, and was not an edge that already existed. The graph is represented as an array  `edges`  of length  `n`  where  `edges[i] = [ai, bi]`  indicates that there is an edge between nodes  `ai`  and  `bi`  in the graph.

Return  _an edge that can be removed so that the resulting graph is a tree of_ `n` _nodes_. If there are multiple answers, return the answer that occurs last in the input.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg)

	Input: edges = [[1,2],[1,3],[2,3]]
	Output: [2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)

	Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
	Output: [1,4]

**Constraints:**

-   `n == edges.length`
-   `3 <= n <= 1000`
-   `edges[i].length == 2`
-   `1 <= ai  < bi  <= edges.length`
-   `ai  != bi`
-   There are no repeated edges.
-   The given graph is connected.

**Solution:**

DFS - time complexity O(N^2), space complexity O(N)
```go
func findRedundantConnection(edges [][]int) []int {
    graph := make(map[int][]int)
    var visited map[int]struct{}
    for _,e := range edges {
        visited = make(map[int]struct{})
        if len(graph[e[0]]) > 0 && len(graph[e[1]]) > 0 && dfs(e[0],e[1],graph,visited) { return e }
        graph[e[0]] = append(graph[e[0]], e[1])
        graph[e[1]] = append(graph[e[1]], e[0])
    }
    
    return []int{}
}

func dfs(source,target int, graph map[int][]int, visited map[int]struct{}) bool {
    if _,found := visited[source]; found { return false }
    visited[source] = struct{}{}
    if source == target { return true }
    neighbors := graph[source]
    for _,neighbor := range neighbors {
        if dfs(neighbor,target,graph,visited) { return true }
    }
    return false
}
```