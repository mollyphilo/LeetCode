# Longest Palindromic Substring
Given a string  `s`, return _the longest palindromic substring_  in  `s`.

**Example 1:**

	Input: s = "babad"
	Output: "bab"
	Note: "aba" is also a valid answer.

**Example 2:**

	Input: s = "cbbd"
	Output: "bb"

**Example 3:**

	Input: s = "a"
	Output: "a"

**Example 4:**

	Input: s = "ac"
	Output: "a"

**Constraints:**

-   `1 <= s.length <= 1000`
-   `s`  consist of only digits and English letters (lower-case and/or upper-case),

**Solution:**

Expand around center: O(N^2)

```go
func longestPalindrome(s string) string {
  // expand around the center approach
  n := len(s)
  if n <= 1 { return s }
  
  var l1,l2,l,maxlen,start int
  for i := 0; i < n ; i++ {
    l1 = expand(s,i,i)
    l2 = expand(s,i,i+1)
    l = max(l1,l2)
    
    if l > maxlen {
      maxlen = l
      start = i - (l-1)/2
    }
  }
  
  return s[start:start+maxlen]
}

func expand(s string, start,end int) int {
  for start >= 0 && end < len(s) && s[start] == s[end] {
    start--
    end++
  }
  
  return end-1 - (start + 1) + 1
}
func max(a,b int) int {
  if a > b { return a }
  return b
}
```

**DP Solutions:**
Base cases:
- P(i,i) = true
- P(i,i+1) = S[i] == S[i+1]

Recursive:
- P(i,j) = P(i+1,j-1) && S[i] == S[j]

Golang
```go
func longestPalindrome(s string) string {
    n := len(s)
    if n <= 1 {
        return s
    }
    var ans string = s[0:1]
    memo := make([][]bool,n)
    // store palindrome w/ size == 1
    for i := 0; i < n ; i++ {
        memo[i] = make([]bool, n)
        memo[i][i] = true
    }
    // store palindrome w/ size == 2
    for i := 0; i < n-1; i++ {
        memo[i][i+1] = (s[i] == s[i+1])
        if memo[i][i+1] {
            ans = s[i:i+2]
        }
    }
    var j int
    for size := 3; size <= n; size++ {
        for i := 0; i <= n-size; i++ {
            j = i+size-1
            memo[i][j] = memo[i+1][j-1] && s[i] == s[j]
            if memo[i][j] && len(ans) < size {
                ans = s[i:j+1]
            }
        }
    }
    return ans
}
```
Java
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() <= 1)
            return s;
        char[] c = s.toCharArray();
        
        int start,end;
        int n = c.length;
        int maxLength = 1;
        int left = 0;
        for (int i = 1; i < n; i++) {
            // odd
            start = i-1;
            end = i+1;
            while(start>=0 && end<n && c[start]==c[end]) {
                if (end-start+1 > maxLength) {
                    left = start;
                    maxLength = end-start+1;
                }
                start -= 1;
                end += 1;
            }
            // even
            start = i-1;
            end = i;
            while(start>=0 && end<n && c[start]==c[end]) {
                if (end-start+1 > maxLength) {
                    left = start;
                    maxLength = end-start+1;
                }
                start -= 1;
                end += 1;
            }
            
        }
        return s.substring(left, maxLength+left);
    }
}
```
