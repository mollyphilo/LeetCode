# Count Vowels Permutation

Given an integer  `n`, your task is to count how many strings of length  `n`  can be formed under the following rules:

-   Each character is a lower case vowel (`'a'`,  `'e'`,  `'i'`,  `'o'`,  `'u'`)
-   Each vowel `'a'`  may only be followed by an  `'e'`.
-   Each vowel `'e'`  may only be followed by an  `'a'` or an  `'i'`.
-   Each vowel `'i'`  **may not**  be followed by another  `'i'`.
-   Each vowel `'o'`  may only be followed by an  `'i'`  or a `'u'`.
-   Each vowel `'u'`  may only be followed by an  `'a'.`

Since the answer may be too large, return it modulo  `10^9 + 7.`

**Example 1:**

	Input: n = 1
	Output: 5
	Explanation: All possible strings are: "a", "e", "i" , "o" and "u".

**Example 2:**

	Input: n = 2
	Output: 10
	Explanation: All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".

**Example 3:**

	Input: n = 5
	Output: 68

**Constraints:**

-   `1 <= n <= 2 * 10^4`

**Solution:**

```go
func countVowelPermutation(n int) int {
    if n == 1 { return 5 }
    d0,d1 :=  make([]int, 5), make([]int, 5)
    length := 1
    for i := 0; i < 5; i++ {
        d0[i] = 1
    }
    for length < n {
        d1[0] = (d0[1] + d0[2] + d0[4]) % 1000000007
        d1[1] = (d0[0] + d0[2]) % 1000000007
        d1[2] = (d0[1] + d0[3]) % 1000000007
        d1[3] = d0[2] % 1000000007
        d1[4] = (d0[2] + d0[3]) % 1000000007
        d0 = make([]int,5)
        copy(d0,d1)
        length++
    }
    var ans int
    for i := 0; i < 5; i++ {
        ans += d0[i]
    }
    
    return ans % 1000000007
}
```