# Prefix and Suffix Search

Design a special dictionary which has some words and allows you to search the words in it by a prefix and a suffix.

Implement the  `WordFilter`  class:

-   `WordFilter(string[] words)`  Initializes the object with the  `words`  in the dictionary.
-   `f(string prefix, string suffix)`  Returns  _the index of the word in the dictionary_  which has the prefix  `prefix`  and the suffix  `suffix`. If there is more than one valid index, return  **the largest**  of them. If there is no such word in the dictionary, return  `-1`.

**Example 1:**

    Input
    ["WordFilter", "f"]
    [[["apple"]], ["a", "e"]]
    Output
    [null, 0]

    Explanation
    WordFilter wordFilter = new WordFilter(["apple"]);
    wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = 'e".

**Constraints:**

-   `1 <= words.length <= 15000`
-   `1 <= words[i].length <= 10`
-   `1 <= prefix.length, suffix.length <= 10`
-   `words[i]`,  `prefix`  and  `suffix`  consist of lower-case English letters only.
-   At most  `15000`  calls will be made to the function  `f`.

**Solution:**

```go
type WordFilter struct {
    keywords map[string]int
}


func Constructor(words []string) WordFilter {
    keywords := make(map[string]int)
    var b strings.Builder
    
    for wi, w := range words {
        for i := 0; i < len(w); i++ {
            for j := len(w)-1; j >= 0; j-- {
                b.WriteString(w[0:i+1])
                b.WriteRune('#')
                b.WriteString(w[j:len(w)])
                keywords[b.String()] = wi
                b.Reset()
            }
        }
    }
    
    return WordFilter{keywords: keywords}
}

func (this *WordFilter) F(prefix string, suffix string) int {
    if i,ok := this.keywords[prefix + "#" + suffix]; ok {
        return i
    }
    return -1
}


/**
 * Your WordFilter object will be instantiated and called as such:
 * obj := Constructor(words);
 * param_1 := obj.F(prefix,suffix);
 */
```

Time Comlexity: O(NxK^2 + Q) where N is the number of words, K is the maximum length of a word, and Q is the nubmer of queries
 
Space Complexity: O(NxK^2)