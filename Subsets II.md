# Subsets II

Given an integer array  `nums`  that may contain duplicates, return  _all possible subsets (the power set)_.

The solution set  **must not**  contain duplicate subsets. Return the solution in  **any order**.

**Example 1:**

	Input: nums = [1,2,2]
	Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]

**Example 2:**

	Input: nums = [0]
	Output: [[],[0]]

**Constraints:**

-   `1 <= nums.length <= 10`
-   `-10 <= nums[i] <= 10`

**Solution:**

```go
import "sort"

func subsetsWithDup(nums []int) [][]int {
    sort.Ints(nums) // sort to have same value number stands consecutively
    var res [][]int
    backtrack(nums, &res, []int{}, 0)
    return res
}

func backtrack(nums []int, res *[][]int, cur []int, pos int) {
    ans := make([]int, len(cur))
    copy(ans,cur)
    *res = append(*res, ans)
    if pos >= len(nums) { return }
    for i := pos; i < len(nums); i++ {
        if i > pos && nums[i] == nums[i-1] { continue }
        backtrack(nums, res, append(cur, nums[i]), i+1)
    }
}
```