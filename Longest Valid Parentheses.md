# Longest Valid Parentheses
Given a string containing just the characters  `'('`  and  `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

	Input: s = "(()"
	Output: 2
	Explanation: The longest valid parentheses substring is "()".

**Example 2:**

	Input: s = ")()())"
	Output: 4
	Explanation: The longest valid parentheses substring is "()()".

**Example 3:**

	Input: s = ""
	Output: 0

**Constraints:**

-   `0 <= s.length <= 3 * 104`
-   `s[i]`  is  `'('`, or  `')'`.

___

**Solution:**

```go
func longestValidParentheses(s string) int {
    stack := []int{-1}
    // -1 is to take care of the case where vaid parenthesis appears at the beginning of the string (e.g. "()((()))"
    
    var ans,n int
    for i := 0; i < len(s); i++ {
        if s[i] == '(' {
            stack = append(stack, i)
        }else {
            n = len(stack)
            // pop index of "(" from the stack to not need to add "-1" to indexes substraction
            if n > 0 {
                stack = stack[:n-1]
                n = n-1
            }
            // if stack is now empty, e.g ")()", push this index to stack as the new base
            if n == 0 {
	            stack = append(stack, i)
            }else{
                if temp := i - stack[n-1]; temp > ans {
                    ans = temp
                }
            }
        }
    }
    
    return ans
}
```