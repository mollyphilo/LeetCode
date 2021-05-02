# Course Schedule II


There are a total of  `n`  courses you have to take labelled from  `0`  to  `n - 1`.

Some courses may have  `prerequisites`, for example, if `prerequisites[i] = [ai, bi]` this means you must take the course  `bi`  before the course  `ai`.

Given the total number of courses `numCourses`  and a list of the  `prerequisite`  pairs, return the ordering of courses you should take to finish all courses.

If there are many valid answers, return  **any**  of them. If it is impossible to finish all courses, return  **an empty array**.

**Example 1:**

    Input: numCourses = 2, prerequisites = [[1,0]]
    Output: [0,1]
    Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

**Example 2:**

    Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
    Output: [0,2,1,3]
    Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
    So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

**Example 3:**

    Input: numCourses = 1, prerequisites = []
    Output: [0]

**Constraints:**

-   `1 <= numCourses <= 2000`
-   `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
-   `prerequisites[i].length == 2`
-   `0 <= ai, bi  < numCourses`
-   `ai != bi`
-   All the pairs  `[ai, bi]`  are  **distinct**

**Solutions:**

This problem asks for a topological sort order. Recall that topolocal sorting for Directed Acylic Graph (DAG) is a linear ordering of vertices such that for every directed edge [u v], vertex u comes before v in the ordering. Topological sorting is not possible with cyclic graph. 
First solution is DFS with visits list to keep track of what node has been visited during the current iteration from the start node. 

```python
class Solution(object):
    
    def findOrder(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: List[int]
        """
        self.is_cyclic = False
        self.unvisited = 0
        self.visiting = 1
        self.visited = 2
        
        topolocal_sorted_order = []
        visits = [0]*numCourses
        adj_nodes = defaultdict(list)
        for dest,src in prerequisites:
            adj_nodes[src].append(dest)
        
        def dfs(node):
            if self.is_cyclic:
                return
            
            visits[node] = self.visiting
            
            for neighbor in adj_nodes[node]:
                if visits[neighbor] == self.visiting:
                    self.is_cyclic = True
                elif visits[neighbor] == self.unvisited:
                    dfs(neighbor)
                    
            visits[node] = self.visited
            topolocal_sorted_order.append(node)
            
        
        for vertex in range(numCourses):
            if visits[vertex] == self.unvisited:
                dfs(vertex)
                
        return topolocal_sorted_order[::-1] if not self.is_cyclic else []        
```
- Time Complexity: O(V+E) where V is the number of vertices and E represents the number of edges. Essentially we iterate through each vertex and each edge for once. 
- Space Complexity:  O(V+E):
	* Adjacent list to represents our edges: O(E)
	* We apply recusion in our algorithm, which in the worst case incurs O(V) exstra space in the function call stack
	* To sum up, the overall space complexity is O(V+E)

The second approach is based on the idea that node without incoming edges can start the topological order. If we process all nodes with 0 in-degree, we can then remove these nodes from the graph, along with their outgoing edges, and find out with nodes should be processed next. These would again be the nodes with 0 in-degree. We can continuously do this until all the nodes have been accounted for. 

```python
class Solution(object):
    
    def findOrder(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: List[int]
        """
        adj_nodes = defaultdict(set)
        indegrees = {}
        
        for dest,src in prerequisites:
            adj_nodes[src].add(dest)
            indegrees[dest] = indegrees.get(dest,0) + 1
            
        topological_sorted_order = []
        # use deque because its popleft() takes O(1) while list.pop() takes O(N)
        zero_indegree_queue = deque([n for n in range(numCourses) if n not in indegrees])
        
        while zero_indegree_queue:
            node = zero_indegree_queue.popleft()
            topological_sorted_order.append(node)
            for neighbor in adj_nodes[node]:
                indegrees[neighbor] -= 1
                if indegrees[neighbor] == 0:
                    zero_indegree_queue.append(neighbor)
            
        return topological_sorted_order if len(topological_sorted_order) == numCourses else []
```