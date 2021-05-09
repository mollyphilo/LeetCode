# Super Palindromes

Let's say a positive integer is a  **super-palindrome**  if it is a palindrome, and it is also the square of a palindrome.

Given two positive integers  `left`  and  `right`  represented as strings, return  _the number of  **super-palindromes**  integers in the inclusive range_  `[left, right]`.

**Example 1:**

	Input: left = "4", right = "1000"
	Output: 4
	Explanation: 4, 9, 121, and 484 are superpalindromes.
	Note that 676 is not a superpalindrome: 26 * 26 = 676, but 26 is not a palindrome.

**Example 2:**

	Input: left = "1", right = "2"
	Output: 1

**Constraints:**

-   `1 <= left.length, right.length <= 18`
-   `left`  and  `right`  consist of only digits.
-   `left`  and  `right`  cannot have leading zeros.
-   `left`  and  `right`  represent integers in the range  `[1, 1018]`.
-   `left`  is less than or equal to  `right`.

**Solution:**

```go
import (
    "fmt"
    "math"
    "strings"
    "strconv"
)

func superpalindromesInRange(left string, right string) int {
    l,_ := strconv.Atoi(left)
    r,_ := strconv.Atoi(right)
    end := int(math.Sqrt(float64(r)))+1
    if end > 10000 {
        end = 10000
    }
    // total number of super palindromes < 10^18 -> number of square root of those number are < 10^9
    // we need at most 4 digits to create palindrom with at most 9 digits
    // we also have right limit. Length of its square root is the total number of digits we need to build palindromes 
    pnums := map[string]struct{}{"1":struct{}{},"2":struct{}{},"3":struct{}{},"4":struct{}{},"5":struct{}{},"6":struct{}{},"7":struct{}{},"8":struct{}{},"9":struct{}{}}
    var num,s string

    for i := 1; i < end; i++ {
        num = strconv.Itoa(i)
        reversed := reverse(num)
        
        s = fmt.Sprintf("%s%s",num,reversed)
        pnums[s] = struct{}{}
        
        for j := 0; j < 10; j++ {
            s = fmt.Sprintf("%s%d%s",num,j,reversed)
            pnums[s] = struct{}{}
        }
    }
    var count int
    for n := range pnums {
        if isSuperPalindrome(n,l,r) {
            count++ 
        }
    }
    return count
}

func isSuperPalindrome(s string,left,right int) bool {
    n,_ := strconv.Atoi(s)
    n *= n
    ns := strconv.Itoa(n)
    rs := reverse(ns) 
    return ns == rs && n >= left && n <= right
}

func reverse(s string) string {
    n := len(s)
    r := make([]byte, n)
    for i := 0; i < n; i++ {
        r[i] = s[n-1-i]
    }
    return string(r)
}
```