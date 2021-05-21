# House Robber II

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are  **arranged in a circle.**  That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array  `nums`  representing the amount of money of each house, return  _the maximum amount of money you can rob tonight  **without alerting the police**_.

**Example 1:**

	Input: nums = [2,3,2]
	Output: 3
	Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

**Example 2:**

	Input: nums = [1,2,3,1]
	Output: 4
	Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
	Total amount you can rob = 1 + 3 = 4.

**Example 3:**

	Input: nums = [0]
	Output: 0

**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 1000`

**Solution:**

Same technique as in House Robber, but this time there are 2 options: rob with choices from 0 to n-2 or rob with choices from 1 to n-1, because houses are arranged in a circle, so first house is the neighbor of the last.

```go
func rob(nums []int) int {
    n := len(nums)
    if n == 0 { return 0 }
    if n == 1 { return nums[0] }
    robFirst := simpleRob(nums,0,n-2)
    robSecond := simpleRob(nums,1,n-1)
    if robFirst > robSecond {
        return robFirst
    }
    return robSecond
}

func simpleRob(nums []int,start,end int) int {
    var prev,curr,temp int
    for i := start; i <= end; i++ {
        temp = curr
        if v := prev + nums[i]; v > curr {
            curr = v
        }
        prev = temp
    }
    
    return curr
}
```