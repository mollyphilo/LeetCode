# Sum of Square Numbers

Given a non-negative integer  `c`, decide whether there're two integers  `a`  and  `b`  such that  `a2  + b2  = c`.

**Example 1:**

	Input: c = 5
	Output: true
	Explanation: 1 * 1 + 2 * 2 = 5

**Example 2:**

	Input: c = 3
	Output: false

**Example 3:**

	Input: c = 4
	Output: true

**Example 4:**

	Input: c = 2
	Output: true

**Example 5:**

	Input: c = 1
	Output: true

**Constraints:**

-   `0 <= c <= 231  - 1`

**Solution:**

```go
import (
  "math"
)
func judgeSquareSum(c int) bool {
  high := int(math.Sqrt(float64(c))) + 1
  var sum,low int
  
  for low <= high {
    sum = low*low + high*high
    if sum == c { 
      return true 
    }else if sum > c {
      high--
    }else {
      low++
    }
  }
  return false
}
```