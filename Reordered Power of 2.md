# Reordered Power of 2

You are given an integer  `n`. We reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return  `true`  _if and only if we can do this so that the resulting number is a power of two_.

**Example 1:**

	Input: n = 1
	Output: true

**Example 2:**

	Input: n = 10
	Output: false

**Example 3:**

	Input: n = 16
	Output: true

**Example 4:**

	Input: n = 24
	Output: false

**Example 5:**

	Input: n = 46
	Output: true

**Constraints:**

-   `1 <= n <= 109`

----------

**Solution:**

```go
func reorderedPowerOf2(n int) bool {
    arr := count(n)
    ans := 1
    for i := 0; i < 31; i++ {
        if arr == count(ans << i) {
            return true
        }
    }
    return false
}

func count(n int) [10]int {
    arr := [10]int{}
    for n > 0 {
        arr[n%10] += 1
        n /= 10
    }
    return arr
}
```