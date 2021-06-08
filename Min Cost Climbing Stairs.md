# Min Cost Climbing Stairs

You are given an integer array  `cost`  where  `cost[i]`  is the cost of  `ith`  step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index  `0`, or the step with index  `1`.

Return  _the minimum cost to reach the top of the floor_.

**Example 1:**

	Input: cost = [10,15,20]
	Output: 15
	Explanation: Cheapest is: start on cost[1], pay that cost, and go to the top.

**Example 2:**

	Input: cost = [1,100,1,1,1,100,1,1,100,1]
	Output: 6
	Explanation: Cheapest is: start on cost[0], and only step on 1s, skipping cost[3].

**Constraints:**

-   `2 <= cost.length <= 1000`
-   `0 <= cost[i] <= 999`

**Solution:**

```go
func minCostClimbingStairs(cost []int) int {
    n := len(cost)
    if n <= 1 {
        return cost[n-1]
    }
    // dp[i] is the cost to climb out of i
    c0,c1,temp := cost[0],cost[1],-1
    for i := 2; i < n; i++ {
        temp = cost[i]
        if c0 < c1 {
            temp += c0
        }else {
            temp += c1
        }
        c0,c1 = c1,temp
    }
    
    if c0 < c1 {
        return c0
    }
    return c1
}
```