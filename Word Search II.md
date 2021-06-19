# Word Search II

Given an  `m x n`  `board` of characters and a list of strings  `words`, return  _all words on the board_.

Each word must be constructed from letters of sequentially adjacent cells, where  **adjacent cells**  are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

	Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
	Output: ["eat","oath"]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

	Input: board = [["a","b"],["c","d"]], words = ["abcb"]
	Output: []

**Constraints:**

-   `m == board.length`
-   `n == board[i].length`
-   `1 <= m, n <= 12`
-   `board[i][j]`  is a lowercase English letter.
-   `1 <= words.length <= 3 * 104`
-   `1 <= words[i].length <= 10`
-   `words[i]`  consists of lowercase English letters.
-   All the strings of  `words`  are unique.

**Solution:**

Need to optimize

Idea: trie + dfs

```go
type Trie struct {
    children []*Trie
    isEndOfWord bool
}

func NewTrie() *Trie { return &Trie{children: make([]*Trie, 26), isEndOfWord: false}}

func (root *Trie) insert(word string) {
    node := root
    for i := 0; i < len(word); i++ {
        if node.children[word[i]-'a'] == nil {
            node.children[word[i]-'a'] = NewTrie()
        }
        node = node.children[word[i]-'a']
    }
    node.isEndOfWord = true
}

func (root *Trie) search(word string) bool {
    node := root
    for i := 0; i < len(word); i++ {
        if node.children[word[i]-'a'] == nil {
            return false
        }
        node = node.children[word[i]-'a']
    }
    return node.isEndOfWord
}
func findWords(board [][]byte, words []string) []string {
    root := NewTrie()
    // build trie for fast look up
    for _,w := range words {
        root.insert(w)
    }

    // dfs board for words
    var ans []string
    for i := 0; i < len(board); i++ {
        for j := 0; j < len(board[0]); j++ {
            dfs(board,i,j,root, &ans, []byte{board[i][j]})        
        }
    }
    
    return ans
}

func dfs(board [][]byte, i,j int, trie *Trie, ans *[]string, word []byte) {
    c := board[i][j]
    if trie == nil || c == '#' || trie.children[c-'a'] == nil  { return }
    trie = trie.children[c-'a']
    
    if trie.isEndOfWord { 
        *ans = append(*ans, string(word))
        trie.isEndOfWord = false // avoid duplication
    }
    
    board[i][j] = '#'
    if i-1 >= 0 { dfs(board, i-1,j, trie, ans, append(word, board[i-1][j])) }
    if i+1 < len(board) { dfs(board, i+1,j, trie, ans, append(word, board[i+1][j])) }
    if j-1 >= 0 { dfs(board, i,j-1, trie, ans, append(word, board[i][j-1])) }
    if j+1 < len(board[0]) { dfs(board, i,j+1, trie, ans, append(word, board[i][j+1])) }
    board[i][j] = c
}
```