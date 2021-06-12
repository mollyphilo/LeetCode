# Sliding Window Maximum

You are given an array of integers `nums`, there is a sliding window of size  `k`  which is moving from the very left of the array to the very right. You can only see the  `k`  numbers in the window. Each time the sliding window moves right by one position.

Return  _the max sliding window_.

**Example 1:**

    Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
    Output: [3,3,5,5,6,7]
    Explanation: 
    Window position                Max
    ---------------               -----
    [1  3  -1] -3  5  3  6  7      3
    1 [3  -1  -3] 5  3  6  7       3
    1  3 [-1  -3  5] 3  6  7       5
    1  3  -1 [-3  5  3] 6  7       5
    1  3  -1  -3 [5  3  6] 7       6
    1  3  -1  -3  5 [3  6  7]      7

**Example 2:**

    Input: nums = [1], k = 1
    Output: [1]

**Example 3:**

    Input: nums = [1,-1], k = 1
    Output: [1,-1]

**Example 4:**

    Input: nums = [9,11], k = 2
    Output: [11]

**Example 5:**

    Input: nums = [4,-2], k = 2
    Output: [4]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104  <= nums[i] <= 104`
-   `1 <= k <= nums.length`

**Solution:**

Monoqueue

```go
import "sort"

func maxSlidingWindow(nums []int, k int) []int {
    n := len(nums)
    monoqueue := make([]int,k-1) // array of indices
    for i := 0; i < k-1; i++ {
        monoqueue[i] = i
    }
    sort.Slice(monoqueue, func(i,j int) bool { return nums[monoqueue[i]] > nums[monoqueue[j]] } )
    var ans []int
    
    for i := k-1; i < n; i++ {
        // pop back those less than nums[i]
        for len(monoqueue) > 0 && nums[i] > nums[monoqueue[len(monoqueue)-1]] {
            monoqueue = monoqueue[0:len(monoqueue)-1]
        }
        monoqueue = append(monoqueue,i)
        // pop front those less than i-k
        for len(monoqueue) > 0 && monoqueue[0] <= i-k {
            monoqueue = monoqueue[1:]
        }
        ans = append(ans, nums[monoqueue[0]])
    }
    return ans
}
```