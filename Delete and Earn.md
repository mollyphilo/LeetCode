# Delete and Earn

Given an array  `nums`  of integers, you can perform operations on the array.

In each operation, you pick any  `nums[i]`  and delete it to earn  `nums[i]`  points. After, you must delete  **every**  element equal to  `nums[i] - 1`  or  `nums[i] + 1`.

You start with  `0`  points. Return the maximum number of points you can earn by applying such operations.

**Example 1:**

**Input:** nums = [3,4,2]
**Output:** 6
**Explanation:** Delete 4 to earn 4 points, consequently 3 is also deleted.
Then, delete 2 to earn 2 points.
6 total points are earned.

**Example 2:**

**Input:** nums = [2,2,3,3,3,4]
**Output:** 9
**Explanation:** Delete 3 to earn 3 points, deleting both 2's and the 4.
Then, delete 3 again to earn 3 points, and 3 again to earn 3 points.
9 total points are earned.

**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `1 <= nums[i] <= 104`

**Solution:**

Keep track of the best of avoid and use
```go
func deleteAndEarn(nums []int) int {
    counts := make([]int, 10001)
    for _,i := range nums {
        counts[i] += 1
    }
    use,avoid,prev,m := 0,0,-1,0
    for i := 1; i <= 10000; i++ {
        if counts[i] > 0 {
            m = max(use,avoid)
            if i-1 == prev {
                use = i*counts[i] + avoid
            }else {
                use = i*counts[i] + m
            }
            avoid = m
            prev = i
        }
    }
    return max(avoid,use)
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```