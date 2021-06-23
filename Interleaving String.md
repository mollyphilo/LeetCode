# Interleaving String
Given strings  `s1`,  `s2`, and  `s3`, find whether  `s3`  is formed by an  **interleaving**  of  `s1`  and  `s2`.

An  **interleaving**  of two strings  `s`  and  `t`  is a configuration where they are divided into  **non-empty**  substrings such that:

-   `s = s1  + s2  + ... + sn`
-   `t = t1  + t2  + ... + tm`
-   `|n - m| <= 1`
-   The  **interleaving**  is  `s1  + t1  + s2  + t2  + s3  + t3  + ...`  or  `t1  + s1  + t2  + s2  + t3  + s3  + ...`

**Note:**  `a + b`  is the concatenation of strings  `a`  and  `b`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true

**Example 2:**

	Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
	Output: false

**Example 3:**

	Input: s1 = "", s2 = "", s3 = ""
	Output: true

**Constraints:**

-   `0 <= s1.length, s2.length <= 100`
-   `0 <= s3.length <= 200`
-   `s1`,  `s2`, and  `s3`  consist of lowercase English letters.

**Follow up:**  Could you solve it using only  `O(s2.length)`  additional memory space?

brute force recursive
```go
func isInterleave(s1 string, s2 string, s3 string) bool {
    if len(s1) + len(s2) != len(s3) {
        return false
    }
    return isInterleaveRecursive(s1,0,s2,0,"",s3)
}

func isInterleaveRecursive(s1 string,i int,s2 string,j int,res string, s3 string) bool {
    if string(res) == s3 && i == len(s1) && j == len(s2) {
        return true
    }
    var ans bool
    if i < len(s1) {
        ans = ans || isInterleaveRecursive(s1,i+1,s2,j, res + s1[i:i+1],s3)
    }
    if j < len(s2) {
        ans = ans || isInterleaveRecursive(s1,i,s2,j+1, res + s2[j:j+1],s3)
    }
    return ans
}
```

recursive + memorization

```go
func isInterleave(s1 string, s2 string, s3 string) bool {
    n1,n2 := len(s1), len(s2)
    if n1 + n2 != len(s3) { return false }
    memo := make([][]int, n1)
    for i := 0; i < n1; i++ {
        memo[i] = make([]int, n2)
    }
    
    return is_interleave(s1,0,s2,0,s3,0,memo)
}

func is_interleave(s1 string, i1 int, s2 string, i2 int, s3 string, i3 int, memo [][]int) bool {
    if i1 == len(s1) { return s2[i2:] == s3[i3:] }
    if i2 == len(s2) { return s1[i1:] == s3[i3:] }
    if memo[i1][i2] < 0 { return false }
    if memo[i1][i2] > 0 { return true }
    
    var ans bool
    if s3[i3] == s2[i2] && is_interleave(s1,i1,s2,i2+1,s3,i3+1,memo) || 
       s3[i3] == s1[i1] && is_interleave(s1,i1+1,s2,i2,s3,i3+1,memo) {
        ans = true
       }
    if ans {
        memo[i1][i2] = 1
    }else {
        memo[i1][i2] = -1
    }
    
    return ans
}
```

2D Dynamic Programming

```go
// 2D Dynamic Programming
func isInterleave(s1 string, s2 string, s3 string) bool {
    if len(s1) + len(s2) != len(s3) {
        return false
    }
    // dp[i][j] is the check at s1 up to index i-1 and s2 up to index j-1
    dp := make([][]bool,len(s1)+1)
    for i := 0; i <= len(s1); i++ {
        dp[i] = make([]bool, len(s2)+1)
    }
     // emptry string
    for i := 0; i <= len(s1); i++ {
        for j := 0; j <= len(s2); j++ {
            if i == 0 && j == 0 {
                dp[0][0] = true            
            } else if i == 0 {
                dp[0][j] = dp[0][j-1] && s2[j-1] == s3[i+j-1]
            }else if j == 0 {
                dp[i][0] = dp[i-1][0] && s1[i-1] == s3[i+j-1]
            }else {
                dp[i][j] = (dp[i-1][j] && s1[i-1] == s3[i+j-1]) || (dp[i][j-1] && s2[j-1] == s3[i+j-1])
            }
        }
    }
    return dp[len(s1)][len(s2)]
}
```

1D Dynamic Programming

```go
// 1D Dynamic Programming
func isInterleave(s1 string, s2 string, s3 string) bool {
    if len(s1) + len(s2) != len(s3) {
        return false
    }
    dp := make([]bool,len(s2)+1)
     // emptry string
    for i := 0; i <= len(s1); i++ {
        for j := 0; j <= len(s2); j++ {
            if i == 0 && j == 0 {
                dp[0] = true            
            } else if i == 0 {
                dp[j] = dp[j-1] && s2[j-1] == s3[i+j-1]
            }else if j == 0 {
                dp[j] = dp[j] && s1[i-1] == s3[i+j-1]
            }else {
                dp[j] = (dp[j] && s1[i-1] == s3[i+j-1]) || (dp[j-1] && s2[j-1] == s3[i+j-1])
            }
        }
    }
    return dp[len(s2)]
}
```