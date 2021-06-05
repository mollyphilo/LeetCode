# Integer to English Words

Convert a non-negative integer  `num`  to its English words representation.

**Example 1:**

    Input: num = 123
    Output: "One Hundred Twenty Three"

**Example 2:**

    Input: num = 12345
    Output: "Twelve Thousand Three Hundred Forty Five"

**Example 3:**

    Input: num = 1234567
    Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"

**Example 4:**

    Input: num = 1234567891
    Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"

**Constraints:**

-   `0 <= num <= 2^31  - 1`

**Solution:**

For number less than 100, their English words are the words of quotients and the words of remainder, which we can store in a hashmap
For number from 100 to 999, their English words are followed by "Hundred"
For number from 1000, we can recursively find the quotients and remainders from the division of billion, million and thousand, then convert them to English words followed by unit "billion", "million" or "thousand", respectively.
Since the max value of num is 2^31 - 1, the words can only go up to "billion". 

```go
import "strings"

var n2s map[int]string = map[int]string{
    0: "Zero", 1: "One", 2: "Two", 3: "Three", 4: "Four", 5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine",
    10: "Ten", 11: "Eleven", 12: "Twelve", 13: "Thirteen",14:"Fourteen",15:"Fifteen",16:"Sixteen",17: "Seventeen",18: "Eighteen",19: "Nineteen",
    20: "Twenty",30:"Thirty",40:"Forty",50:"Fifty",60:"Sixty",70:"Seventy",80:"Eighty",90: "Ninety",100:"One Hundred",
}

func numberToWords(num int) string {
    if s,found := n2s[num]; found { return s }
    
    var ans []string

    n := num/1000000000
    num = num%1000000000
    if n > 0 {
        ans = append(ans,num2words(n)...)
        ans = append(ans,"Billion")
    }
    
    n = num/1000000
    num = num%1000000
    if n > 0 {
        ans = append(ans,num2words(n)...)
        ans = append(ans,"Million")
    }
    
    n = num/1000
    num = num%1000
    if n > 0 {
        ans = append(ans,num2words(n)...)
        ans = append(ans,"Thousand")
    }
    if num > 0 {
        ans = append(ans,num2words(num)...)
    }

    return strings.Join(ans," ")
}

func num2words(num int) []string {
    if s,found := n2s[num]; found {
        return []string{s}
    }
    
    var ans []string
    if num > 100 {
        n := num/100
        num = num%100
        if n > 0 {
            ans = append(ans,num2words(n)...)
            ans = append(ans,"Hundred")
        }
    }
    if num == 0 { return ans }
    
    if s,found := n2s[num]; found {
        return append(ans,s)
    }

    mod := num%10
    num -= mod

    return append(ans,n2s[num],n2s[mod])
}
```