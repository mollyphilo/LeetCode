# Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in  [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are  `+`,  `-`,  `*`, and  `/`. Each operand may be an integer or another expression.

**Note**  that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

**Example 1:**

    Input: tokens = ["2","1","+","3","*"]
    Output: 9
    Explanation: ((2 + 1) * 3) = 9

**Example 2:**

    Input: tokens = ["4","13","5","/","+"]
    Output: 6
    Explanation: (4 + (13 / 5)) = 6

**Example 3:**

    Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
    Output: 22
    Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
    = ((10 * (6 / (12 * -11))) + 17) + 5
    = ((10 * (6 / -132)) + 17) + 5
    = ((10 * 0) + 17) + 5
    = (0 + 17) + 5
    = 17 + 5
    = 22

**Constraints:**

-   `1 <= tokens.length <= 104`
-   `tokens[i]`  is either an operator:  `"+"`,  `"-"`,  `"*"`, or  `"/"`, or an integer in the range  `[-200, 200]`.

**Solution:**

```go
import "strconv"

func evalRPN(tokens []string) int {
    var stack []int
    
    for i := 0; i < len(tokens); i++ {
        n,err := strconv.Atoi(tokens[i])
        if err == nil {
            stack = append(stack,n)
        } else {
            n := len(stack)
            a,b := stack[n-2],stack[n-1]
            switch tokens[i] {
            case "+":
                stack = append(stack[0:n-2], a+b)
            case "-":
                stack = append(stack[0:n-2], a-b)
            case "*":
                stack = append(stack[0:n-2], a*b)
            case "/":
                stack = append(stack[0:n-2], a/b)
            }    
        }
    }
    
    return stack[0]
}
```