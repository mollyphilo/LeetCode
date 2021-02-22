# Count Numbers with Unique Digits

Given a  **non-negative**  integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

**Example:**

**Input:** 2
**Output:** 91 
**Explanation:** The answer should be the total numbers in the range of 0 ≤ x < 100, 
             excluding `11,22,33,44,55,66,77,88,99`

**Constraints:**

-   `0 <= n <= 8`

**Solution:**

e.g. Let number of digits for a number is 3. The count of numbers with 3 unique digits = 9x(10-1)x(10-2)

The count of number with unique digits, for number of digits is up to 3, is:
dp[3] = d[2] + 9x(10-1)x(10-2)

Generalized, dp[i] = d[i-1] + 9x(10-1)x(10-2)..(10-(i-1))
```go
func countNumbersWithUniqueDigits(n int) int {
    if n == 0 { return 1 }
    if n == 1 { return 10 }
    dp := make([]int,n+1)
    dp[0],dp[1] = 1,10
    
    for i := 2; i <= n; i++ {
        dp[i] = 9
        for j := i-1; j > 0; j-- {
            dp[i] *= (10-j)
        }
        dp[i] += dp[i-1]
    }
    
    return dp[n]
}
```