# Divide Two Integers

Given two integers  `dividend`  and  `divisor`, divide two integers without using multiplication, division, and mod operator.

Return the quotient after dividing  `dividend`  by  `divisor`.

The integer division should truncate toward zero, which means losing its fractional part. For example,  `truncate(8.345) = 8`  and  `truncate(-2.7335) = -2`.

**Note:**

-   Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [$−2^{31}$, $2^{31}$  − 1]. For this problem, assume that your function  **returns $2^{31}$  − 1 when the division result overflows**.

**Example 1:**

    Input: dividend = 10, divisor = 3
    Output: 3
    Explanation: 10/3 = truncate(3.33333..) = 3.

**Example 2:**

    Input: dividend = 7, divisor = -3
    Output: -2
    Explanation: 7/-3 = truncate(-2.33333..) = -2.

**Example 3:**

    Input: dividend = 0, divisor = 1
    Output: 0

**Example 4:**

    Input: dividend = 1, divisor = 1
    Output: 1

**Constraints:**

- $−2^{31}$  <= dividend, divisor <= $2^{31}$  - 1
- `divisor != 0`

**Solution:**

1) Subtraction
   
	```go
	const (
		MaxInt int = 2147483647
		MinInt int = -2147483648
	)

	func divide(dividend int, divisor int) int {
		if divisor == 0 {
				return MaxInt
		}
		if divisor == -1 && dividend < MinInt {
				return MaxInt
		}

		sign := 1
		if (dividend > 0) != (divisor > 0) {
				sign = -1
		}
		dd,ds := abs(dividend),abs(divisor)
		
		var count int
		for ds <= dd {
				dd -= ds
				count++
		}
		
		ans := sign * count
		if ans < MinInt || ans > MaxInt {
				return MaxInt
		}
		return ans
	}

	func abs(a int) int {
		if a < 0 { return -a }
		return a
	}
	```

2) Bitwise operations

	```go
	const (
			MaxInt int = 2147483647
			MinInt int = -2147483648
	)

	func divide(dividend int, divisor int) int {
		if divisor == 0 {
				return MaxInt
		}
		if divisor == -1 && dividend < MinInt {
				return MaxInt
		}

		sign := 1
		if (dividend > 0) != (divisor > 0) {
				sign = -1
		}
		dd,ds := abs(dividend),abs(divisor)
		
		var temp,count,result int
		for ds <= dd {
				temp,count = ds,1
				for temp <= dd {
						temp <<= 1
						count <<= 1
				}
				result += (count >> 1)
				dd -= (temp >> 1)
		}
		
		ans := sign * result
		if ans < MinInt || ans > MaxInt {
				return MaxInt
		}
		return ans
	}

	func abs(a int) int {
			if a < 0 { return -a }
			return a
	}
	```