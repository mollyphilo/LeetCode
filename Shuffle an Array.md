# Shuffle an Array

Given an integer array  `nums`, design an algorithm to randomly shuffle the array. All permutations of the array should be  **equally likely**  as a result of the shuffling.

Implement the  `Solution`  class:

-   `Solution(int[] nums)`  Initializes the object with the integer array nums.
-   `int[] reset()`  Resets the array to its original configuration and returns it.
-   `int[] shuffle()`  Returns a random shuffling of the array.

**Example 1:**

	Input
	["Solution", "shuffle", "reset", "shuffle"]
	[[[1, 2, 3]], [], [], []]
	Output
	[null, [3, 1, 2], [1, 2, 3], [1, 3, 2]]

**Explanation**

	Solution solution = new Solution([1, 2, 3]);
	solution.shuffle();    // Shuffle the array [1,2,3] and return its result.
	                       // Any permutation of [1,2,3] must be equally likely to be returned.
	                       // Example: return [3, 1, 2]
	solution.reset();      // Resets the array back to its original configuration [1,2,3]. Return [1, 2, 3]
	solution.shuffle();    // Returns the random shuffling of array [1,2,3]. Example: return [1, 3, 2]

**Constraints:**

-   `1 <= nums.length <= 200`
-   `-106  <= nums[i] <= 106`
-   All the elements of  `nums`  are  **unique**.
-   At most  `5 * 104`  calls  **in total**  will be made to  `reset`  and  `shuffle`.

**Solution:**

```go
import "math/rand"

type Solution struct {
    original,nums []int
}


func Constructor(nums []int) Solution {
    ori := make([]int, len(nums))
    copy(ori,nums)
    return Solution{original: ori, nums: nums}
}


/** Resets the array to its original configuration and return it. */
func (this *Solution) Reset() []int {
    copy(this.nums, this.original)
    return this.nums
}


/** Returns a random shuffling of the array. */
func (this *Solution) Shuffle() []int {
    rand.Seed(time.Now().UnixNano())
    n := len(this.nums)
    var swapidx int
    for i := range this.nums {
        swapidx = rand.Intn(n-i) + i
        this.nums[swapidx],this.nums[i] = this.nums[i], this.nums[swapidx]
    }
    return this.nums
}


/**
 * Your Solution object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.Reset();
 * param_2 := obj.Shuffle();
 */
```