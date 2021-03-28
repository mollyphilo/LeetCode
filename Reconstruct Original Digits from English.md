# Reconstruct Original Digits from English

Given a  **non-empty**  string containing an out-of-order English representation of digits  `0-9`, output the digits in ascending order.

**Note:**  

1.  Input contains only lowercase English letters.
2.  Input is guaranteed to be valid and can be transformed to its original digits. That means invalid inputs such as "abc" or "zerone" are not permitted.
3.  Input length is less than 50,000.

**Example 1:**  

	Input: "owoztneoer"

	Output: "012"

**Example 2:**  

	Input: "fviefuro"

	Output: "45"
**Solution:**

```go
func originalDigits(s string) string {
    chars := make(map[uint8]int)
    for i := 0; i < len(s); i++ {
        chars[s[i]] += 1
    }
    
    findNumber := func(u uint8, word, number string) string {
        count := chars[u]
        for i := 0; i < len(word); i++ {
            chars[word[i]] -= count
        }
        return strings.Repeat(number,count)
    }
    
    ans := make([]string,10)
    ans[0] = findNumber('z', "zero", "0")
    ans[2] = findNumber('w', "two", "2")
    ans[4] = findNumber('u', "four", "4")
    ans[5] = findNumber('f', "five", "5")
    ans[6] = findNumber('x', "six", "6")
    ans[1] = findNumber('o', "one", "1")
    ans[3] = findNumber('r', "three", "3")
    ans[7] = findNumber('v', "seven", "7")
    ans[8] = findNumber('g', "eight", "8")
    ans[9] = findNumber('e', "nine", "9")
    
    return strings.Join(ans, "")
}
```