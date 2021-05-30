# Permutations

Given an array  `nums`  of distinct integers, return  _all the possible permutations_. You can return the answer in  **any order**.

**Example 1:**

	Input: nums = [1,2,3]
	Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Example 2:**

	Input: nums = [0,1]
	Output: [[0,1],[1,0]]

**Example 3:**

	Input: nums = [1]
	Output: [[1]]

**Constraints:**

-   `1 <= nums.length <= 6`
-   `-10 <= nums[i] <= 10`
-   All the integers of  `nums`  are  **unique**.

**Solution:**

```go
func permute(nums []int) [][]int {
    var res [][]int
    backtracking(nums,&res,0)
    return res
}

func backtracking(nums []int, res *[][]int, k int) {
    if k >= len(nums) {
        temp := make([]int,len(nums))
        copy(temp,nums)
        *res = append(*res,temp)
        return
    }
    for i := k; i < len(nums); i++ {
        nums[i],nums[k] = nums[k],nums[i]
        backtracking(nums,res,k + 1)
        nums[i],nums[k] = nums[k],nums[i]
    }
}
```