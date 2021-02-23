# Coin Change 2

You are given coins of different denominations and a total amount of money  _amount_. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return  `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

**Input:** coins = [1,2,5], amount = 11
**Output:** 3
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**

**Input:** coins = [2], amount = 3
**Output:** -1

**Example 3:**

**Input:** coins = [1], amount = 0
**Output:** 0

**Example 4:**

**Input:** coins = [1], amount = 1
**Output:** 1

**Example 5:**

**Input:** coins = [1], amount = 2
**Output:** 2

**Constraints:**

-   `1 <= coins.length <= 12`
-   `1 <= coins[i] <= 231  - 1`
-   `0 <= amount <= 104`

**Solution:**

for each coin, see if it is possible to make up to amount x with that coin. 

```go
func change(amount int, coins []int) int {
    if amount == 0 { return 1 }
    if len(coins) == 0 { return 0 }
    dp := make([]int,amount+1)
    dp[0] = 1
    for _,c := range coins {
        for i := c; i <= amount; i++ {
            dp[i] += dp[i-c]
        }
    }
    return dp[amount]
}
```