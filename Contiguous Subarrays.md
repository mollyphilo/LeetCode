# Contiguous Subarrays

You are given an array arr of N integers. For each index i, you are required to determine the number of contiguous subarrays that fulfill the following conditions:

-   The value at index i must be the maximum element in the contiguous subarrays, and
    
-   These contiguous subarrays must either start from or end on index i.
    

Signature  int[] countSubarrays(int[] arr)

Input

-   Array arr is a non-empty list of unique integers that range between 1 to 1,000,000,000
    
-   Size N is between 1 and 1,000,000
    

Output An array where each index i contains an integer denoting the maximum number of contiguous subarrays of arr[i]

Example: 

	arr = [3, 4, 1, 6, 2]
	output = [1, 3, 1, 5, 1]

Explanation:

-   For index 0 - [3] is the only contiguous subarray that starts (or ends) with 3, and the maximum value in this subarray is 3.
    
-   For index 1 - [4], [3, 4], [4, 1]
    
-   For index 2 - [1]
    
-   For index 3 - [6], [6, 2], [1, 6], [4, 1, 6], [3, 4, 1, 6]
    
-   For index 4 - [2]
    

So, the answer for the above input is [1, 3, 1, 5, 1]

**Solution:**

```go
import (
  "fmt"
  "reflect"
)

func countSubarrays(arr []int) []int {
  // Write your code here
  n,maxIdx := len(arr),0
  ans := make([]int, n)
  ans[0] = 1
  for i := 1; i < n; i++ {
    if arr[maxIdx] < arr[i] {
      ans[i] = i - (-1)
      maxIdx = i
    }else {
      // find j closest to i and greater than i
      var closestMax int = maxIdx
      for j := maxIdx+1; j < i; j++ {
        if arr[j] >= arr[i] { closestMax = j }
      }
      ans[i] = i-closestMax
    }
  }
  
  maxIdx = n-1
  for i := n-2; i >= 0; i-- {
    if arr[maxIdx] < arr[i] {
      ans[i] += n-i-1
      maxIdx = i
    }else {
      // find j closest to i and greater than i
      var closestMax int = maxIdx
      for j := maxIdx-1; j > i; j-- {
        if arr[j] >= arr[i] { closestMax = j }
      }
      fmt.Printf("max=%d closestMax=%d arr[i]=%d\n", arr[maxIdx], arr[closestMax], arr[i])
      ans[i] += closestMax-i-1
    }
  }
	return ans;
}

```