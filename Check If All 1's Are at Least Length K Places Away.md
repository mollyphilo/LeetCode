# Check If All 1's Are at Least Length K Places Away

Given an array  `nums`  of 0s and 1s and an integer  `k`, return  `True`  if all 1's are at least  `k`  places away from each other, otherwise return  `False`.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/04/15/sample_1_1791.png)**

	Input: nums = [1,0,0,0,1,0,0,1], k = 2
	Output: true
	Explanation: Each of the 1s are at least 2 places away from each other.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2020/04/15/sample_2_1791.png)**

	Input: nums = [1,0,0,1,0,1], k = 2
	Output: false
	Explanation: The second 1 and third 1 are only one apart from each other.

**Example 3:**

	Input: nums = [1,1,1,1,1], k = 0
	Output: true

**Example 4:**

	Input: nums = [0,1,0,1], k = 1
	Output: true

**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= k <= nums.length`
-   `nums[i]`  is  `0`  or  `1`

**Solutions:**
```go
func kLengthApart(nums []int, k int) bool {
    n := len(nums)
    var i,j int = 0,0
    for i < n {
        if nums[i] == 1 {
            j = i+1
            for j < n && nums[j] != 1 {
                j++
            }
            if j < n && nums[j] == 1 && (j-i-1) < k {
                return false
            }
            i = j
        }else {
            i++        
        }
    }
    
    return true
}
```