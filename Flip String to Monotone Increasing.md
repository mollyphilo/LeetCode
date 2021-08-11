# Flip String to Monotone Increasing

A binary string is monotone increasing if it consists of some number of  `0`'s (possibly none), followed by some number of  `1`'s (also possibly none).

You are given a binary string  `s`. You can flip  `s[i]`  changing it from  `0`  to  `1`  or from  `1`  to  `0`.

Return  _the minimum number of flips to make_ `s` _monotone increasing_.

**Example 1:**

	Input: s = "00110"
	Output: 1
	Explanation: We flip the last digit to get 00111.

**Example 2:**

	Input: s = "010110"
	Output: 2
	Explanation: We flip to get 011111, or alternatively 000111.

**Example 3:**

	Input: s = "00011000"
	Output: 2
	Explanation: We flip to get 00000000.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s[i]`  is either  `'0'`  or  `'1'`.

**Solution:**

```go
/* what about reversed order?
if s[i] == '0':
    flip2zero[i] = 0 + min(flip2zero[i+1],flip2one[i+1]) // all digits after i could be 0 or 1
    flip2one[i] = 1 + flip2one[i+1] // all digits after i must be 1
else if s[i] == '1':
    flip2zero[i] = 1 + min(flip2zero[i+1],flip2one[i+1]) // all digits after i could be 0 or 1
    flip2one[i] = 0 +  flip2one[i+1] // all digits after i must be 1
    
==> flip2zero[i] = int(s[i]-'0') + min(flip2zero[i+1],flip2one[i+1]) 
    flip2one[i] = int('1'-s[i]) +  flip2one[i+1]
*/

func minFlipsMonoIncr(s string) int {
    var flip2zero,flip2one int
    for i := len(s)-1; i >= 0; i-- {
        flip2zero = int(s[i]-'0') + min(flip2zero,flip2one) 
        flip2one = int('1'-s[i]) + flip2one
    }
    
    return min(flip2zero,flip2one)
}

func min(a,b int) int {
    if a < b { return a }
    return b
}
```