# Find and Replace Pattern

Given a list of strings  `words`  and a string  `pattern`, return  _a list of_  `words[i]`  _that match_  `pattern`. You may return the answer in  **any order**.

A word matches the pattern if there exists a permutation of letters  `p`  so that after replacing every letter  `x`  in the pattern with  `p(x)`, we get the desired word.

Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.

**Example 1:**

	Input: words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"
	Output: ["mee","aqq"]
	Explanation: "mee" matches the pattern because there is a permutation {a -> m, b -> e, ...}. 
	"ccc" does not match the pattern because {a -> c, b -> c, ...} is not a permutation, since a and b map to the same letter.

**Example 2:**

	Input: words = ["a","b","c"], pattern = "a"
	Output: ["a","b","c"]

**Constraints:**

-   `1 <= pattern.length <= 20`
-   `1 <= words.length <= 50`
-   `words[i].length == pattern.length`
-   `pattern`  and  `words[i]`  are lowercase English letters.

**Solution:**

```go
func findAndReplacePattern(words []string, pattern string) []string {
    n := len(pattern)
    encoded := make([]int, n)
    convert := make(map[uint8]int)
    for i := 0; i < n; i++ {
        if v,ok := convert[pattern[i]]; ok {
            encoded[i] = v
        }else {
            convert[pattern[i]] = i
            encoded[i] = i
        }
    }
    
    var temp []int
    var ans []string
    var match bool
    for _,w := range words {
        temp = make([]int, n)
        convert = make(map[uint8]int)
        match = true
        for i := 0; i < n; i++ {
            if v,ok := convert[w[i]]; ok {
                temp[i] = v
            }else {
                convert[w[i]] = i
                temp[i] = i
            }
            if temp[i] != encoded[i] {
                match = false
                break
            }
        }
        if match {
            ans = append(ans,w)
        }
    }
    return ans
}
```