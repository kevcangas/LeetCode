# 2981. Find Longest Special Substring That Occurs Thrice I

A string is called **special** if it is made up of only a single character. For example, the string "abc" is not special, whereas the strings "ddd", "zz", and "f" are special.

Return the length of the **longest special substring** of s which occurs **at least thrice**, or -1 if no special substring occurs at least thrice.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

## Examples

**Example 1:**

**Input:** s = "aaaa"
**Output:** 2
**Explanation:** The longest special substring which occurs thrice is "aa": substrings "aaaa", "aaaa", and "aaaa".
It can be shown that the maximum length achievable is 2.

**Example 2:**

**Input:** s = "abcdef"
**Output:** -1
**Explanation:** There exists no special substring which occurs at least thrice. Hence return -1.

**Example 3:**

**Input:** s = "abcaba"
**Output:** 1
**Explanation:** The longest special substring which occurs thrice is "a": substrings "abcaba", "abcaba", and "abcaba".
It can be shown that the maximum length achievable is 1.

## Constraints:

3 <= s.length <= 50
s consists of only lowercase English letters.

## Solution

```python
class Solution:
    def maximumLength(self, s: str) -> int:
        
        count = {}

        for start in range(len(s)):
            curr_string = []

            # This loop starts depending on the variable 'start' 
            for end in range(start, len(s)):

                # This condition verify if the new character is equal or not
                # the current characters analyzed
                if not curr_string or curr_string[-1] == s[end]:
                    curr_string.append(s[end])

                    # This cast the variable curr_string to a string
                    curr_to_string = "".join(curr_string)

                    #This section save the information in the count dictionary
                    if curr_to_string in count:
                        count[curr_to_string] += 1
                    else:
                        count[curr_to_string] = 1
                
                # If the current character differs to the last character, the loop breaks
                else:
                    break
            
            # This section searches the longest substring in the count dictionary
            # and verify if the substring occurs thrice
        ans = 0
        for str, freq in count.items():
            if freq >= 3 and len(str) > ans:
                ans = len(str)
        
        if ans == 0:
            return -1
            
        return ans
```