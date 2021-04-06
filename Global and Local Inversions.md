# Global and Local Inversions

We have some permutation  `A`  of  `[0, 1, ..., N - 1]`, where  `N`  is the length of  `A`.

The number of (global) inversions is the number of  `i < j`  with  `0 <= i < j < N`  and  `A[i] > A[j]`.

The number of local inversions is the number of  `i`  with  `0 <= i < N`  and  `A[i] > A[i+1]`.

Return  `true` if and only if the number of global inversions is equal to the number of local inversions.

**Example 1:**

    Input: A = [1,0,2]
    Output: true
    Explanation: There is 1 global inversion, and 1 local inversion.

**Example 2:**

    Input: A = [1,2,0]
    Output: false
    Explanation: There are 2 global inversions, and 1 local inversion.

**Note:**

-   `A`  will be a permutation of  `[0, 1, ..., A.length - 1]`.
-   `A`  will have length in range  `[1, 5000]`.
-   The time limit for this problem has been reduced.

---

**Solution:**

Number of local inversions equal number of global inversions only when a number is greater than only one value from its position

This means abs(i-A[i]) == 1:
* If i - A[i] > 1, for example, i = 3 and A[i] = 5, then we have seen 3 numbers already, yet there are 5 numbers less than 5, meaning that there are at least 2 numbers that are less 5 that we haven yet seen, or at least 2 global inversions

* If A[i] - i > 1, for example, i = 3 and A[i] = 1, then we have seen 3 numbers already, and only one number less than 1 (0). This means at least 2 of the numbers we've seen are greater than 1, or at least 2 global inversions.

```go
func isIdealPermutation(A []int) bool {
    for i := 0; i < len(A); i++ {
        if i - A[i] > 1 || A[i]-i > 1 {
            return false
        }
    }
    
    return true
}
```

