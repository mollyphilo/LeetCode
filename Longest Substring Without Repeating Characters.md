# Longest Substring Without Repeating Characters


Given a string  `s`, find the length of the  **longest substring**  without repeating characters.

**Example 1:**

    Input: s = "abcabcbb"
    Output: 3
    Explanation: The answer is "abc", with the length of 3.

**Example 2:**

    Input: s = "bbbbb"
    Output: 1
    Explanation: The answer is "b", with the length of 1.

**Example 3:**

    Input: s = "pwwkew"
    Output: 3
    Explanation: The answer is "wke", with the length of 3.
    Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Example 4:**

**Input:** s = ""
**Output:** 0

**Constraints:**

-   `0 <= s.length <= 5 * 104`
-   `s`  consists of English letters, digits, symbols and spaces.

**Solution:**

***Brute force***

There is no shy to mention brute force solution - checking uniqueness of every substring. 
```go
func lengthOfLongestSubstring(s string) int {
  ans:= 0
  n := len(s)
  for i := 0; i < n; i++ {
    for j := i+1; j < n; j++ {
      if isUnique(i,j,s) {
        if temp := j-i+1; temp > ans {
	  ans = temp
	}
      }
    }
  }
  return ans
}

func isUnique(i,j int, s string) bool {
  set := make(map[uint8]struct{})
  for k := i; k <= j; k++ {
    if _,found := set[s[k]]; found {
      return false
    }
    set[s[k]] = struct{}{}
  }
  return true
}
```

Time Complexity: O(n^3) 
Space Complexity: O(k) where k is the size of Set. k is min of the length of input string N and the size of alphabet characters M -> O(min(N,M))

***Sliding window***

It is unnecessary to check part of the substring that has already been checked. All we need to know is that for all substrings starting from index i, which longest substring does not have duplicated character. We can use a Set to add unique character starting from index i and stop when a duplication is found or when we reach end of the string. Then we start over from index i+1 and instead of using a new Set, we just need to remove character at index i from the Set because the rest are characters of a unique substring.

To optimize the above solution, instead of starting over from index i+1, we can start the search over from index from index k+1, with k is the index where we find previous occurrence of the duplicated character

```go
func lengthOfLongestSubstring(s string) int {
  // current index of character
  set := make(map[uint8]int)
  ans,i := 0,0
  for j := 0; j < len(s); j++ {
      if k,found := set[s[j]]; found {
          if k > i {
              i = k
          }
      }
      if temp := j-i+1; temp > ans {
          ans = temp
      }
      set[s[j]] = j+1 // next i = j+1 where j is previous occurence of the character
  }
  return ans
}
```
