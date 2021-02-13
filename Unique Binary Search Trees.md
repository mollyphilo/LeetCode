# Unique Binary Search Trees

Given an integer  `n`, return  _the number of structurally unique  **BST'**s (binary search trees) which has exactly_ `n` _nodes of unique values from_  `1`  _to_  `n`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

**Input:** n = 3
**Output:** 5

**Example 2:**

**Input:** n = 1
**Output:** 1

**Constraints:**

-   `1 <= n <= 19`

**Solution**

```go
func numTrees(n int) int {
    if n <= 1 { return n }

    dp := make([]int,n+1)
    // consider root=nil is a valid BST, then we have
    dp[0] = 1
    dp[1] = 1
    // if i is the root, number of trees of size is the number of possible trees on the left multiples by the number of possible trees on the right. 
    // The former is dp[i-1], because left trees have nodes that are less than i
    // The latter is dp[n-i], because right trees have nodes that are greater than i
    // Therefore: dp[i] = dp[(i-1)]*dp[(n-i)]
    // numTrees(n) = sum of dp[i] for i=1..n
    for i := 2; i <= n; i++ {
        for j := 0; j < i; j++ {
            dp[i] += dp[j]*dp[i-j-1]
        }
    }
    return dp[n]
}
```

Time complexity: O(N^2)

Space complexity: O(N)