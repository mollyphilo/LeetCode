# Jump Game VI

You are given a  **0-indexed**  integer array  `nums`  and an integer  `k`.

You are initially standing at index  `0`. In one move, you can jump at most  `k`  steps forward without going outside the boundaries of the array. That is, you can jump from index  `i`  to any index in the range  `[i + 1, min(n - 1, i + k)]`  **inclusive**.

You want to reach the last index of the array (index  `n - 1`). Your  **score**  is the  **sum**  of all  `nums[j]`  for each index  `j`  you visited in the array.

Return  _the  **maximum score**  you can get_.

**Example 1:**

	Input: nums = [1,-1,-2,4,-7,3], k = 2
	Output: 7
	Explanation: You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.

**Example 2:**

	Input: nums = [10,-5,-2,4,0,3], k = 3
	Output: 17
	Explanation: You can choose your jumps forming the subsequence [10,4,3] (underlined above). The sum is 17.

**Example 3:**

	Input: nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
	Output: 0

**Constraints:**

-   `1 <= nums.length, k <= 105`
-   `-104  <= nums[i] <= 104`

**Solution:**

```go
// monoqueue of indexes such that their values are strictly decreasing
func maxResult(nums []int, k int) int {
    n := len(nums)
    var monoqueue []int  // contains indices
    var cur int
    for i := n-1; i >= 0; i-- {
        cur = nums[i]
        if len(monoqueue) > 0 {
            cur += nums[monoqueue[0]]
        }
        // remove all values less than current
        for len(monoqueue) > 0 && cur > nums[monoqueue[len(monoqueue)-1]] {
            monoqueue = monoqueue[0:len(monoqueue)-1]
        }
        monoqueue = append(monoqueue,i)
        // remove all values k steps from i
        for len(monoqueue) > 0 && monoqueue[0] >= i+k {
            monoqueue = monoqueue[1:]
        }
        nums[i] = cur
    }
    return cur
}
```