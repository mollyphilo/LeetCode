# 4Sum
Given an array  `nums`  of  `n`  integers, return  _an array of all the  **unique**  quadruplets_  `[nums[a], nums[b], nums[c], nums[d]]`  such that:

-   `0 <= a, b, c, d < n`
-   `a`,  `b`,  `c`, and  `d`  are  **distinct**.
-   `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in  **any order**.

**Example 1:**

	Input: nums = [1,0,-1,0,-2,2], target = 0
	Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]

**Example 2:**

	Input: nums = [2,2,2,2,2], target = 8
	Output: [[2,2,2,2]]

**Constraints:**

-   `1 <= nums.length <= 200`
-   `-109  <= nums[i] <= 109`
-   `-109  <= target <= 109`

**Solution:**

```go
import (
    "sort"
    "strings"
    "strconv"
)

func fourSum(nums []int, target int) [][]int {
    if len(nums) == 0 { return [][]int{} }
    
    sort.Ints(nums)
    ans := make(map[string]struct{})
    backtrack(nums, []int{}, ans, 0, 0, target)
    
    var res [][]int
    for v := range ans {
        temp := strings.Split(v, ",")
        cur := make([]int,4)
        for i := 0; i < 4; i++ { cur[i],_ = strconv.Atoi(temp[i]) }
        res = append(res, cur)
    }
    return res
}

func backtrack(nums []int, cur []int, ans map[string]struct{}, pos, sum, target int) {
    if len(cur) == 4 { 
        if sum != target { return }
        ans[fmt.Sprintf("%d,%d,%d,%d", cur[0],cur[1],cur[2],cur[3])] = struct{}{}
        return 
    }
    
    for i := pos; i < len(nums); i++ {
        cur = append(cur, nums[i])
        backtrack(nums, cur, ans, i+1, sum+nums[i], target)
        cur = cur[0:len(cur)-1]
    }
}
```
