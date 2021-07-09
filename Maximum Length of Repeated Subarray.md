# Maximum Length of Repeated Subarray

Given two integer arrays  `nums1`  and  `nums2`, return  _the maximum length of a subarray that appears in  **both**  arrays_.

**Example 1:**

Input: nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
Output: 3
Explanation: The repeated subarray with maximum length is [3,2,1].

**Example 2:**

Input: nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
Output: 5

**Constraints:**

-   `1 <= nums1.length, nums2.length <= 1000`
-   `0 <= nums1[i], nums2[i] <= 100`

**Solution:**

```go
func findLength(nums1 []int, nums2 []int) int {
    m,n := len(nums1),len(nums2)
    dp := make([][]int, m+1)
    for i := 0; i <= m; i++ {
        dp[i] = make([]int, n+1)
    }

    var ans int
    for i := m-1; i >= 0; i-- {
        for j := n-1; j >= 0; j-- {
            if nums1[i]  == nums2[j] {
                dp[i][j] = dp[i+1][j+1] + 1
            }
            if dp[i][j] > ans {
                ans = dp[i][j]
            }
        }
    }
    
    return ans
}
```