
# 309. Best Time to Buy and Sell Stock with Cooldown

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

```
buy[i] = max(buy[i-1], sell[i-2]-prices[i])
sell[i] = max(sell[i-1], buy[i-1]+prices[i])
```

Since all indices one iteration care about is i-2,i-1 and i, let represent them with variables instead of array of buy and sell:
```
b1,b0 = buy[i-1],buy[i] // buy[i-2] ain't used in the algorithm so it is not needed
s2,s1,s0 = sell[i-2],sell[i-1],sell[i]
```

Their initial values are as followed:

```
b1 = b0 = -prices[0] // at 0 a stock can only be bought
s2 = s1 = s0 = 0 // does not have anything to sell at 0
```

Here is the code:

```go
/*
b0,b1
s0,s1

b0 = -prices[0]
s0 = 0
// option: buy,sell,do nothing
s1 = max(s0, b0 + prices[i])
b1 = max(b0, sp - prices[i])
b0,sp,s0 = b1,s0,s1
*/
func maxProfit(prices []int) int {
    n := len(prices)
    // i = 0
    b0,s0 := -prices[0],0
    var b1,sp,s1 int

    for i := 1; i < n; i++ {
        s1 = max(s0, b0 + prices[i])
        b1 = max(b0, sp - prices[i])
        b0,sp,s0 = b1,s0,s1    
    }
    
    return max(b1,s1)
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```