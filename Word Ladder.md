# Word Ladder
Given two words beginWord and endWord, and a dictionary wordList, return the length of the shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time.
Each transformed word must exist in the word list.
Return 0 if there is no such transformation sequence.

 

Example 1:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog", return its length 5.
Example 2:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
 

Constraints:

1 <= beginWord.length <= 100
endWord.length == beginWord.length
1 <= wordList.length <= 5000
wordList[i].length == beginWord.length
beginWord, endWord, and wordList[i] consist of lowercase English letters.
beginWord != endWord
All the strings in wordList are unique.

**Solution:**

This problem asks to search for a shortest path from undirected and unweighted set of words. Essentially this is a graph problem. We need to quickly check all the neighbors and find the shortest path requirement, so we can solve this with Breadth First Search. 

1. Pre-process words in wordList to find adjacent nodes which differ by one letter. This helps us not to iterate every word in wordList over and over again to search for adjacent nodes. We save results in a dictionary, which key is word with a letter replaced by special character, e.g. '*'. For example,  dog and hog are one letter away, so the dictionary will store them as map['*og'] = [dog, hog]

1. Push a tuple containing `beginWord` and level `1` in queue. 

1. To prevent cycles, use a visited dictionary

1. While the queue is not empty, pop element out and look for its neighbors

1. If a word is `endWord`, increased level by 1 and return. Else, push unvisited words to queue with level increased by 1


```go
type tuple struct {
    Word string
    Level int
}

func ladderLength(beginWord string, endWord string, wordList []string) int {
    L,N := len(beginWord), len(wordList)
    // pre-processing each word in wordList to find their 1-character-distanced tranformation
    adjacentWords := make(map[string][]string) 
    for i := 0; i < N; i++ {
        word := wordList[i]
        for j := 0; j < L; j++ {
            key := word[:j] + "*" + word[j+1:]
            adjacentWords[key] = append(adjacentWords[key], word)
        }
    }
    
    // BFS queue 
    queue := []tuple{{beginWord,1}}

    // To prevent cycles, use a visited dictionary.
    visited := map[string]struct{}{beginWord:struct{}{}}

    for len(queue) > 0 {
        t := queue[len(queue) -1]
        queue = queue[:len(queue)-1]
        
        for j := 0; j < L; j++ {
            key := t.Word[:j] + "*" + t.Word[j+1:]
            for _,w := range adjacentWords[key] {
                if w == endWord {
                    return t.Level + 1
                }
                if _,ok := visited[w]; !ok {
                    visited[w] = struct{}{}
                    queue = append(queue, tuple{w,t.Level+1})
                }    
            }
        }
    }
    
    return 0
}
```

**Time Complexity:** O(M^2 x N) where M is length of each word and N is size of wordList. It takes O(M x N) iterations to forms adjacent dictionary and O(M) time to  for each intermediate key. Same in BFS algo,

**Space Complexity:** O(M^2 x N) where M is length of each word and N is size of wordList. Each word has M keys, so we need a space of size MxM for all of these keys. As we have N words, the total space is O(M^2 x N). `visited` dictionary needs O(M x N) space as there are at most N words, each word needs M space. Queue for BFS would need a space for all N words in the worst cas, which results in a space complexity of O(M x N)

**Solution with bidirectional BFS**

```go
type tuple struct {
    Word string
    Level int
}

type Solution struct {
    adjacentWords map[string][]string
    m,n int
}

func ladderLength(beginWord string, endWord string, wordList []string) int {
    var isEndWordExist bool
    for _,w := range wordList {
        if w == endWord {
            isEndWordExist = true
        }
    }
    if !isEndWordExist {
        return 0
    }
     L,N := len(beginWord), len(wordList)
    // pre-processing each word in wordList to find their 1-character-distanced tranformation
    adjacentWords := make(map[string][]string) 
    for i := 0; i < N; i++ {
        word := wordList[i]
        for j := 0; j < L; j++ {
            key := word[:j] + "*" + word[j+1:]
            adjacentWords[key] = append(adjacentWords[key], word)
        }
    }
    
    s := Solution{
        m: L,
        n: N,
        adjacentWords: adjacentWords,
    }
    
    beginQueue := []tuple{{beginWord,1}}
    endQueue := []tuple{{endWord,1}}
    
    beginVisited := map[string]int{beginWord: 1}
    endVisited := map[string]int{endWord: 1}
    
    for len(beginQueue) > 0 && len(endQueue) > 0 {
        if ans := s.visitWord(&beginQueue, beginVisited,endVisited); ans > -1 {
            return ans
        }
        
        if ans := s.visitWord(&endQueue, endVisited, beginVisited); ans > -1 {
            return ans
        }
    }
    
    return 0
}

func (s *Solution) visitWord(q *[]tuple, visited,otherVisited map[string]int) int {
    t := (*q)[0]
    *q = (*q)[1:]
    
    for i := 0; i < s.m; i++ {
        key := t.Word[:i] + "*" + t.Word[i+1:]
        
        for _,w := range s.adjacentWords[key] {
            if val,found := otherVisited[w]; found {
                return t.Level + val
            }
            
            if _, found := visited[w]; !found {
                visited[w] = t.Level + 1
                *q = append(*q, tuple{w,t.Level + 1})
            }
        }
    }
    
    return -1 
}
```