# Longest String Chain

Given a list of words, each word consists of English lowercase letters.

Let's say  `word1`  is a predecessor of  `word2`  if and only if we can add exactly one letter anywhere in  `word1`  to make it equal to  `word2`. For example,  `"abc"`  is a predecessor of  `"abac"`.

A  _word chain_ is a sequence of words  `[word_1, word_2, ..., word_k]`  with  `k >= 1`, where  `word_1`  is a predecessor of  `word_2`,  `word_2`  is a predecessor of  `word_3`, and so on.

Return the longest possible length of a word chain with words chosen from the given list of  `words`.

**Example 1:**

    Input: words = ["a","b","ba","bca","bda","bdca"]
    Output: 4
    Explanation: One of the longest word chain is "a","ba","bda","bdca".

**Example 2:**

    Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
    Output: 5

**Constraints:**

-   `1 <= words.length <= 1000`
-   `1 <= words[i].length <= 16`
-   `words[i]`  only consists of English lowercase letters.

**Solution:**

Bottom-up DP

```go
import "sort"

func longestStrChain(words []string) int {
    var max int
    dp := make(map[string]int)
    sort.Slice(words, func(i,j int) bool { return len(words[i]) < len(words[j]) })
    var temp []byte
    var s string
    
    for i := 0; i < len(words); i++ {
        if len(words[i]) == 1 { 
            dp[words[i]] = 1 
        }else {
            temp = []byte(words[i])
            for j := 0; j < len(temp); j++ {
                s = string(temp[:j])+string(temp[j+1:])
                if v := dp[s]+1; v > dp[words[i]] {
                    dp[words[i]] = v
                }
            }
        }
        
        if dp[words[i]] > max {
            max = dp[words[i]]
        }
    }
    return max
}
```