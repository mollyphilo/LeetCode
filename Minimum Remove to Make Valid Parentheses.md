# Minimum Remove to Make Valid Parentheses

Given a string  s of `'('` , `')'` and lowercase English characters.

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting  _parentheses string_  is valid and return  **any**  valid string.

Formally, a  _parentheses string_  is valid if and only if:

-   It is the empty string, contains only lowercase characters, or
-   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
-   It can be written as `(A)`, where `A` is a valid string.

**Example 1:**

	Input: s = "lee(t(c)o)de)"
	Output: "lee(t(c)o)de"
	Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.

**Example 2:**

	Input: s = "a)b(c)d"
	Output: "ab(c)d"

**Example 3:**

	Input: s = "))(("
	Output: ""
	Explanation: An empty string is also valid.

**Example 4:**

	Input: s = "(a(b(c)d)"
	Output: "a(b(c)d)"

**Constraints:**

-   `1 <= s.length <= 10^5`
-   `s[i]` is one of `'('`  ,  `')'`  and lowercase English letters`.`

**Solution**

Look complicated but beat 100% Go submissions

Mark extra parenthesis as empty character (0) from left to right then from right to left. Clean up empty character before return new string

```go
func minRemoveToMakeValid(s string) string {
    chars := []uint8(s)
    n,count := len(s),0
    for i := 0; i < n; i++ {
        if s[i] == '(' {
            count += 1
        }else if s[i] == ')' {
            if count > 0 {
                count -= 1
            }else {
                chars[i] = 0 // make this empty rune
            }
        }
    }

    if count == 0 {
        s = string(chars)
        chars = []uint8{}
        for i := 0; i < len(s); i++ {
            if s[i] != 0 {
                chars = append(chars,s[i])
            }
        }
    }
    
    for i := n-1; i >= 0; i-- {
        if s[i] == '(' && count > 0 {
            count -= 1
            chars[i] = 0 // make this empty rune because we have extra '('
        }else if s[i] == ')' && count < 0 {
            count += 1
            chars[i] = 0 // make this empty rune
        }
        
        if count == 0 {
            break
        }
    }

    s = string(chars)
    chars = []uint8{}
    for i := 0; i < len(s); i++ {
        if s[i] != 0 {
            chars = append(chars,s[i])
        }
    }
    return string(chars)
}
```

Time and space complexity: O(N)