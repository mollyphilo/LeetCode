# Word Break

Given a string  `s`  and a dictionary of strings  `wordDict`, return  `true`  if  `s`  can be segmented into a space-separated sequence of one or more dictionary words.

**Note**  that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

	Input: s = "leetcode", wordDict = ["leet","code"]
	Output: true
	Explanation: Return true because "leetcode" can be segmented as "leet code".

**Example 2:**

	Input: s = "applepenapple", wordDict = ["apple","pen"]
	Output: true
	Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
	Note that you are allowed to reuse a dictionary word.

**Example 3:**

	Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
	Output: false

**Constraints:**

-   `1 <= s.length <= 300`
-   `1 <= wordDict.length <= 1000`
-   `1 <= wordDict[i].length <= 20`
-   `s`  and  `wordDict[i]`  consist of only lowercase English letters.
-   All the strings of  `wordDict`  are  **unique**.

**Solution:**

```go
func wordBreak(s string, wordDict []string) bool {
    dict := make(map[string]struct{})
    var minLen,maxLen,n int = 20,0,0
    for _,w := range wordDict {
        dict[w] = struct{}{}
        n = len(w)
        if n < minLen {
            minLen = n
        }
        if n > maxLen {
            maxLen = n
        }
    }
    n = len(s)
    if n < minLen { return false }
    
    dp := make([]bool,n+1)
    dp[0] = true
    for i := 0; i < n; i++ {
        for l := minLen; l <= maxLen && i+l <= n; l++ {
            if _,ok := dict[s[i:(i+l)]]; ok && dp[i] {
                dp[i+l] = true
            }
        }
    }
    return dp[n]
}
```

Trie with Memorization

```go
type Node struct {
    children []*Node
    isWord bool
}

func wordBreak(s string, wordDict []string) bool {
    root := buildtree(wordDict)
    
    cache := make(map[string]bool)
    return search(s, root, cache)
}

func search(s string, root *Node, cache map[string]bool) bool {
    if val,ok := cache[s]; ok {
        return val
    }
    
    node := root
    
    for start := 0; start < len(s); start++ {
        i := s[start]-'a'
        node = node.children[i]

        if node == nil {
            cache[s] = false
            return false
        }
        // is this is the end of the word, we can search for next word as well as continue to next letter 
        if node.isWord {
            if start == len(s)-1 {
                cache[s] = true
                return true
            }
            nextstr := s[start+1:]
            val := search(nextstr, root, cache)
            cache[nextstr] = val
            if val {
                return val
            }
        }
    }
    
    cache[s] = false
    return cache[s]
}

func buildtree(words []string) *Node {
    root := &Node{children: make([]*Node, 26), isWord: false}
    
    for _,w := range words {
        node := root
        for i := 0; i < len(w); i++ {
            if node.children[w[i]-'a'] == nil {
                node.children[w[i]-'a'] = &Node{children: make([]*Node, 26)}
            }
            node = node.children[w[i]-'a']
        }
        node.isWord = true
    }
    
    return root
}
```