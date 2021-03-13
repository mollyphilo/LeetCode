# Check If a String Contains All Binary Codes of Size K
Given a binary string  `s`  and an integer  `k`.

Return  _True_  if every binary code of length  `k`  is a substring of  `s`. Otherwise, return  _False_.

**Example 1:**

> **Input:** s = "00110110", k = 2
**Output:** true
**Explanation:** The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indicies 0, 1, 3 and 2 respectively.

**Example 2:**

> **Input:** s = "00110", k = 2
**Output:** true

**Example 3:**

> **Input:** s = "0110", k = 1
**Output:** true
**Explanation:** The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring. 

**Example 4:**

> **Input:** s = "0110", k = 2
**Output:** false
**Explanation:** The binary code "00" is of length 2 and doesn't exist in the array.

**Example 5:**

>**Input:** s = "0000000001011100", k = 4
**Output:** false

**Constraints:**

-   `1 <= s.length <= 5 * 10^5`
-   `s`  consists of 0's and 1's only.
-   `1 <= k <= 20`

**Solution:**

Keep a boolean array with size is 2^k. Index is all possible values from 0 to 2^k - 1. When a value in this boolan array is set to true (not count duplicated), we decrease the counter by 1. Counter starts at 2^k and counter equals 0 means that we find all substrings with value from 0 to 2^k -1.

We set bool[i] to true if a substring with length k has decimal value is i. To convert substring value to decimal, we use: `hashVal = ((hashVal << 1) & allOne)| int(s[i] - '0')`
```go
func hasAllCodes(s string, k int) bool {
    var counts,hashVal int = 1 << k, 0 // there are 2^k numbers from 0 to 2^k - 1
    got := make([]bool,counts)
    hashVal,allOne := 0,counts - 1
    for i := 0; i < len(s); i++ {
        hashVal = ((hashVal << 1) & allOne)| int(s[i] - '0')
        // i is last character of substring with k characters.
        // first character needs to be at least 0, so i-k+1 >= 0
        if i >= k-1 && !got[hashVal] {
            got[hashVal] = true
            counts--
            if counts == 0 {
                return true
            }
        }
    }
    return false
}
```