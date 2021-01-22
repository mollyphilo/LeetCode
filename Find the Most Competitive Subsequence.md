# Find the Most Competitive Subsequence
Given an integer array  `nums`  and a positive integer  `k`, return  _the most **competitive**  subsequence of_ `nums`  _of size_ `k`.

An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.

We define that a subsequence  `a`  is more  **competitive**  than a subsequence  `b`  (of the same length) if in the first position where  `a`  and  `b`  differ, subsequence  `a`  has a number  **less**  than the corresponding number in  `b`. For example,  `[1,3,4]`  is more competitive than  `[1,3,5]`  because the first position they differ is at the final number, and  `4`  is less than  `5`.

**Example 1:**

	Input: nums = [3,5,2,6], k = 2
	Output: [2,6]
	Explanation: Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.

**Example 2:**

	Input: nums = [2,4,3,3,5,4,9,6], k = 4
	Output: [2,3,3,4]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 109`
-   `1 <= k <= nums.length`

**Solutions:**
```go
func mostCompetitive(nums []int, k int) []int {
    n := len(nums)
    stack := []int{}
    // max number of times you can pop out of the stack so that the final size of stack is still k
    attempts := n-k
    for i := 0; i < n; i++ {
        for len(stack) > 0 && stack[len(stack)-1] > nums[i] && attempts > 0 {
            stack = stack[:(len(stack)-1)]
            attempts--
        }
        stack = append(stack,nums[i])
    }
    return stack[:k]
}
```
- Time complexity: O(N*K) 
- Space complexity: O(K) for the stack