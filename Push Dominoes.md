# Push Dominoes

There are  `n`  dominoes in a line, and we place each domino vertically upright. In the beginning, we simultaneously push some of the dominoes either to the left or to the right.

After each second, each domino that is falling to the left pushes the adjacent domino on the left. Similarly, the dominoes falling to the right push their adjacent dominoes standing on the right.

When a vertical domino has dominoes falling on it from both sides, it stays still due to the balance of the forces.

For the purposes of this question, we will consider that a falling domino expends no additional force to a falling or already fallen domino.

You are given a string  `dominoes`  representing the initial state where:

-   `dominoes[i] = 'L'`, if the  `ith`  domino has been pushed to the left,
-   `dominoes[i] = 'R'`, if the  `ith`  domino has been pushed to the right, and
-   `dominoes[i] = '.'`, if the  `ith`  domino has not been pushed.

Return  _a string representing the final state_.

**Example 1:**

	Input: dominoes = "RR.L"
	Output: "RR.L"
	Explanation: The first domino expends no additional force on the second domino.

**Example 2:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/05/18/domino.png)

	Input: dominoes = ".L.R...LR..L.."
	Output: "LL.RR.LLRRLL.."

**Constraints:**

-   `n == dominoes.length`
-   `1 <= n <= 105`
-   `dominoes[i]`  is either  `'L'`,  `'R'`, or  `'.'`.

**Solution:**

```go
func pushDominoes(dominoes string) string {
    n := len(dominoes)
    forces := make([]int, n)
    
    var f int
    for i := 0; i < n; i++ {
        if dominoes[i] == 'R' {
            f = n
        } else if dominoes[i] == 'L' {
            f = 0
        } else {
            f = max(f-1, 0)
        }
        forces[i] += f
    }
    f = 0
    for i := n-1; i >= 0; i-- {
        if dominoes[i] == 'L' {
            f = n
        } else if dominoes[i] == 'R' {
            f = 0
        } else {
            f = max(f-1, 0)
        }
        
        forces[i] -= f
    }
    
    temp := []byte(dominoes)
    for i := 0; i < n; i++ {
        if forces[i] == 0 {
            temp[i] = '.'
        } else if forces[i] > 0 {
            temp[i] = 'R'
        } else {
            temp[i] = 'L'
        }
    }
    
    return string(temp)
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```