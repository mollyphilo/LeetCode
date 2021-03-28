# Vowel Spellchecker

Given a  `wordlist`, we want to implement a spellchecker that converts a query word into a correct word.

For a given  `query`  word, the spell checker handles two categories of spelling mistakes:

-   Capitalization: If the query matches a word in the wordlist (**case-insensitive**), then the query word is returned with the same case as the case in the wordlist.
    -   Example:  `wordlist = ["yellow"]`,  `query = "YellOw"`:  `correct = "yellow"`
    -   Example:  `wordlist = ["Yellow"]`,  `query = "yellow"`:  `correct = "Yellow"`
    -   Example:  `wordlist = ["yellow"]`,  `query = "yellow"`:  `correct = "yellow"`
-   Vowel Errors: If after replacing the vowels  `('a', 'e', 'i', 'o', 'u')`  of the query word with any vowel individually, it matches a word in the wordlist (**case-insensitive**), then the query word is returned with the same case as the match in the wordlist.
    -   Example:  `wordlist = ["YellOw"]`,  `query = "yollow"`:  `correct = "YellOw"`
    -   Example:  `wordlist = ["YellOw"]`,  `query = "yeellow"`:  `correct = ""`  (no match)
    -   Example:  `wordlist = ["YellOw"]`,  `query = "yllw"`:  `correct = ""`  (no match)

In addition, the spell checker operates under the following precedence rules:

-   When the query exactly matches a word in the wordlist (**case-sensitive**), you should return the same word back.
-   When the query matches a word up to capitlization, you should return the first such match in the wordlist.
-   When the query matches a word up to vowel errors, you should return the first such match in the wordlist.
-   If the query has no matches in the wordlist, you should return the empty string.

Given some  `queries`, return a list of words  `answer`, where  `answer[i]`  is the correct word for  `query = queries[i]`.

**Example 1:**

	Input: wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]
	Output: ["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]

**Example 2:**

	Input: wordlist = ["yellow"], queries = ["YellOw"]
	Output: ["yellow"]

**Constraints:**

-   `1 <= wordlist.length, queries.length <= 5000`
-   `1 <= wordlist[i].length, queries[i].length <= 7`
-   `wordlist[i]`  and  `queries[i]`  consist only of only English letters.

**Solution:**

```go
func spellchecker(wordlist []string, queries []string) []string {
    match := make(map[string]struct{})
    uppercase := make(map[string][]string)
    vowel := make(map[string][]string)
    
    replaceFunc := func(r rune) rune {
		switch r {
            case 'a','e','o','u','i':
                return '_'
        }
		return r
	}
    for _,w := range wordlist {
        match[w] = struct{}{}
        k1,k2 := strings.ToLower(w),strings.Map(replaceFunc, strings.ToLower(w))
        uppercase[k1] = append(uppercase[k1], w)
        vowel[k2] = append(vowel[k2], w)
    }

    n := len(queries)
    ans := make([]string,n)
    for i := 0; i < n; i++ {
        if _,ok := match[queries[i]]; ok {
            ans[i] = queries[i]
        }else if v,ok := uppercase[strings.ToLower(queries[i])]; ok {
            ans[i] = v[0]
        }else if v,ok := vowel[strings.Map(replaceFunc, strings.ToLower(queries[i]))]; ok {
            ans[i] = v[0]
        }else {
            ans[i] = ""
        }
    }
    return ans
}
```