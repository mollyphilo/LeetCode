# Jump Game II

Given an array of non-negative integers  `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

**Example 1:**

    Input: nums = [2,3,1,1,4]
    Output: 2
    Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

    Input: nums = [2,3,0,1,4]
    Output: 2

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `0 <= nums[i] <= 105`

**Solution:**

```go
func jump(nums []int) int {
    n := len(nums)
    if n <= 1 { return 0 }
    if n == 2 { return 1 }
    dp := make([]int,n)
    dp[0],dp[1] = 0,1
    for i := 2; i < n; i++ {
        dp[i] = n
    }
    for i := 0; i < n; i++ {
        for j := 1; j <= nums[i] && i+j < n; j++ {
            if dp[i] + 1 < dp[i+j] {
                dp[i+j] = dp[i] + 1
            }
        }
    }
    return dp[n-1]
}
```