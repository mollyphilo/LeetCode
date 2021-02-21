# Coin Change

You are given coins of different denominations and a total amount of money  _amount_. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return  `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

	Input: coins = [1,2,5], amount = 11
	Output: 3
	Explanation: 11 = 5 + 5 + 1

**Example 2:**

	Input: coins = [2], amount = 3
	Output: -1

**Example 3:**

	Input: coins = [1], amount = 0
	Output: 0

**Example 4:**

	Input: coins = [1], amount = 1
	Output: 1

**Example 5:**

	Input: coins = [1], amount = 2
	Output: 2

**Constraints:**

-   `1 <= coins.length <= 12`
-   `1 <= coins[i] <= 231  - 1`
-   `0 <= amount <= 104`

**Solution:**

```go
func coinChange(coins []int, amount int) int {
    if amount == 0 { return 0 }
    dp := make([]int,amount+1)
    for i := 0; i <= amount; i++ {
        dp[i] = amount + 1
    }
    
    for i := 1; i <= amount; i++ {
        for _,c := range coins {
            if i == c {
                dp[i] = 1
            }else if i - c >= 0 {
                if v := dp[c] + dp[i-c]; v < dp[i]  {
                    dp[i] = v
                }
            }
        }
    }
    
    if dp[amount] == amount + 1 {
        return -1
    }
    return dp[amount]
}
```