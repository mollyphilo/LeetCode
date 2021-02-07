# Shortest Distance to a Character

Given a string  `s`  and a character  `c`  that occurs in  `s`, return  _an array of integers  `answer`  where_  `answer.length == s.length`  _and_  `answer[i]`  _is the shortest distance from_  `s[i]`  _to the character_  `c`  _in_  `s`.

**Example 1:**

	Input: s = "loveleetcode", c = "e"
	Output: [3,2,1,0,1,0,0,1,2,2,1,0]

**Example 2:**

	Input: s = "aaab", c = "b"
	Output: [3,2,1,0]

**Constraints:**

-   `1 <= s.length <= 104`
-   `s[i]`  and  `c`  are lowercase English letters.
-   `c`  occurs at least once in  `s`.

**Solution:**

Find distance from left to right then right to left to find the min distance

```go
func shortestToChar(s string, c byte) []int {
    n := len(s)
    ans := make([]int, n)
    cIdx := 10000 // from constraint
    
    for i := 0; i < n; i++ {
        if s[i] == c { 
            cIdx = i
        }
        ans[i] = i - cIdx    
    }
    
    for i := n-1; i >= 0; i-- {
        if s[i] == c { 
            cIdx = i
        }
        ans[i] = min(ans[i],cIdx - i)
    }

    return ans
}

func min(a,b int) int {
    if a < 0 { return b }
    if b < 0 { return a }
    if a < b { return a }
    return b
}
```