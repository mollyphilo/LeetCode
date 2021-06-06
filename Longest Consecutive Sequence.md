# Longest Consecutive Sequence

Given an unsorted array of integers  `nums`, return  _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

	Input: nums = [100,4,200,1,3,2]
	Output: 4
	Explanation: The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

**Example 2:**

	Input: nums = [0,3,7,2,5,8,4,6,0,1]
	Output: 9

**Constraints:**

-   `0 <= nums.length <= 105`
-   `-109  <= nums[i] <= 109`

**Solution:**

```go
func longestConsecutive(nums []int) int {
    set := make(map[int]struct{})
    for i := 0 ; i < len(nums); i++ {
        set[nums[i]] = struct{}{}
    }
    
    var max int
    
    for i := 0; i < len(nums); i++ {
        // don't repeat the sequence that has been processed
        // e.g. 1 2 3 4, at 1 we find sequence w/ len 4. 
        // so we don't need to process at 2
        if _,found := set[nums[i]-1]; found { continue }
        curr := 1
        elem := nums[i]+1 
        for {
            if _,found := set[elem]; !found {
                break
            }
            curr++
            elem++
        }
        if curr > max {
            max = curr
        }
    }
    
    return max
}
```