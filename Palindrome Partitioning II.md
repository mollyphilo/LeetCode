# Palindrome Partitioning II

Given a string  `s`, partition  `s`  such that every substring of the partition is a palindrome.

Return  _the minimum cuts needed_  for a palindrome partitioning of  `s`.

**Example 1:**

Input: s = "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.

**Example 2:**

Input: s = "a"
Output: 0

**Example 3:**

Input: s = "ab"
Output: 1

**Constraints:**

-   `1 <= s.length <= 2000`
-   `s`  consists of lower-case English letters only.

**Solution:**

```go
func minCut(s string) int {
    n := len(s)
    isPal := make([][]bool,n)
    for i := 0; i < n; i++ { isPal[i] = make([]bool, n) }
    
    dp := make([]int, n) // dp[i] = min cuts with substring ending at i char
    var cuts int
    
    for i := 0; i < n; i++ {
        cuts = i
        
        for j := 0; j <= i; j++ {
            if s[j] == s[i] && ( j+1 > i-1 || isPal[j+1][i-1]) {
                isPal[j][i] = true
                if j == 0 {
                    cuts = 0 
                }else {
                    cuts = min(cuts, dp[j-1] + 1)                
                }
            }
        }
        dp[i] = cuts
    }
    
    return dp[n-1]
}

func min(a,b int) int {
    if a < b { return a }
    return b
}
```