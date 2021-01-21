# Valid Parentheses
Given a string  `s`  containing just the characters  `'('`,  `')'`,  `'{'`,  `'}'`,  `'['`  and  `']'`, determine if the input string is valid.

An input string is valid if:

1.  Open brackets must be closed by the same type of brackets.
2.  Open brackets must be closed in the correct order.

**Example 1:**

	Input: s = "()"
	Output: true

**Example 2:**

	Input: s = "()[]{}"
	Output: true

**Example 3:**

	Input: s = "(]"
	Output: false

**Example 4:**

	Input: s = "([)]"
	Output: false

**Example 5:**

	Input: s = "{[]}"
	Output: true

Constraints:

-   `1 <= s.length <= 104`
-   `s`  consists of parentheses only  `'()[]{}'`.

**Solutions:**

```go
func isValid(s string) bool {
    n := len(s)
    if n == 0 {
        return true
    } else if n == 1 {
        return false
    }
    queue := []uint8{s[0]}
    var char uint8
    for i := 1; i < len(s); i++ {
        if len(queue) > 0 {
            char = queue[len(queue)-1]            
        }else {
            char = 'a'
        }
        if (s[i] == '}' && char == '{') || (s[i] == ')' && char == '(') || (s[i] == ']' && char == '[') {
            queue = queue[:len(queue)-1]
        } else {
            queue = append(queue, s[i])
        }
    }
    return len(queue) == 0
}
```