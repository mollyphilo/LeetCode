# Short Encoding of Words
A  **valid encoding**  of an array of  `words`  is any reference string  `s`  and array of indices  `indices`  such that:

-   `words.length == indices.length`
-   The reference string  `s`  ends with the  `'#'`  character.
-   For each index  `indices[i]`, the  **substring**  of  `s`  starting from  `indices[i]`  and up to (but not including) the next  `'#'`  character is equal to  `words[i]`.

Given an array of  `words`, return  _the  **length of the shortest reference string**_ `s` _possible of any  **valid encoding**  of_ `words`_._

**Example 1:**

	Input: words = ["time", "me", "bell"]
	Output: 10
	Explanation: A valid encoding would be s = `"time#bell#" and indices = [0, 2, 5`].
	words[0] = "time", the substring of s starting from indices[0] = 0 to the next '#' is underlined in "time#bell#"
	words[1] = "me", the substring of s starting from indices[1] = 2 to the next '#' is underlined in "time#bell#"
	words[2] = "bell", the substring of s starting from indices[2] = 5 to the next '#' is underlined in "time#bell#"

**Example 2:**

	Input: words = ["t"]
	Output: 2
	Explanation: A valid encoding would be s = "t#" and indices = [0].

**Constraints:**

-   `1 <= words.length <= 2000`
-   `1 <= words[i].length <= 7`
-   `words[i]`  consists of only lowercase letters.

**Solutions:**

s is shortest when it contains only words which are not a suffix of any words in the list. we can find such words by 2 ways:
- put all words in a set and iterate each word and remove substring from index i to end
	```go
	func minimumLengthEncoding(words []string) int {
	    set := make(map[string]struct{})
	    for _,w := range words {
	        set[w] = struct{}{}
	    }
	    for _,w := range words {
	        for i := 1; i < len(w); i++ {
	            delete(set,w[i:])
	        }
	    }
	    var ans int
	    for w := range set {
	        ans += len(w) + 1
	    }
	    return ans
	}
	```
	Time Complexity: O(N*M) where M is max word length and N is number of words
	Space Complexity: O(N) where N is the number of words
- build a trie with reversed words -> all words with same suffix will belong to one branch
	```go
	type TrieNode struct {
	    children []*TrieNode
	    count int
	}

	func NewTrie() *TrieNode { return &TrieNode{children: make([]*TrieNode,26), count: 0} }
	func (node *TrieNode) get(c uint8) *TrieNode {
	    if node.children[c-'a'] == nil {
	        node.children[c-'a'] = NewTrie()
	        node.count += 1
	    }
	    return node.children[c-'a']
	}

	func minimumLengthEncoding(words []string) int {
	    // hash table of TreeNode and the index of its character
	    ht := make(map[*TrieNode]int)
	    trie := NewTrie()
	    var cur *TrieNode
	    for idx,w := range words {
	        cur = trie
	        for i := len(w)-1; i >= 0; i-- {
	            cur = cur.get(w[i])
	        }
	        ht[cur] = idx
	    }
	    var ans int
	    for trie,idx := range ht {
	        // no prefix == is not suffix of something
	        if trie.count == 0 {
	            ans += len(words[idx]) + 1    
	        }
	    }
	    return ans
	}
	```
	Time Complexity: O(NxM) time to build the trie
	
	Space Complexity: O(K) where K is all possible TrieNode

