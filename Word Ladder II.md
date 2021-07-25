# Word Ladder II

A  **transformation sequence**  from word  `beginWord`  to word  `endWord`  using a dictionary  `wordList`  is a sequence of words  `beginWord -> s1  -> s2  -> ... -> sk`  such that:

-   Every adjacent pair of words differs by a single letter.
-   Every  `si`  for  `1 <= i <= k`  is in  `wordList`. Note that  `beginWord`  does not need to be in  `wordList`.
-   `sk  == endWord`

Given two words,  `beginWord`  and  `endWord`, and a dictionary  `wordList`, return  _all the  **shortest transformation sequences**  from_  `beginWord`  _to_  `endWord`_, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words_ `[beginWord, s1, s2, ..., sk]`.

**Example 1:**
	
	Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
	Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
	Explanation: There are 2 shortest transformation sequences:
	"hit" -> "hot" -> "dot" -> "dog" -> "cog"
	"hit" -> "hot" -> "lot" -> "log" -> "cog"

**Example 2:**

	Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
	Output: []
	Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

**Constraints:**

-   `1 <= beginWord.length <= 5`
-   `endWord.length == beginWord.length`
-   `1 <= wordList.length <= 1000`
-   `wordList[i].length == beginWord.length`
-   `beginWord`,  `endWord`, and  `wordList[i]`  consist of lowercase English letters.
-   `beginWord != endWord`
-   All the words in  `wordList`  are  **unique**.

**Solution:**

Breadth-First Search (BFS) + Backtracking

Time complexity: O(NK^2 + alpha) where N=#words, K=len(word), alpha is the number of possible paths from beginWord to endWord in the directed graph we have.

Space complexity: O(NK)

```go
type set map[string]struct{}

func findLadders(beginWord string, endWord string, wordList []string) [][]string {
    if len(beginWord) != len(endWord) { return [][]string{} }
    graph := bfs(beginWord, wordList)
    var ans [][]string
    backtrack(beginWord,endWord, graph, &ans, []string{beginWord})
    return ans
}

// return a graph of words reachable from source
// such that there is no cyclic path
func bfs(beginWord string, wordList []string) map[string]set {
    wordSet := make(set)
    for _,w := range wordList { wordSet[w] = struct{}{} }
    delete(wordSet,beginWord)

    chars := make([]byte,26)
    for i := 0; i < 26; i++ { chars[i] = 'a' + byte(i) }
    
    q := []string{beginWord}
    var n int
    var visited set

    wlen := len(beginWord)
    
    graph := make(map[string]set)
    isEnqueued := make(set)
    
    for len(q) > 0 {
        n = len(q)
        visited = make(set)
        for i := 0; i < n; i++ {
            // find neighbor
            temp := []byte(q[i])
            for j := 0; j < wlen; j++ {
                for _,b := range chars {
                    temp[j] = b
                    w := string(temp)
                    if _,exist := wordSet[w]; exist {
                        visited[w] = struct{}{}
                        if _,ok := graph[q[i]]; !ok { graph[q[i]] = make(set) }
                        graph[q[i]][w] = struct{}{}
                        
                        if _,ok := isEnqueued[w]; !ok {
                            q = append(q, w)
                            isEnqueued[w] = struct{}{}
                        }
                    }
                    temp[j] = q[i][j]
                }
            }
        }
        
        q = q[n:]
        for v := range visited { delete(wordSet, v) }
    }
    
    return graph
}

func backtrack(begin,end string, graph map[string]set, ans *[][]string, cur []string) {
    if begin == end {
        temp := make([]string, len(cur))
        copy(temp,cur)
        *ans = append(*ans, temp)
    }
    neighbors,ok := graph[begin]
    if !ok { return }
    for n := range neighbors {
        backtrack(n,end,graph,ans,append(cur,n))
    }
}
```