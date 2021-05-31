# Search Suggestions System
Given an array of strings  `products`  and a string  `searchWord`. We want to design a system that suggests at most three product names from  `products` after each character of `searchWord`  is typed. Suggested products should have common prefix with the searchWord. If there are more than three products with a common prefix return the three lexicographically minimums products.

Return  _list of lists_  of the suggested  `products`  after each character of `searchWord`  is typed.

**Example 1:**

	Input: products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"
	Output: [
	["mobile","moneypot","monitor"],
	["mobile","moneypot","monitor"],
	["mouse","mousepad"],
	["mouse","mousepad"],
	["mouse","mousepad"]
	]
	Explanation: products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"]
	After typing m and mo all products match and we show user ["mobile","moneypot","monitor"]
	After typing mou, mous and mouse the system suggests ["mouse","mousepad"]

**Example 2:**

	Input: products = ["havana"], searchWord = "havana"
	Output: [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]

**Example 3:**

	Input: products = ["bags","baggage","banner","box","cloths"], searchWord = "bags"
	Output: [["baggage","bags","banner"],["baggage","bags","banner"],["baggage","bags"],["bags"]]

**Example 4:**

	Input: products = ["havana"], searchWord = "tatiana"
	Output: [[],[],[],[],[],[],[]]

**Constraints:**

-   `1 <= products.length <= 1000`
-   There are no repeated elements in `products`.
-   `1 <= Σ products[i].length <= 2 * 10^4`
-   All characters of  `products[i]`  are lower-case English letters.
-   `1 <= searchWord.length <= 1000`
-   All characters of  `searchWord` are lower-case English letters.

**Solution:**

Build a trie and doing preorder search on trie (dfs) to get 3 words for each prefix

```go
type Node struct {
    isWord bool
    children []*Node
}

func insertToTrie(root *Node, w string) *Node {
    if root == nil { return nil }
    if root.children == nil {
        root.children = make([]*Node,26)
    }
    node := root
    for i := 0; i < len(w); i++ {
        if node.children[w[i]-'a'] == nil {
            node.children[w[i]-'a'] = &Node{children: make([]*Node,26)}
        }
        node = node.children[w[i]-'a']
    }
    // mark a completed word
    node.isWord = true
    
    return root
}

func search(root *Node, prefix string) []string {
    if root == nil { return  make([]string,0) }
    node := root
    for i := 0; i < len(prefix); i++ {
        if node.children[prefix[i]-'a'] == nil {
            return make([]string,0)
        }
        node = node.children[prefix[i]-'a']
    }
    
    var res []string
    dfs(node,prefix,&res)
    return res
}

func dfs(node *Node, word string, res *[]string) {
    if len(*res) == 3 {
        return
    }
    if node.isWord {
        *res = append(*res,word)
    }
    for i := 0; i < 26; i++ {
        if node.children[i] != nil {
            dfs(node.children[i], word + string('a'+i),res)
        }
    }
}

func suggestedProducts(products []string, searchWord string) [][]string {
    root := &Node{children: make([]*Node,26)}
    for _,w := range products {
        root = insertToTrie(root,w)
    }
    
    var res [][]string
    
    for i := 0; i < len(searchWord); i++ {
        res = append(res,search(root,searchWord[0:i+1]))
    }
    
    return res
}
```