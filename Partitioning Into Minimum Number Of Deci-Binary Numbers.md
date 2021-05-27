# Partitioning Into Minimum Number Of Deci-Binary Numbers

A decimal number is called  **deci-binary**  if each of its digits is either  `0`  or  `1`  without any leading zeros. For example,  `101`  and  `1100`  are  **deci-binary**, while  `112`  and  `3001`  are not.

Given a string  `n`  that represents a positive decimal integer, return  _the  **minimum**  number of positive  **deci-binary**  numbers needed so that they sum up to_ `n`_._

**Example 1:**

	Input: n = "32"
	Output: 3
	Explanation: 10 + 11 + 11 = 32

**Example 2:**

	Input: n = "82734"
	Output: 8

**Example 3:**

	Input: n = "27346209830709182346"
	Output: 9

**Constraints:**

-   `1 <= n.length <= 105`
-   `n`  consists of only digits.
-   `n`  does not contain any leading zeros and represents a positive integer.

**Solution:**

```go
/*
32 = 
11
11
10

82734 =
11111
11111
10111
10101
10100
10100
10100
10000

This means we only need to find the max digit in the string n
82734 -> 8 -> 8 numbers
*/
func minPartitions(n string) int {
    var maxDigit rune = '0'
    for _,v := range n {
        if v > maxDigit {
            maxDigit = v
        }
    }
    
    return int(maxDigit-'0')
}
```