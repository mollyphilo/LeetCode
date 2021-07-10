# Longest Increasing Subsequence

Given an integer array  `nums`, return the length of the longest strictly increasing subsequence.

A  **subsequence**  is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example,  `[3,6,2,7]`  is a subsequence of the array  `[0,3,1,6,2,2,7]`.

**Example 1:**

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**

Input: nums = [0,1,0,3,2,3]
Output: 4

**Example 3:**

Input: nums = [7,7,7,7,7,7,7]
Output: 1

**Constraints:**

-   `1 <= nums.length <= 2500`
-   `-104  <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))`  time complexity?

**Solution:**

### Dynamic Progrogramming

```go
func lengthOfLIS(nums []int) int {
    n := len(nums)
    if n == 1 { return 1 }
    var max int 
    dp := make([]int, n)
    for i := 0; i < n; i++ { dp[i] = 1}
    
    for i := 0; i < n; i++ {
        for j := i+1; j < n; j++ {
            if nums[j] > nums[i] && dp[i]+1 > dp[j] {
                dp[j] = dp[i]+1
            }
            if dp[j] > max {
                max = dp[j]
            }
        }
    }
    return max
}
```

### Binary Index Tree

```go
/*
Binary index tree
- Order numbers by their sorted indexes so that we can find 
- bis[i] = max(bits[j]) + 1 for j=0->i
*/
import "sort"

func lengthOfLIS(nums []int) int {
    n := len(nums)
    bits := make([]int, n+1) // 1-based
    indexes := make(map[int]int) // map numbers & their sorted indexes. sorted indexes in used on bits
    // save spaces by using bits array for sort indexes
    copy(bits[1:n+1], nums)
    sort.Ints(bits[1:n+1])
    for i := 1; i <= n; i++ {
        indexes[bits[i]] = i
        bits[i] = 0
    }
    var ans int
    for _,num := range nums {
        // find the length of longest substring up to i - 1
        temp := longest_sub(bits, indexes[num]-1)
        // compare with current max and update values off all bits from i onwards
        ans = max(ans,temp+1)
        update(bits, indexes[num], temp+1)
    }
    return ans
}

func update(bits []int, i,val int) {
    for i < len(bits) {
        bits[i] = max(bits[i], val)
        i += i & -i
    }
}

// find the length of longest substring from 0->i
func longest_sub(bits []int, i int) int {
    var ans int
    for i > 0 {
        ans = max(bits[i],ans)
        i -= i & -i
    }
    return ans
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```