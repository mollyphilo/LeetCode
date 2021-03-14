# Partition Array for Maximum Sum

Given an integer array  `arr`, you should partition the array into (contiguous) subarrays of length at most  `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return  _the largest sum of the given array after partitioning._

**Example 1:**

	Input: arr = [1,15,7,9,2,5,10], k = 3
	Output: 84
	Explanation: arr becomes [15,15,15,9,10,10,10]

**Example 2:**

	Input: arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
	Output: 83

**Example 3:**

	Input: arr = [1], k = 1
	Output: 1

**Constraints:**

-   `1 <= arr.length <= 500`
-   `0 <= arr[i] <= 109`
-   `1 <= k <= arr.length`

**Solution:**

```go
func maxSumAfterPartitioning(arr []int, k int) int {
    n := len(arr)
    dp := make([]int, n+1)
    for i := 1; i <= n; i++ {
        for j := 1; j <= k && j <= i; j++ {
            dp[i] = max(dp[i], dp[i-j] +  max(arr[i-j:i]...)*j)
        }
    }
    return dp[n]
}

func max(val ...int) int {
    m := -1
    for _,v := range val {
        if v > m {
            m = v
        }
    }
    return m
}
```