# Number of Matching Subsequences

Given a string  `s`  and an array of strings  `words`, return  _the number of_  `words[i]`  _that is a subsequence of_  `s`.

A  **subsequence**  of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

-   For example,  `"ace"`  is a subsequence of  `"abcde"`.

**Example 1:**

	Input: s = "abcde", words = ["a","bb","acd","ace"]
	Output: 3
	Explanation: There are three strings in words that are a subsequence of s: "a", "acd", "ace".

**Example 2:**

	Input: s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]
	Output: 2

**Constraints:**

-   `1 <= s.length <= 5 * 104`
-   `1 <= words.length <= 5000`
-   `1 <= words[i].length <= 50`
-   `s`  and  `words[i]`  consist of only lowercase English letters.

**Solution:**

```go
func numMatchingSubseq(s string, words []string) int {
    chars := make(map[uint8][]int)
    n := len(s)
    for i := 0; i < n; i++ { chars[s[i]] = append(chars[s[i]], i) }
    
    ans := make(map[string]struct{})
    var prev int
    var count bool
    for _,w := range words {
        prev = -1
        count = true
        for i := 0; i < len(w); i++ {
            if _,found := ans[w[i:]]; found { break }
            v,ok := chars[w[i]]
            if !ok || v[len(v)-1] <= prev { 
                count = false
                break 
            }
            for j := 0; j < len(v); j++ {
                if v[j] > prev {
                    prev = v[j]
                    break
                }
            }
        }
        if count { ans[w] = struct{}{} }
    }
    
    return len(ans)
}
```