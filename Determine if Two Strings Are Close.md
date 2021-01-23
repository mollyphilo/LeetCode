# Determine if Two Strings Are Close
Two strings are considered  close  if you can attain one from the other using the following operations:

	-   Operation 1: Swap any two  existing  characters.
	    -   For example,  `abcde  -> aecdb`
	-   Operation 2: Transform  every  occurrence of one  existing  character into another  existing  character, and do the same with the other character.
	    -   For example,  `aacabb  ->  bbcbaa`  (all  `a`'s turn into  `b`'s, and all  `b`'s turn into  `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings,  `word1`  and  `word2`, return  `true` _if_ `word1` _and_ `word2` _are  close, and_ `false` _otherwise._

Example 1:

	Input: word1 = "abc", word2 = "bca"
	Output: true
	Explanation: You can attain word2 from word1 in 2 operations.
	Apply Operation 1: "abc" -> "acb"
	Apply Operation 1: "acb" -> "bca"

Example 2:

	Input: word1 = "a", word2 = "aa"
	Output: false
	Explanation: It is impossible to attain word2 from word1, or vice versa, in any number of operations.

Example 3:

	Input: word1 = "cabbba", word2 = "abbccc"
	Output: true
	Explanation: You can attain word2 from word1 in 3 operations.
	Apply Operation 1: "cabbba" -> "caabbb"
	`Apply Operation 2: "`caabbb" -> "baaccc"
	Apply Operation 2: "baaccc" -> "abbccc"

Example 4:

	Input: word1 = "cabbba", word2 = "aabbss"
	Output: false
	Explanation: It is impossible to attain word2 from word1, or vice versa, in any amount of operations.

**Constraints**:

-   `1 <= word1.length, word2.length <= 105`
-   `word1`  and  `word2`  contain only lowercase English letters.

**Solutions:**
Check that 2 strings have same characters and same number of frequencies
```go
import (
    "reflect"
    "sort"
)

func closeStrings(word1 string, word2 string) bool {
    n,m := len(word1),len(word2)
    if n != m {
        return false
    }
    chars := make(map[uint8]int)
    for i := 0; i < n; i++ {
        val,ok := chars[word1[i]]
        if !ok {
            val = 0
        }
        chars[word1[i]] = val + 1        
    }
    word1Chars := []uint8{}
    word1Freq := []int{}
    for k,v := range chars {
        word1Chars = append(word1Chars, k)
        word1Freq = append(word1Freq,v)
    }
    
    chars = make(map[uint8]int)
    for i := 0; i < n; i++ {
        val,ok := chars[word2[i]]
        if !ok {
            val = 0
        }
        chars[word2[i]] = val + 1        
    }

    word2Chars := []uint8{}
    word2Freq := []int{}
    for k,v := range chars {
        word2Chars = append(word2Chars, k)
        word2Freq = append(word2Freq,v)
    }
    sort.Slice(word1Chars, func(i, j int) bool { return word1Chars[i] < word1Chars[j]})
    sort.Slice(word2Chars, func(i, j int) bool { return word2Chars[i] < word2Chars[j]})
    
    sort.Ints(word1Freq)
    sort.Ints(word2Freq)
    return reflect.DeepEqual(word1Chars, word2Chars) && reflect.DeepEqual(word1Freq, word2Freq)
}
```