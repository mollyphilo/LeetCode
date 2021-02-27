# Best Time to Buy and Sell Stock with Transaction Fee
Your are given an array of integers  `prices`, for which the  `i`-th element is the price of a given stock on day  `i`; and a non-negative integer  `fee`  representing a transaction fee.

You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)

Return the maximum profit you can make.

**Example 1:**  

	Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
	Output: 8
	Explanation: The maximum profit can be achieved by:
	-   Buying at prices[0] = 1
	-   Selling at prices[3] = 8
	-   Buying at prices[4] = 4
	-   Selling at prices[5] = 9
	The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

**Note:**

-   `0 < prices.length <= 50000`.
-   `0 < prices[i] < 50000`.
-   `0 <= fee < 50000`.

**Solution:**

Similar to problem [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/), we keep track of best buy and best sell

Let buy[i] be the max profit till date i. The series of transaction ends with a buy at i

Let sell[i] be the max profit till date i with the series of transactions ending with a sell at i

Buy at i either means taking a rest since the buy at i-1, or sell before/at i-1 then buy at i

Sell at i either means taking a rest since the sell at i-1, or buy at/before i-1 and sell at i

In code they mean:

	buy[i] = max(buy[i-1], sell[i-1]-prices[i])
	sell[i] = max(sell[i-1], buy[i-1]+prices[i]-fee)

Since we only care about buy and sell at dates `i-1` and `i`, we only need 4 variables to keep track of them

```go
func maxProfit(prices []int, fee int) int {
    var b0,b1,s0,s1 int = -prices[0],-prices[0],0,0
    
    for i := 1; i < len(prices); i++ {
        b0 = max(b1,s1 - prices[i])
        s0 = max(s1,b1+prices[i]-fee)
        
        s1 = s0
        b1 = b0
    }
    return s0
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```