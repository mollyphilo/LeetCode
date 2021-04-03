# Ones and Zeroes

You are given an array of binary strings  `strs`  and two integers  `m`  and  `n`.

Return  _the size of the largest subset of  `strs`  such that there are  **at most**_ `m`  `0`_'s and_ `n`  `1`_'s in the subset_.

A set  `x`  is a  **subset**  of a set  `y`  if all elements of  `x`  are also elements of  `y`.

**Example 1:**

	Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
	Output: 4
	Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
	Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
	{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.

**Example 2:**

	Input: strs = ["10","0","1"], m = 1, n = 1
	Output: 2
	Explanation: The largest subset is {"0", "1"}, so the answer is 2.

**Constraints:**

-   `1 <= strs.length <= 600`
-   `1 <= strs[i].length <= 100`
-   `strs[i]`  consists only of digits  `'0'`  and  `'1'`.
-   `1 <= m, n <= 100`

----------

**Solutions:**

First approach: greedy with sorted array. Failed many test cases, for example:
```
Input: ["111","1000","1000","1000"]
Output: 1 ("111")
Expected output: 3 ("1000","1000","1000")
```
Second approach: dynamic programming, but let start with the recursive solution
```go
import "strings"

func findMaxForm(strs []string, m int, n int) int {
    return findMaxFormRec(strs,0,m,n)
}

func findMaxFormRec(strs []string,i,m,n int) int {
    if i >= len(strs) || m < 0 || n < 0 { return 0 }
    size := len(strs[i])
    zeros := strings.Count(strs[i], "0")
    ones := size - zeros
    
    var inclusive int
    if m >= zeros && n >= ones {
        inclusive = 1 + findMaxFormRec(strs, i+1, m-zeros,n-ones)
    }
    exclusive := findMaxFormRec(strs, i+1, m, n)
    if inclusive > exclusive {
        return inclusive
    }
    return exclusive
}
```

This one exceeds the time limit of some test cases, which requires us to memorize the calculated cases.
The memorization table is a double array with 2 indexes, one for *limit* number of zeros, the other for the *limit* number of ones.
If a string has num1 zeros and num2 ones, then any limit number of zeros M and limit number of ones N with M >= num1 and N >= num2, has at least 1 string

```go
import "strings"

func findMaxForm(strs []string, m int, n int) int {
    size := len(strs)
    if size == 0 { return 0 }
    
    dp := make([][]int,m+1)
    for i := 0; i <= m; i++ {
        dp[i] = make([]int,n+1)
    }
    
    var M,N int
    for _,s := range strs {
        M = strings.Count(s, "0")
        N = len(s) - M
        
        for i := m; i >= M; i-- {
            for j := n; j >= N; j-- {
                if v := dp[i-M][j-N] + 1; v > dp[i][j] {
                    dp[i][j] = v
                }
            }
        }
    }
    return dp[m][n]
}
```