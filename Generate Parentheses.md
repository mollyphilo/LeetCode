
# Generate Parentheses

Given  `n`  pairs of parentheses, write a function to  _generate all combinations of well-formed parentheses_.

**Example 1:**

	Input: n = 3
	Output: ["((()))","(()())","(())()","()(())","()()()"]

**Example 2:**

	Input: n = 1
	Output: ["()"]

**Constraints:**

-   `1 <= n <= 8`

**Solution:**

backtracking

```go
func generateParenthesis(n int) []string {
    var res []string
    backtrack(n,[]byte{},&res, 0,0)
    return res    
}

func backtrack(n int, parens []byte, res *[]string,open,closed int) {
    if len(parens) == n * 2 {
        *res = append(*res,string(parens))
        return
    }
    
    if open < n {
        parens = append(parens, '(')
        backtrack(n, parens,res,open+1,closed)
        parens = parens[0:len(parens)-1]
    }
    
    if closed < open {
        parens = append(parens, ')')
        backtrack(n, parens,res,open,closed+1)
        parens = parens[0:len(parens)-1]
    }
}
```