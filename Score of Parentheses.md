# Score of Parentheses
Given a balanced parentheses string  `S`, compute the score of the string based on the following rule:

-   `()`  has score 1
-   `AB`  has score  `A + B`, where A and B are balanced parentheses strings.
-   `(A)`  has score  `2 * A`, where A is a balanced parentheses string.

**Example 1:**

	Input: "()"
	Output: 1

**Example 2:**

	Input: "(())"
	Output: 2

**Example 3:**

	Input: "()()"
	Output: 2

**Example 4:**

	Input: "(()(()))"
	Output: 6

**Note:**

1.  `S`  is a balanced parentheses string, containing only  `(`  and  `)`.
2.  `2 <= S.length <= 50`

**Solution:**

Use a stack to track balanced parenthesis counter:
* '(': add 0 to stack
* ')' and previous char is '(': set previous counter with 1
* else: sum all counters until we see a 0 -> means '('. Then replace 0 with sum*2

Lastly, return the sum of all the counters on stack

For example:
```
( ( ) ( ( ) ( ) ) )
0 0
0 1   0 0
0 1   0 1   0
0 1   0 1   1
0 1   4
10
```
```go
func scoreOfParentheses(S string) int {
    stack := []int{0}
    var sum,j int
    for i := 0; i < len(S); i++ {
        if S[i] == '(' {
            stack = append(stack,0)
        }else if S[i-1] == '(' {
            stack[len(stack)-1] = 1
        }else {
            j,sum = len(stack)-1,0
            for j >= 0 && stack[j] != 0 {
                sum += stack[j]
                j--
            }
            stack[j] = sum * 2
            stack = stack[:j+1]
        }
    }
    
    sum = 0
    for j := 0; j < len(stack); j++ {
        sum += stack[j]
    }
    return sum
}
```