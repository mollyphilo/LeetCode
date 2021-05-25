# To Lower Case
Given a string  `s`, return  _the string after replacing every uppercase letter with the same lowercase letter_.

**Example 1:**

	Input: s = "Hello"
	Output: "hello"

**Example 2:**

	Input: s = "here"
	Output: "here"

**Example 3:**

	Input: s = "LOVELY"
	Output: "lovely"

**Constraints:**

-   `1 <= s.length <= 100`
-   `s`  consists of printable ASCII characters.

**Solution:**

```go
func toLowerCase(s string) string {
    chars := make(map[uint8]uint8)
    var i uint8
    for i = 0; i < 26; i++ {
        chars['A'+i] = 'a'+i
    }
    b := []byte(s)
    for i := 0; i < len(s); i++ {
        if v,ok := chars[b[i]]; ok {
            b[i] = v
        }
    }
    return string(b)
}
```