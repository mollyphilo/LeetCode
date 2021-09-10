# Critical Connections in a Network

There are `n`  servers numbered from `0` to `n-1`  connected by undirected server-to-server  `connections`  forming a network where  `connections[i] = [a, b]` represents a connection between servers  `a` and  `b`. Any server can reach any other server directly or indirectly through the network.

A  _critical connection_ is a connection that, if removed, will make some server unable to reach some other server.

Return all critical connections in the network in any order.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png)**

	Input: n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
	Output: [[1,3]]
	Explanation: [[3,1]] is also accepted.

**Constraints:**

-   `1 <= n <= 10^5`
-   `n-1 <= connections.length <= 10^5`
-   `connections[i][0] != connections[i][1]`
-   There are no repeated connections.

**Solution:**


Find bridges between Strongly Connected Components

Solution: Tarjan's algorithm

1. Start at any node and do DFS traversal. Label node with increasing 'id' as we go
2. Keep track of the 'id' of each node and assign smallest id (low link) from its neighbor (if there is smaller id than the current node)
3. During DFS, if we see that current node's id is less than lowlink of its neighbor, then the connection between the current node and this neighbor is the bridge of the current SCC and another SCC

```go
const Unvisited int = 0
var id int

func criticalConnections(n int, connections [][]int) [][]int {
    // graph contains adjacent nodes
    graph := make(map[int][]int)
    for _,c := range connections {
        graph[c[0]] = append(graph[c[0]], c[1])
        graph[c[1]] = append(graph[c[1]], c[0])
    }

    id = 0
    // ids in which dfs examines v
    // fill all of them with 0s
    ids := make([]int,n)
    // lowest order of any vertex connected to v
    // fill all of them with 0s
    lowlinks := make([]int,n)
    // bridges between SCCs or Strongly Connected Components
    var bridges [][]int
    
    // Since all vertexes in undirected graph are connected, we can start at any vertex
    // we choose to start at 0
    dfs(0,-1, ids,lowlinks,graph,&bridges)
    
    return bridges
}

func dfs(node,prevNode int, ids,lowlinks []int, graph map[int][]int, bridges *[][]int) {
    id += 1
    ids[node],lowlinks[node] = id,id

    for _,neighbor := range graph[node] {
        if neighbor == prevNode { continue }
        if ids[neighbor] == Unvisited {
            dfs(neighbor,node,ids,lowlinks,graph,bridges)
            lowlinks[node] = min(lowlinks[node],lowlinks[neighbor])
            if ids[node] < lowlinks[neighbor] {
                *bridges = append(*bridges, []int{node,neighbor})
            }
        } else {
            // revisit a node, which may have lower id than the current lowest link
            lowlinks[node] = min(lowlinks[node],ids[neighbor])
        }
    }
}

func min(a,b int) int {
    if a < b { return a }
    return b
}
```

Simpler implemtation:

```go
// In other word, find the edges that connect Strongly Connected Component (SCC)
// We will use Tarjan's algorithm to discover SCC
// If our current node reaches a node belonged to different SCC, a.k.a dfs_num[node] < dfs_low[current] or node 
// was visited before the SCC containing current node, then this edge is a bridge between SCC
func criticalConnections(n int, connections [][]int) [][]int {
    // generate adjacent lists from connections
    adjList := make([][]int, n)
    for _,con := range connections {
        adjList[con[0]] = append(adjList[con[0]], con[1])
        adjList[con[1]] = append(adjList[con[1]], con[0])
    }
    // dfs_nums stores the node's visited order for tarjan's algorithm. We use pointer of integer here
    // to easily check if an item has been set, which determines whether or not has it visited
    dfs_num := make([]*int, n)
    var bridges [][]int // our answer
    
    // call dfs for a node
    dfs(0,-1,0, adjList, dfs_num, &bridges)
    // return bridges
    return bridges
}

func dfs(cur, prev, depth int, adjList [][]int, dfs_num []*int,bridges *[][]int) {
    dfs_num[cur] = &depth
    
    for _,n := range adjList[cur] {
        // don't visit the node where we came from
        if n == prev {
            continue
        }
        
        if dfs_num[n] == nil {
            dfs(n,cur,depth+1,adjList,dfs_num,bridges)
        }
        
        // n is the starting node of this SCC
        if *dfs_num[cur] > *dfs_num[n] {
            dfs_num[cur] = dfs_num[n]
        }
        if *dfs_num[n] > depth {
            *bridges = append(*bridges,[]int{cur,n})
        }
    }
}
```

Time Complexity: O(V + E)

Space Complexity O(V)