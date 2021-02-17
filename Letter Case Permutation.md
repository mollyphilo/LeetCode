# Letter Case Permutation

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.

Return  _a list of all possible strings we could create_. You can return the output in  **any order**.

**Example 1:**

	Input: S = "a1b2"
	Output: ["a1b2","a1B2","A1b2","A1B2"]

**Example 2:**

	Input: S = "3z4"
	Output: ["3z4","3Z4"]

**Example 3:**

	Input: S = "12345"
	Output: ["12345"]

**Example 4:**

	Input: S = "0"
	Output: ["0"]

**Constraints:**

-   `S`  will be a string with length between  `1`  and  `12`.
-   `S`  will consist only of letters or digits.

**Solution:**

```go
import "strings"

func letterCasePermutation(S string) []string {
    n := len(S)
    tb := make([]uint8,26)
    for i := 0; i < 26; i++ {
        tb[i] = uint8('A' + i)
    }
    
    lower := strings.ToLower(S)
    s := []byte(lower)
    ans := []string{lower}
    
    var upper uint8
    for i := 0; i < n; i++ {
        if s[i] >= 'a' {
            upper = tb[s[i]-'a'] 
            size := len(ans)
            for j := 0; j < size; j++ {
                temp := []byte(ans[j])
                temp[i] = upper
                ans = append(ans,string(temp))
            }
        }
    }
    return ans
}
```