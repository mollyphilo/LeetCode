# Partition Equal Subset Sum
Given a  **non-empty**  array  `nums`  containing  **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Example 1:**

**Input:** nums = [1,5,11,5]
**Output:** true
**Explanation:** The array can be partitioned as [1, 5, 5] and [11].

**Example 2:**

**Input:** nums = [1,2,3,5]
**Output:** false
**Explanation:** The array cannot be partitioned into equal sum subsets.

**Constraints:**

-   `1 <= nums.length <= 200`
-   `1 <= nums[i] <= 100`

**Solution:**

Turn problem into: Is there a subset that can sum to target (sum/2)?

```go
func canPartition(nums []int) bool {
    sum,n := 0,len(nums)
    if n == 0 { return true }
    for i := 0; i < n; i++ {
        sum += nums[i]
    }
    if sum % 2 == 1 {
        return false
    }
    target := sum/2
    dp := make([]bool, target+1)
    dp[0] = true
    for i := 1; i <= n; i++ {
        for j := target; j >= 0; j-- {
            if (j - nums[i-1]) >= 0 {
                dp[j] = dp[j - nums[i-1]] || dp[j]
            }
        }
    }
    return dp[target]
}
```

Time Complexity: O(N^2)

Space complexity: O(N)