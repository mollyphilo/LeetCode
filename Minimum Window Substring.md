# 76. Minimum Window Substring

Given two strings  `s`  and  `t`  of lengths  `m`  and  `n`  respectively, return  _the  **minimum window substring**  of_ `s` _such that every character in_ `t` _(**including duplicates**) is included in the window. If there is no such substring__, return the empty string_ `""`_._

The testcases will be generated such that the answer is  **unique**.

A  **substring**  is a contiguous sequence of characters within the string.

**Example 1:**

	Input: s = "ADOBECODEBANC", t = "ABC"
	Output: "BANC"
	Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Example 2:**

	Input: s = "a", t = "a"
	Output: "a"
	Explanation: The entire string s is the minimum window.

**Example 3:**

	Input: s = "a", t = "aa"
	Output: ""
	Explanation: Both 'a's from t must be included in the window.
	Since the largest window of s only has one 'a', return empty string.

**Constraints:**

-   `m == s.length`
-   `n == t.length`
-   `1 <= m, n <= 105`
-   `s`  and  `t`  consist of uppercase and lowercase English letters.

**Solution:**

Sliding window, time complexity = O(m+n), m and n as above

```go
func minWindow(s string, t string) string {
  if len(s) == 0 || len(t) == 0 { return "" }
  
  dictT := make(map[byte]int)
  for i := 0; i < len(t); i++ {
    dictT[t[i]]++
  }
  
  // required is the #unique chars in T
  // formed is the #unique chars from T formed in the sliding window
  // formed is increased when a character c exist in T and S and its counter matched the that from dictT
  // when formed == required we have a candidate for the answer
  required,formed := len(dictT),0
  
  dictWindow := make(map[byte]int)
  for char := range dictT {
    dictWindow[char] = 0
  }
  // ans contains window length, left pointer, right pointer
  ans := [3]int{-1,0,0}
  var left,right int
  
  // sliding window algo
  for right < len(s) {
    if _,found := dictT[s[right]]; found {
      dictWindow[s[right]]++
      if dictWindow[s[right]] == dictT[s[right]] {
        formed++
      }
    }
    // reduce window size by moving left pointer until window not a candidate anymore
    for left <= right && formed == required {
      if ans[0] == -1 || right-left+1 < ans[0] {
        ans[0],ans[1],ans[2] = right-left+1,left,right
      }
      // before move up left, update its char's counter in dict
      if count,found := dictT[s[left]]; found {
        dictWindow[s[left]]--
        if dictWindow[s[left]] < count { formed-- }
      }
      left++
    }
    right++
  }
  
  if ans[0] == -1 { return "" }
  return s[ans[1]:ans[2]+1]
}
```

To reduce the runtime of moving left pointer in the case that m is much bigger than n, we can filter string S only to index where  characters existing in T --> Optimized sliding window

```go
func minWindow(s string, t string) string {
  if len(s) == 0 || len(t) == 0 { return "" }
  
  dictT := make(map[byte]int)
  for i := 0; i < len(t); i++ {
    dictT[t[i]]++
  }
  
  type tuple struct {
    index int
    char byte
  }
  var filteredS []tuple
  for i := 0; i < len(s); i++ {
    if _,found := dictT[s[i]]; found {
      filteredS = append(filteredS, tuple{i,s[i]})
    }
  }
  
  // required is the #unique chars in T
  // formed is the #unique chars from T formed in the sliding window
  // formed is increased when a character c exist in T and S and its counter matched the that from dictT
  // when formed == required we have a candidate for the answer
  required,formed := len(dictT),0
  
  dictWindow := make(map[byte]int)
  for char := range dictT {
    dictWindow[char] = 0
  }
  // ans contains window length, left pointer, right pointer
  ans := [3]int{-1,0,0}
  var left,right int
  
  // sliding window algo
  for right < len(filteredS) {
    rightIndex,rightChar := filteredS[right].index,filteredS[right].char
    
    dictWindow[rightChar]++
    if dictWindow[rightChar] == dictT[rightChar] {
      formed++
    }    
    
    // reduce window size by moving left pointer until window not a candidate anymore
    for left <= right && formed == required {
      leftIndex,leftChar := filteredS[left].index,filteredS[left].char
      
      if ans[0] == -1 || rightIndex-leftIndex+1 < ans[0] {
        ans[0],ans[1],ans[2] = rightIndex-leftIndex+1,leftIndex,rightIndex
      }
      // before move up left, update its char's counter in dict
      dictWindow[leftChar]--
      if dictWindow[leftChar] < dictT[leftChar] { formed-- }
      
      left++
    }
    right++
  }
  
  if ans[0] == -1 { return "" }
  return s[ans[1]:ans[2]+1]
}
```