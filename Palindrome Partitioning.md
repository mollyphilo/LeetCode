# Palindrome Partitioning
Given a string  `s`, partition  `s`  such that every substring of the partition is a  **palindrome**. Return all possible palindrome partitioning of  `s`.

A  **palindrome**  string is a string that reads the same backward as forward.

**Example 1:**

    Input: s = "aab"
    Output: [["a","a","b"],["aa","b"]]

**Example 2:**

    Input: s = "a"
    Output: [["a"]]

**Constraints:**

-   `1 <= s.length <= 16`
-   `s`  contains only lowercase English letters.

**Solution:**

Backtracking + DP

Time Complexity : O(N x 2^N) where N is length of string s, 2^N is number of possible substring in the worst case

Space Complexity: O(N^2). The recursive stack requires N space and dp has size 2^N

```go
func partition(s string) [][]string {
    n := len(s)
    var result [][]string
    dp := make([][]bool, n)
    for i := 0; i < n; i++ {
        dp[i] = make([]bool,n)
    }
    
    dfs(&result,s,0,&[]string{},dp)
    return result
}

func dfs(result *[][]string, s string, start int, currentList *[]string, dp [][]bool) {
    if start >= len(s) {
        list := make([]string, len(*currentList))
        copy(list,*currentList)
        *result = append(*result, list)
    }
    
    for end := start; end < len(s); end++ {
        if s[start] == s[end] && (end - start <= 2 || dp[start+1][end-1]) {
            dp[start][end] = true
            *currentList = append(*currentList, s[start:end+1])
            dfs(result,s,end+1,currentList,dp)
            *currentList = (*currentList)[:len(*currentList)-1]
        }
    }
}
```