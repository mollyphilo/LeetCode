
# Best Time to Buy and Sell Stock with Cooldown
Say you have an array for which the  _i_th  element is the price of a given stock on day  _i_.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

-   You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
-   After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

**Example:**

	Input: [1,2,3,0,2]
	Output: 3 
	Explanation: transactions = [buy, sell, cooldown, buy, sell]

**Solution:**
Let buy[i] be the max profit till date i. The series of transaction ends with a buy at i
Let sell[i] be the max profit till date i with the series of transactions ending with a sell at i

Buy at i either means taking a rest since the buy at i-1, or sell before/at i-2 then buy at i
Sell at i either means taking a rest since the sell at i-1, or buy at/before i-1 and sell at i

In code they mean:
buy[i] = max(buy[i-1], sell[i-2]-prices[i])
sell[i] = max(sell[i-1], buy[i-1]+prices[i])

Since all indices one iteration care about is i-2,i-1 and i, let represent them with variables instead of array of buy and sell:
b1,b0 = buy[i-1],buy[i] // buy[i-2] ain't used in the algorithm so it is not needed
s2,s1,s0 = sell[i-2],sell[i-1],sell[i]

Their initial values are as followed:
b1 = b0 = -prices[0] // at 0 a stock can only be bought
s2 = s1 = s0 = 0 // does not have anything to sell at 0

```go
func maxProfit(prices []int) int {
    n := len(prices)
    if n <= 1 {
        return 0
    }
    var b1,b0,s2,s1,s0 int = -prices[0],-prices[0],0,0,0
    
    for i := 1; i < n; i++ {
        b0 = max(b1,s2-prices[i])
        s0 = max(s1, b1 + prices[i])
        
        b1,s2,s1 = b0,s1,s0
    }
    return s0
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```