# Maximum Length of Pair Chain
You are given  `n`  pairs of numbers. In every pair, the first number is always smaller than the second number.

Now, we define a pair  `(c, d)`  can follow another pair  `(a, b)`  if and only if  `b < c`. Chain of pairs can be formed in this fashion.

Given a set of pairs, find the length longest chain which can be formed. You needn't use up all the given pairs. You can select pairs in any order.

**Example 1:**  

**Input:** [[1,2], [2,3], [3,4]]
**Output:** 2
**Explanation:** The longest chain is [1,2] -> [3,4]

**Note:**  

1.  The number of given pairs will be in the range [1, 1000].

**Solution:**

sort then choose pair greedily: take any pair that satisfies b < c

```go
import "sort"

func findLongestChain(pairs [][]int) int {
    sort.Slice(pairs, func(i,j int) bool { return pairs[i][1] < pairs[j][1] })
    ans,b := 0,pairs[0][0]-1
    for i := 0; i < len(pairs); i++ {
        if b < pairs[i][0] {
            b = pairs[i][1]
            ans++
        } 
    }
    
    return ans
}
```