# Snakes and Ladders: The Quickest Way Up

Markov takes out his  [Snakes and Ladders](http://en.wikipedia.org/wiki/Snakes_and_Ladders)  game, stares at the board and wonders: "If I can always roll the die to whatever number I want, what would be the least number of rolls to reach the destination?"

**Rules**  The game is played with a cubic die of  faces numbered  to  .

1.  Starting from square  , land on square  with the exact roll of the die. If moving the number rolled would place the player beyond square  , no move is made.
    
2.  If a player lands at the base of a ladder, the player must climb the ladder. Ladders go up only.
    
3.  If a player lands at the mouth of a snake, the player must go down the snake and come out through the tail. Snakes go down only.
    

**Function Description**

Complete the  _quickestWayUp_  function in the editor below. It should return an integer that represents the minimum number of moves required.

quickestWayUp has the following parameter(s):

-   _ladders_: a 2D integer array where each  contains the start and end cell numbers of a ladder
-   _snakes_: a 2D integer array where each  contains the start and end cell numbers of a snake

**Input Format**

The first line contains the number of tests,  .

For each testcase:  
- The first line contains  , the number of ladders.  
- Each of the next  lines contains two space-separated integers, the start and end of a ladder.  
- The next line contains the integer  , the number of snakes.  
- Each of the next  lines contains two space-separated integers, the start and end of a snake.

**Constraints**

  

The board is always  with squares numbered  to  .  
Neither square  nor square  will be the starting point of a ladder or snake.  
A square will have at most one endpoint from either a snake or a ladder.

**Output Format**

For each of the t test cases, print the least number of rolls to move from start to finish on a separate line. If there is no solution, print  `-1`.

**Sample Input**

```
2
3
32 62
42 68
12 98
7
95 13
97 25
93 37
79 27
75 19
49 47
67 17
4
8 52
6 80
26 42
2 72
9
51 19
39 11
37 29
81 3
59 5
79 23
53 7
43 33
77 21 

```

**Sample Output**

```
3
5

```

**Explanation**

_For the first test:_

The player can roll a  and a  to land at square  . There is a ladder to square  . A roll of  ends the traverse in  rolls.

_For the second test:_

The player first rolls  and climbs the ladder to square  . Three rolls of  get to square  . A final roll of  lands on the target square in  total rolls.

**Solution:**

```go
func quickestWayUp(ladders [][]int32, snakes [][]int32) int32 {
	// Write your code here
	// try all possible rolls such that
	// if cur + num is a start on the ladder or on a snake
	// then next pos is the end of the ladder/snake
	// then we compare the cost to move the end: cost[curr] + 1 vs cost[end] (if exist)
	// and we pick the smaller for cost[end]
	costs := map[int32]int32{1: 0}
	// transform ladders and snakes into map for faster lookup
	mladders, mSnakes := make(map[int32]int32), make(map[int32]int32)
	for i := 0; i < len(ladders); i++ {
		mladders[ladders[i][0]] = ladders[i][1]
	}
	for i := 0; i < len(snakes); i++ {
		mSnakes[snakes[i][0]] = snakes[i][1]
	}

	queue := []int32{1}
	var n int
	var nextPos int32
	for len(queue) > 0 {
		n = len(queue)
		for i := 0; i < n; i++ {
			pos := queue[i]
			if pos == 100 {
				return costs[pos]
			}
			for steps := int32(1); steps <= int32(6); steps++ {
				nextPos = pos + steps
				// favor ladder more than snake
				if mladders[nextPos] > 0 {
					nextPos = mladders[nextPos]
				} else if mSnakes[nextPos] > 0 {
					nextPos = mSnakes[nextPos]
				}

				// not yet visited, add nextPos to queue, else just update the cost
				if costs[nextPos] == 0 {
					costs[nextPos] = costs[pos] + 1
					queue = append(queue, nextPos)
				} else if costs[nextPos] > costs[pos]+1 {
					costs[nextPos] = costs[pos] + 1
				}
			}
		}
		queue = queue[n:]
	}
	return -1
}

```