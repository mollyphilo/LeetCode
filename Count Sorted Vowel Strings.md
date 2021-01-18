# Count Sorted Vowel Strings
Given an integer  `n`, return  _the number of strings of length_ `n` _that consist only of vowels (_`a`_,_ `e`_,_ `i`_,_ `o`_,_ `u`_) and are  **lexicographically sorted**._

A string  `s`  is  **lexicographically sorted**  if for all valid  `i`,  `s[i]`  is the same as or comes before  `s[i+1]`  in the alphabet.

**Example 1:**

**Input:** n = 1
**Output:** 5
**Explanation:** The 5 sorted strings that consist of vowels only are `["a","e","i","o","u"].`

**Example 2:**

**Input:** n = 2
**Output:** 15
**Explanation:** The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.

**Example 3:**

**Input:** n = 33
**Output:** 66045

**Constraints:**

-   `1 <= n <= 50`

**Solutions:**

With n = 1, the number of strings start with `["a","e","i","o","u"]` are countVowels = [5,4,3,2,1], respectively. 
With n = 2, "a" can be appended to the front of all strings resulted from n = 1, so countVowels[0] = sum([5,4,3,2,1]). For "e", countVowels[1] = sum([4,3,2,1]), and continuously doing that we can find the number of vowels starts with "i", "o", and "u"

```python
class Solution(object):
  def countVowelStrings(self, n):
    """
    :type n: int
    :rtype: int
    """
    if n == 0:
        return 0
    elif n == 1:
        return 5
    vowels = [1,1,1,1,1]
    for i in range(2,n+1):
        temp = [0]*5
        temp[0] = sum(vowels)
        for i in range(1,5):
            temp[i] = temp[i-1] - vowels[i-1]
        vowels = temp
        
    return sum(vowels)
```