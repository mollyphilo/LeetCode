# Valid Triangle Number

Given an integer array  `nums`, return  _the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle_.

**Example 1:**

	Input: nums = [2,2,3,4]
	Output: 3
	Explanation: Valid combinations are: 
	2,3,4 (using the first 2)
	2,3,4 (using the second 2)
	2,2,3

**Example 2:**

	Input: nums = [4,2,3,4]
	Output: 4

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `0 <= nums[i] <= 1000`

**Solution:**

```go
import "sort"

func triangleNumber(nums []int) int {
    n := len(nums)
    sort.Ints(nums)
    var ans,k int
    for i := 0; i < n-2; i++ {
        if nums[i] == 0 { continue }
        for j := i+1; j < n-1; j++ { 
            k = j+1
            for k < n && nums[i] + nums[j] > nums[k] { k++ }
            ans += k-j-1
        }
    }
    
    
    return ans
}
```