# Decode Ways II

A message containing letters from  `A-Z`  can be  **encoded**  into numbers using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"

To  **decode**  an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example,  `"11106"`  can be mapped into:

-   `"AAJF"`  with the grouping  `(1 1 10 6)`
-   `"KJF"`  with the grouping  `(11 10 6)`

Note that the grouping  `(1 11 06)`  is invalid because  `"06"`  cannot be mapped into  `'F'`  since  `"6"`  is different from  `"06"`.

**In addition**  to the mapping above, an encoded message may contain the  `'*'`  character, which can represent any digit from  `'1'`  to  `'9'`  (`'0'`  is excluded). For example, the encoded message  `"1*"`  may represent any of the encoded messages  `"11"`,  `"12"`,  `"13"`,  `"14"`,  `"15"`,  `"16"`,  `"17"`,  `"18"`, or  `"19"`. Decoding  `"1*"`  is equivalent to decoding  **any**  of the encoded messages it can represent.

Given a string  `s`  containing digits and the  `'*'`  character, return  _the  **number**  of ways to  **decode**  it_.

Since the answer may be very large, return it  **modulo**  `109  + 7`.

**Example 1:**

	Input: s = "*"
	Output: 9
	Explanation: The encoded message can represent any of the encoded messages "1", "2", "3", "4", "5", "6", "7", "8", or "9".
	Each of these can be decoded to the strings "A", "B", "C", "D", "E", "F", "G", "H", and "I" respectively.
	Hence, there are a total of 9 ways to decode "*".

**Example 2:**

	Input: s = "1*"
	Output: 18
	Explanation: The encoded message can represent any of the encoded messages "11", "12", "13", "14", "15", "16", "17", "18", or "19".
	Each of these encoded messages have 2 ways to be decoded (e.g. "11" can be decoded to "AA" or "K").
	Hence, there are a total of 9 * 2 = 18 ways to decode "1*".

**Example 3:**

	Input: s = "2*"
	Output: 15
	Explanation: The encoded message can represent any of the encoded messages "21", "22", "23", "24", "25", "26", "27", "28", or "29".
	"21", "22", "23", "24", "25", and "26" have 2 ways of being decoded, but "27", "28", and "29" only have 1 way.
	Hence, there are a total of (6 * 2) + (3 * 1) = 12 + 3 = 15 ways to decode "2*".

**Constraints:**

-   `1 <= s.length <= 105`
-   `s[i]`  is a digit or  `'*'`.

**Solution:**

1/ Recursion with memorization: TLE

```go
const MOD int = 1000000007
func numDecodings(s string) int {
    memo := make([]int, len(s))
    return num_decodings(s, len(s)-1, memo)
}

func num_decodings(s string, i int, memo []int) int {
    if i < 0 { return 1 }
    if memo[i] > 0 { return memo[i] }
    res := num_decodings(s, i-1, memo) % MOD
    
    if s[i] != '*' {
        if s[i] == '0' { res = 0 } // encoding starts with 1
        if i > 0 && (s[i-1] == '1' || (s[i-1] == '2' && s[i] <= '6') || (s[i-1] == '*' && s[i] > '6')){
            res += num_decodings(s, i-2, memo) % MOD
        } else if i > 0 &&s[i-1] == '*' && s[i] <= '6'{
            res += 2*num_decodings(s, i-2, memo) % MOD
        }
    }else {
        res *= 9 // * is converted to 1->9
        if i > 0 && s[i-1] == '1' {
            res += 9 * num_decodings(s, i-2, memo) % MOD
        }else if i > 0 && s[i-1] == '2' {
            res += 6 * num_decodings(s, i-2, memo) % MOD
        }else if i > 0 && s[i-1] == '*' {
            res += 15 * num_decodings(s, i-2, memo) % MOD
        }
    }
    memo[i] = res % MOD
    return memo[i]
}
```
Convert to constant space DP & accepted

```go
const MOD int = 1000000007
func numDecodings(s string) int {
    d0 := 1
    var d1 int
    if s[0] == '0' {
        d1 = 0
    }else if s[0] == '*' { 
        d1 = 9
    }else {
        d1 = 1
    }
    
    for i := 1; i < len(s);  i++ {
        temp := d1
        if s[i] != '*' {
            if s[i] == '0' {
                d1 = 0
            }
            if s[i-1] == '1' || (s[i-1] == '2' && s[i] <= '6') || (s[i-1] == '*' && s[i] > '6') {
                d1 = (d1+d0) % MOD
            }else if s[i-1] == '*' && s[i] <= '6' {
                d1 = (d1+2*d0) % MOD
            }
        }else {
            d1 = (d1 * 9) % MOD
            if i > 0 && s[i-1] == '1' {
                d1 = (d1 + 9*d0) % MOD
            }else if i > 0 && s[i-1] == '2' {
                d1 = (d1 + 6*d0) % MOD
            }else if i > 0 && s[i-1] == '*' {
                d1 = (d1 + 15*d0) % MOD
            }
        }
        d0 = temp
    }
    
    return d1
}
```