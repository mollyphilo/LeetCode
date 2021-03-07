# Implement Trie (Prefix Tree)
Implement a trie with  `insert`,  `search`, and  `startsWith`  methods.

**Example:**

	Trie trie = new Trie();

	trie.insert("apple");
	trie.search("apple");   // returns true
	trie.search("app");     // returns false
	trie.startsWith("app"); // returns true
	trie.insert("app");   
	trie.search("app");     // returns true

**Note:**

-   You may assume that all inputs are consist of lowercase letters  `a-z`.
-   All inputs are guaranteed to be non-empty strings.

**Solution:**
```go
type Trie struct {
    children []*Trie
    isEndOfWord bool
}


/** Initialize your data structure here. */
func Constructor() Trie {
    return Trie{children: make([]*Trie,26), isEndOfWord: false}
}


/** Inserts a word into the trie. */
func (this *Trie) Insert(word string)  {
    trie := this
    for i := 0; i < len(word); i++ {
        if trie.children[word[i]-'a'] == nil {
            trie.children[word[i]-'a'] = &Trie{children: make([]*Trie,26), isEndOfWord: false}
        }
        trie = trie.children[word[i]-'a']
    }
    trie.isEndOfWord = true
}


/** Returns if the word is in the trie. */
func (this *Trie) Search(word string) bool {
    trie := this
    for i := 0; i < len(word); i++ {
        if trie.children[word[i]-'a'] == nil {
            return false
        }
        trie = trie.children[word[i]-'a']
    }
    
    return trie.isEndOfWord
}


/** Returns if there is any word in the trie that starts with the given prefix. */
func (this *Trie) StartsWith(prefix string) bool {
    trie := this
    for i := 0; i < len(prefix); i++ {
        if trie.children[prefix[i]-'a'] == nil {
            return false
        }
        trie = trie.children[prefix[i]-'a']
    }
    return true
}


/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */
```