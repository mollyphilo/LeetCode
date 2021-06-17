# Matchsticks to Square

You are given an integer array  `matchsticks`  where  `matchsticks[i]`  is the length of the  `ith`  matchstick. You want to use  **all the matchsticks**  to make one square. You  **should not break**  any stick, but you can link them up, and each matchstick must be used  **exactly one time**.

Return  `true`  if you can make this square and  `false`  otherwise.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/matchsticks1-grid.jpg)

Input: matchsticks = [1,1,2,2,2]
Output: true
Explanation: You can form a square with length 2, one side of the square came two sticks with length 1.

**Example 2:**

Input: matchsticks = [3,3,3,3,4]
Output: false
Explanation: You cannot find a way to form a square with all the matchsticks.

**Constraints:**

-   `1 <= matchsticks.length <= 15`
-   `1 <= matchsticks[i] <= 108`

**Solution:**

```go
import "sort"

func makesquare(matchsticks []int) bool {
    n := len(matchsticks)
    if n < 4 { return false }
    var sum int
    for i := 0; i < n; i++ {
        sum += matchsticks[i]
    }
    
    if sum % 4 != 0 { return false }
    sort.Ints(matchsticks)
    
    // starting from the last index, we try to add stick to one side at a time
    return backtrack(matchsticks, [4]int{}, n-1, sum/4)
}

func backtrack(sticks []int, sides [4]int, index, length int) bool {
    // went through the end of the sticks array
    if index < 0 { return true }
    for i := 0; i < 4; i++ {
	    // we can have 2nd condition because sides[i-1] also finishes before sides[i]
	    // if they are equal which mean we processed these sides
        if sides[i] + sticks[index] > length || ( i > 0 && sides[i] == sides[i-1] ){ continue }
        sides[i] += sticks[index]
        if backtrack(sticks, sides, index-1,length) { return true }
        sides[i] -= sticks[index]
    }
    
    return false
}
```