# Stone Game VII

Alice and Bob take turns playing a game, with  **Alice starting first**.

There are  `n`  stones arranged in a row. On each player's turn, they can  **remove**  either the leftmost stone or the rightmost stone from the row and receive points equal to the  **sum**  of the remaining stones' values in the row. The winner is the one with the higher score when there are no stones left to remove.

Bob found that he will always lose this game (poor Bob, he always loses), so he decided to  **minimize the score's difference**. Alice's goal is to  **maximize the difference**  in the score.

Given an array of integers  `stones`  where  `stones[i]`  represents the value of the  `ith`  stone  **from the left**, return  _the  **difference**  in Alice and Bob's score if they both play  **optimally**._

**Example 1:**

	Input: stones = [5,3,1,4,2]
	Output: 6
	Explanation: 
	- Alice removes 2 and gets 5 + 3 + 1 + 4 = 13 points. Alice = 13, Bob = 0, stones = [5,3,1,4].
	- Bob removes 5 and gets 3 + 1 + 4 = 8 points. Alice = 13, Bob = 8, stones = [3,1,4].
	- Alice removes 3 and gets 1 + 4 = 5 points. Alice = 18, Bob = 8, stones = [1,4].
	- Bob removes 1 and gets 4 points. Alice = 18, Bob = 12, stones = [4].
	- Alice removes 4 and gets 0 points. Alice = 18, Bob = 12, stones = [].
	The score difference is 18 - 12 = 6.

**Example 2:**

	Input: stones = [7,90,5,1,100,10,10,2]
	Output: 122

**Constraints:**

-   `n == stones.length`
-   `2 <= n <= 1000`
-   `1 <= stones[i] <= 1000`

**Solution:**

The problem asks to find max difference for Alice while allow Bob to have max score too

Base cases:
- [1]: 0
- [1,2]: max(2-0,1-0) // bob's turn
- [1,2,3]: max([2,3],[1,2])

Let `dp[i][j]` be the max difference that one gains from picking stones in range [i,j] inclusively. 

If Alice picks the left most, `dp[i][j] = sumStones[i+1,j] - dp[i+1,j]` because Bob will take the max in range [i+1,j]. 

If Alice picks the right most, `dp[i][j] = sumStones[i,j-1] - dp[i,j-1]`. 

Therefore, the max differences for Alice is `dp[i][j] = max(sumStones[i+1,j] - dp[i+1,j], sumStones[i,j-1] - dp[i,j-1])` 

```go
func stoneGameVII(stones []int) int {
    n := len(stones)
    // transform: stones[i] = sum of stones in range [0,i]
    for i := 1 ; i < n; i++ {
        stones[i] += stones[i-1]
    }
    getSum := func(i,j int) int {
        if i > 0 { return stones[j] - stones[i-1] }
        return stones[j]
    }
    // dp[i][j] = max diff in range [i,j]
    dp := make([][]int,n)
    for i := 0; i < n; i++ {
        dp[i] = make([]int,n)
    }
    for size := 2; size <= n; size++ {
        for i := 0; i <= n-size; i++ {
            j := i+size-1
            // substract because Bob will take best in range [start,end]
            dp[i][j] = max(getSum(i+1,j) - dp[i+1][j], getSum(i,j-1) - dp[i][j-1])
        }
    }
    return dp[0][n-1]
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```

Further optimize with O(N) space complexity for dp

```go
func stoneGameVII(stones []int) int {
    n := len(stones)
    // transform: stones[i] = sum of stones in range [0,i]
    for i := 1 ; i < n; i++ {
        stones[i] += stones[i-1]
    }
    getSum := func(i,j int) int {
        if i > 0 { return stones[j] - stones[i-1] }
        return stones[j]
    }
    // for a given length dp[i] = max diff of range length, starting from index i
    dp := make([]int,n)
    for size := 2; size <= n; size++ {
        for i := 0; i <= n-size; i++ {
            j := i+size-1
            // substract because Bob will take best in range [start,end]
            dp[i] = max(getSum(i+1,j) - dp[i+1], getSum(i,j-1) - dp[i])
        }
    }
    return dp[0]
}

func max(a,b int) int {
    if a > b { return a }
    return b
}
```