# Find K Closest Elements

Given a  **sorted**  integer array  `arr`, two integers  `k`  and  `x`, return the  `k`  closest integers to  `x`  in the array. The result should also be sorted in ascending order.

An integer  `a`  is closer to  `x`  than an integer  `b`  if:

-   `|a - x| < |b - x|`, or
-   `|a - x| == |b - x|`  and  `a < b`

**Example 1:**

	Input: arr = [1,2,3,4,5], k = 4, x = 3
	Output: [1,2,3,4]

**Example 2:**

	Input: arr = [1,2,3,4,5], k = 4, x = -1
	Output: [1,2,3,4]

**Constraints:**

-   `1 <= k <= arr.length`
-   `1 <= arr.length <= 104`
-   `arr`  is sorted in  **ascending**  order.
-   `-104  <= arr[i], x <= 104`

**Solutions:**

1. Sort

Time Complexity: O(NlogN + klogk)  with the former for first sort, the latter for 2nd sort. Space complexity O(k) for the answer array
```go
import "sort"

func findClosestElements(arr []int, k int, x int) []int {
    sort.Slice(arr, func(i,j int) bool { 
        a,b := abs(arr[i], x), abs(arr[j],x)
        return (a == b && arr[i] < arr[j]) ||  (a < b)
    })
    ans := arr[0:k]
    sort.Ints(ans)
    return ans
}

func abs(a,b int) int {
    if a > b { return a - b } 
    return b - a
}
```

2. Binary search with sliding window

Search for the closest number to x in `arr`. Then use sliding window to find k numbers around the found number. 

Time Complexity: O(logN + k). Space complexity: O(12)
```go
func findClosestElements(arr []int, k int, x int) []int {
    n := len(arr)
    if n == k { return arr }
    // binary search the closet number to x
    low,high := 0,n
    var mid int
    for low < high {
        mid = (low + high)/2
        if arr[mid] >= x { 
            high = mid
        }else {
            low = mid + 1    
        }
    }
    // left bound
    low -= 1
    // right bound
    high = low + 1
    for high - low - 1 < k {
        if low == -1 {
            high++
            continue
        }
        if high == n || abs(arr[low]-x) <= abs(arr[high]-x) {
            low--
        }else {
            high++
        }
    }
    
    return arr[low+1:high]
}

func abs(n int) int {
    if n > 0 { return n }
    return -n
}
```
3. Binary search to find the left bound

Time complexity: O(log(N-k)+k). Space complexity: O(1)

```go
func findClosestElements(arr []int, k int, x int) []int {
    n := len(arr)
    if n == k { return arr }
    low,high := 0,n-k
    var mid int
    for low < high {
        mid = (low+high)/2
        if x - arr[mid] > arr[mid+k] - x {
            low = mid + 1
        }else {
            high = mid
        }
    }
    
    return arr[low:low+k]
}
```