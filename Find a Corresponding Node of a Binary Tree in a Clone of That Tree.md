# Find a Corresponding Node of a Binary Tree in a Clone of That Tree
Given two binary trees  `original`  and  `cloned`  and given a reference to a node  `target`  in the original tree.

The  `cloned`  tree is a  **copy of**  the  `original`  tree.

Return  _a reference to the same node_  in the  `cloned`  tree.

**Note**  that you are  **not allowed**  to change any of the two trees or the  `target`  node and the answer  **must be**  a reference to a node in the  `cloned`  tree.

**Follow up:** Solve the problem if repeated values on the tree are allowed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/02/21/e1.png)

**Input:** tree = [7,4,3,null,null,6,19], target = 3
**Output:** 3
**Explanation:** In all examples the original and cloned trees are shown. The target node is a green node from the original tree. The answer is the yellow node from the cloned tree.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/02/21/e2.png)

**Input:** tree = [7], target =  7
**Output:** 7

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/02/21/e3.png)

**Input:** tree = [8,null,6,null,5,null,4,null,3,null,2,null,1], target = 4
**Output:** 4

**Example 4:**

![](https://assets.leetcode.com/uploads/2020/02/21/e4.png)

**Input:** tree = [1,2,3,4,5,6,7,8,9,10], target = 5
**Output:** 5

**Example 5:**

![](https://assets.leetcode.com/uploads/2020/02/21/e5.png)

**Input:** tree = [1,2,null,3], target = 2
**Output:** 2

**Constraints:**

 -   The number of nodes in the  `tree`  is in the range  `[1, 10^4]`.
 -   The values of the nodes of the  `tree`  are unique.
 -   `target`  node is a node from the  `original`  tree and is not  `null`.
**Solutions:**
This is a Python 2 practice
The solutions are just tree traversal
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
```
**DFS Recursive inorder traveral**
```python
class Solution(object):
    def getTargetCopy(self, original, cloned, target):
        """
        :type original: TreeNode
        :type cloned: TreeNode
        :type target: TreeNode
        :rtype: TreeNode
        """
        def inorder(ori, clo):
            if ori:
                inorder(ori.left,clo.left)
                if ori == target:
                    self.ans = clo
                inorder(ori.right,clo.right)
                
        inorder(original,cloned)
        return self.ans
```
 - Time Complexity: O(N) as one has to visit each node, N = # of nodes
 - Space Complexity: O(H) to keep the recursion stack, where H is a tree
   height

**DFS Iterative inorder traversal**
Use stack to keep track of nodes one needs to visit
```python
class Solution:
	def getTargetCopy(self, original, cloned, target):
		stackO, stackC = [],[]
		nodeO, nodeC = original, cloned
		while stackO or stackC:
			while nodeO:
				stackO.append(nodeO)
				stackC.append(nodeC)
				nodeO = nodeO.left
				nodeC = nodeC.left
			
			nodeO = stackO.pop()
			nodeC = stackC.pop()

			if nodeO is target:
				return nodeC
			nodeO = nodeO.right
			nodeC = nodeC.right
```

-   Time complexity:  O(N) since one has to visit each node.
-   Space complexity: up to  O(H)  to keep the stack, where  HH  is a tree height.
**BFS Iterative traversal**
```python
class Solution:
	def getTargetCopy(self, original, cloned, target):
		queueO,queueC = [original],[cloned]
		while queueO:
			nodeO = queueO.pop(0)
			nodeC = queueC.pop(0)

			if nodeO is target:
				return nodeC
			if nodeO:
				queueO.append(nodeO.left)
				queueO.append(nodeO.right)
				queueC.append(nodeC.left)
				queueC.append(nodeC.right)
```
-   Time complexity:  \mathcal{O}(N)O(N)  since one has to visit each node.
-   Space complexity: up to  \mathcal{O}(N)O(N)  to keep the queue. Let's use the last level to estimate the queue size. This level could contain up to  N/2N/2  tree nodes in the case of  [complete binary tree](https://leetcode.com/problems/count-complete-tree-nodes/).