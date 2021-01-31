# Next Permutation
Implement  **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be  [in place](http://en.wikipedia.org/wiki/In-place_algorithm)  and use only constant extra memory.

**Example 1:**

	Input: nums = [1,2,3]
	Output: [1,3,2]

**Example 2:**

	Input: nums = [3,2,1]
	Output: [1,2,3]

**Example 3:**

	Input: nums = [1,1,5]
	Output: [1,5,1]

**Example 4:**

	Input: nums = [1]
	Output: [1]

**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 100`

**Solution:**
```go
import "sort"

func nextPermutation(nums []int)  {
    // find the ascending order with smallest distance to swap
    n,besti,bestj := len(nums),-1,-1
    for i := n-1; i > 0; i-- {
        for j := i-1; j >= 0; j-- {
            if nums[i] > nums[j] && j > bestj {
                besti,bestj = i,j
            }
        }
    }
    
    // this is greatest permutation. return the smallest one
    if besti == -1 && bestj == -1 {
        sort.Ints(nums)
        return
    }
    
    nums[besti],nums[bestj] = nums[bestj],nums[besti]
    sort.Ints(nums[bestj+1:])
}
```
Time Complexity: wosrst case O(N^2) 

Another approach, but runtime is just as bad
```go
import "sort"

func nextPermutation(nums []int)  {
    n := len(nums)
    // find first decreasing number
    var i,bestj int = n-2,-1
    for ; i >= 0; i-- {
        if nums[i] < nums[i+1] {
            // then find the smallest number that is greater than nums[i]
            for j := i+1; j < n; j++ {
                if nums[j] <= nums[i] {
                    break
                }
                bestj = j
            }
            break
        }
    }
    
    if bestj == -1 {
        sort.Ints(nums)
        return
    }
    
    nums[i],nums[bestj] = nums[bestj],nums[i]
    sort.Ints(nums[i+1:])
}
```