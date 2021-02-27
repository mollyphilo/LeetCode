# Validate Stack Sequences

Given two sequences  `pushed`  and  `popped` **with distinct values**, return  `true`  if and only if this could have been the result of a sequence of push and pop operations on an initially empty stack.

**Example 1:**

	Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
	Output: true
	Explanation: We might do the following sequence:
	push(1), push(2), push(3), push(4), pop() -> 4,
	push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1

**Example 2:**

	Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
	Output: false
	Explanation: 1 cannot be popped before 2.

**Constraints:**

-   `0 <= pushed.length == popped.length <= 1000`
-   `0 <= pushed[i], popped[i] < 1000`
-   `pushed`  is a permutation of  `popped`.
-   `pushed`  and  `popped`  have distinct values.

**Solution:**

Greedy

```go
func validateStackSequences(pushed []int, popped []int) bool {
    n,m,i,j,size := len(pushed),len(popped),0,0,0
    if n > 0 && (m == 0 ||  m > n) {
        return false
    }
    var stack []int
    for i < n && j < m {
        stack = append(stack,pushed[i])
        size = len(stack)
        for size > 0 && j < m && stack[size-1] == popped[j] {
            stack = stack[:size-1]
            size -= 1
            j++
        }
        i++
    }
    return len(stack) == 0 && i == n && j == m
}
```