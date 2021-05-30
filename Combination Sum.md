# Combination Sum

Given an array of  **distinct**  integers  `candidates`  and a target integer  `target`, return  _a list of all  **unique combinations**  of_ `candidates` _where the chosen numbers sum to_ `target`_._  You may return the combinations in  **any order**.

The  **same**  number may be chosen from  `candidates`  an  **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is  **guaranteed**  that the number of unique combinations that sum up to  `target`  is less than  `150`  combinations for the given input.

**Example 1:**

	Input: candidates = [2,3,6,7], target = 7
	Output: [[2,2,3],[7]]
	Explanation:
	2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
	7 is a candidate, and 7 = 7.
	These are the only two combinations.

**Example 2:**

	Input: candidates = [2,3,5], target = 8
	Output: [[2,2,2,2],[2,3,3],[3,5]]

**Example 3:**

	Input: candidates = [2], target = 1
	Output: []

**Example 4:**

	Input: candidates = [1], target = 1
	Output: [[1]]

**Example 5:**

	Input: candidates = [1], target = 2
	Output: [[1,1]]

**Constraints:**

-   `1 <= candidates.length <= 30`
-   `1 <= candidates[i] <= 200`
-   All elements of  `candidates`  are  **distinct**.
-   `1 <= target <= 500`

**Solution:**

Backtracking

```go
func combinationSum(candidates []int, target int) [][]int {
    var res [][]int
    backtrack(candidates,target,&res,[]int{},0)
    return res
}

func backtrack(nums []int, target int, res *[][]int, comb []int,k int) {
    if target == 0 {
        temp := make([]int,len(comb))
        copy(temp,comb)
        *res = append(*res,temp)
        return
    }
    
    for i := k; i < len(nums); i++ {
        if target - nums[i] >= 0 {
            comb = append(comb,nums[i])
            backtrack(nums,target-nums[i],res,comb,i)
            comb = comb[0:len(comb)-1]
        }
    }
}
```