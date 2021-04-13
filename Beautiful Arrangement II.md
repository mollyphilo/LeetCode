# Beautiful Arrangement II

Given two integers  `n`  and  `k`, you need to construct a list which contains  `n`  different positive integers ranging from  `1`  to  `n`  and obeys the following requirement:  
Suppose this list is [a1, a2, a3, ... , an], then the list [|a1  - a2|, |a2  - a3|, |a3  - a4|, ... , |an-1  - an|] has exactly  `k`  distinct integers.

If there are multiple answers, print any of them.

**Example 1:**  

    Input: n = 3, k = 1
    Output: [1, 2, 3]
    Explanation: The [1, 2, 3] has three different positive integers ranging from 1 to 3, and the [1, 1] has exactly 1 distinct integer: 1.

**Example 2:**  

    Input: n = 3, k = 2
    Output: [1, 3, 2]
    Explanation: The [1, 3, 2] has three different positive integers ranging from 1 to 3, and the [2, 1] has exactly 2 distinct integers: 1 and 2.

**Note:**  

1.  The  `n`  and  `k`  are in the range 1 <= k < n <= 104.

---

**Solution:**

The output is achieved by alternatively adding and substracting the decreasing value of k. We stop when k == 0 and the remaining numbers have values `ans[i] = i + 1`, i is the index of a number

```
n = 8 , k = 7
1 (+7) 8 (-6) 2 (+5) 7 (-4) 3 (+3) 6 (-2) 4 (-1) 5

n = 8, k = 2
1 (+2) 3 (-1) 2 4 5 6 7 8

n = 8 , k = 1
1 (+1) 2 3 4 5 6 7 8
```

```go
func constructArray(n int, k int) []int {
    ans := make([]int,n)
    for i := 0; i < n; i++ {
        ans[i] = i+1
    }
    
    sign := 1
    for i := 1; i < n && k > 0; i++ {
        ans[i] = ans[i-1] + k*sign
        k--
        sign *= -1
    }
        
    return ans
}
```