# Max Consecutive Ones III
Given a binary array  `nums`  and an integer  `k`, return  _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_  `k`  `0`'s.

**Example 1:**

	Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
	Output: 6
Explanation: [1,1,1,0,0,**1**,1,1,1,1,**1**]

	Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

	Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
	Output: 10
Explanation: [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]

	Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `nums[i]`  is either  `0`  or  `1`.
-   `0 <= k <= nums.length`

**Solution:**

Translation: find the longest substring with at most K zeros

```go
func longestOnes(nums []int, k int) int {
    var zeros,start,ans int
    for i := 0; i < len(nums);i++ {
        if nums[i] == 0 { zeros++ }
        for zeros > k {
            if nums[start] == 0 { zeros-- }
            start++
        }
        if curr := i - start + 1; curr > ans {
            ans = curr
        }
    }
    
    return ans
}
```