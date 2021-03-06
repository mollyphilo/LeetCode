# Triples with Bitwise AND Equal To Zero

Given an array of integers  `A`, find the number of triples of indices (i, j, k) such that:

-   `0 <= i < A.length`
-   `0 <= j < A.length`
-   `0 <= k < A.length`
-   `A[i] & A[j] & A[k] == 0`, where  `&` represents the bitwise-AND operator.

**Example 1:**

**Input:** [2,1,3]
**Output:** 12
**Explanation:** We could choose the following i, j, k triples:
(i=0, j=0, k=1) : 2 & 2 & 1
(i=0, j=1, k=0) : 2 & 1 & 2
(i=0, j=1, k=1) : 2 & 1 & 1
(i=0, j=1, k=2) : 2 & 1 & 3
(i=0, j=2, k=1) : 2 & 3 & 1
(i=1, j=0, k=0) : 1 & 2 & 2
(i=1, j=0, k=1) : 1 & 2 & 1
(i=1, j=0, k=2) : 1 & 2 & 3
(i=1, j=1, k=0) : 1 & 1 & 2
(i=1, j=2, k=0) : 1 & 3 & 2
(i=2, j=0, k=1) : 3 & 2 & 1
(i=2, j=1, k=0) : 3 & 1 & 2

**Note:**

1.  `1 <= A.length <= 1000`
2.  `0 <= A[i] < 2^16`

**Solution:**

Brute force: loop 3 times to find all possible solution

To reduce time complexity but increase space complexity, we pre-calculate the number the combinations made up with a number in A and a number from 1 -> 2^16 (k) such that A[i] & k == 0. Then we only need to find the number of pair of numbers in A that have AND value to be k

* Time Complexity: O(M^2),  where M = 2^16
* Space Complexity: O(M)

```go
func countTriplets(A []int) int {
    max,n := 1<<16, len(A)
    inv := make([]int, max)
    
    for i := 0; i < max; i++ {
        for j := 0; j < n; j++ {
            if A[j] & i == 0 {
                inv[i] += 1
            }
        }
    }
    
    var ans int
    for i := 0; i < n; i++ {
        for j := 0; j < n; j++ {
            ans += inv[A[i] & A[j]]
        }
    }   
    
    return ans
}
```