# Remove All Adjacent Duplicates in String II

You are given a string  `s`  and an integer  `k`, a  `k`  **duplicate removal**  consists of choosing  `k`  adjacent and equal letters from  `s`  and removing them, causing the left and the right side of the deleted substring to concatenate together.

We repeatedly make  `k`  **duplicate removals**  on  `s`  until we no longer can.

Return the final string after all such duplicate removals have been made. It is guaranteed that the answer is unique.

**Example 1:**

    Input: s = "abcd", k = 2
    Output: "abcd"
    Explanation: There's nothing to delete.

**Example 2:**

    Input: s = "deeedbbcccbdaa", k = 3
    Output: "aa"
    Explanation: First delete "eee" and "ccc", get "ddbbbdaa"
    Then delete "bbb", get "dddaa"
    Finally delete "ddd", get "aa"

**Example 3:**

    Input: s = "pbbcggttciiippooaais", k = 2
    Output: "ps"

**Constraints:**

-   `1 <= s.length <= 105`
-   `2 <= k <= 104`
-   `s`  only contains lower case English letters.

---

**Solutions:**

Stack to keep track of adjacent duplicates

```go
import "strings"

type Item struct {
    char uint8
    count int
}

func removeDuplicates(s string, k int) string {
    var stack []Item 
    
    var n int
    for i := 0; i < len(s); i++ {
        n = len(stack)
        if n == 0 {
            stack = append(stack, Item{char: s[i], count: 1})
        } else if stack[n-1].char == s[i] {
            if stack[n-1].count + 1 == k {
                stack = stack[:n-1]
            }else {
                stack[n-1].count += 1
            }
        } else {
            stack = append(stack, Item{char: s[i], count: 1})
        }
    }
    
    ans := make([]string, len(stack))
    for i := 0; i < len(stack); i++ {
        ans[i] = strings.Repeat(string(stack[i].char), stack[i].count)
    }
    return strings.Join(ans, "")
}
```

Time Complexity: O(N)

Space Complexity: O(N)

Optimization:

```go
func removeDuplicates(s string, k int) string {
    var stack [][2]int
    
    var n,char int
    for i := 0; i < len(s); i++ {
        char = int(s[i]-'a')
        n = len(stack)
        if n == 0 {
            stack = append(stack, [2]int{char,1})
        } else if stack[n-1][0] == char {
            if stack[n-1][1] + 1 == k {
                stack = stack[:n-1]
            }else {
                stack[n-1][1] += 1
            }
        } else {
            stack = append(stack, [2]int{char,1})
        }
    }
    
    var ans []byte
    for i := 0; i < len(stack); i++ {
        for j := 0; j < stack[i][1]; j++ {
            ans = append(ans, byte(stack[i][0] + 'a'))
        }
    }
    return string(ans)
}
```