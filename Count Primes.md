 # Count Primes

Count the number of prime numbers less than a non-negative number,  `n`.

**Example 1:**

	Input: n = 10
	Output: 4
	Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

**Example 2:**

	Input: n = 0
	Output: 0

**Example 3:**

	Input: n = 1
	Output: 0

**Constraints:**

-   `0 <= n <= 5 * 106`

**Solution:**

Shamelessly copied Leetcode's hint and solution
```go
func countPrimes(n int) int {
    isComposite := make([]bool,n)
    for i := 2; i*i < n; i++ {
        if isComposite[i] { continue }
        for j := i*i; j < n; j += i {
            isComposite[j] = true
        }
    }
    
    var count int
    for i := 2; i < n; i++ {
        if !isComposite[i] { count++ }
    }
    
    return count
}
```