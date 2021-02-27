# Best Time to Buy and Sell Stock III
Say you have an array for which the  _i_th  element is the price of a given stock on day  _i_.

Design an algorithm to find the maximum profit. You may complete at most  _two_  transactions.

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**

	Input: prices = [3,3,5,0,0,3,1,4]
	Output: 6
	Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
	Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

**Example 2:**

	Input: prices = [1,2,3,4,5]
	Output: 4
	Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
	Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.

**Example 3:**

	Input: prices = [7,6,4,3,1]
	Output: 0
	Explanation: In this case, no transaction is done, i.e. max profit = 0.

**Example 4:**

	Input: prices = [1]
	Output: 0

**Constraints:**

-   `1 <= prices.length <= 105`
-   `0 <= prices[i] <= 105`

**Solution:**

```go
func maxProfit(prices []int) int {
	// 1st buy, 2nd buy, 1st sell, 2nd sell
    var b0,b1,s0,s1 int = -prices[0],-prices[0],0,0
    
    for i := 0; i < len(prices); i++ {
        b0 = max(b0, -prices[i])
        s0 = max(s0, prices[i]+b0)
        b1 = max(b1, s0-prices[i])
        s1 = max(s1, prices[i]+b1)
    }
    
    return s1
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```