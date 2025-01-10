# 916. Word Subsets

You are given two string arrays words1 and words2.

- A string b is a subset of string a if every letter in b occurs in a including multiplicity.

For example, "wrr" is a subset of "warrior" but is not a subset of "world".
A string a from words1 is universal if for every string b in words2, b is a subset of a.

Return an array of all the universal strings in words1. You may return the answer in any order.

## Examples

### Example 1:

Input: words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["e","o"]
Output: ["facebook","google","leetcode"]

### Example 2:

Input: words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["l","e"]
Output: ["apple","google","leetcode"]

## Constraints:

- 1 <= words1.length, words2.length <= 104
- 1 <= words1[i].length, words2[i].length <= 10
- words1[i] and words2[i] consist only of lowercase English letters.
- All the strings of words1 are unique.

## Solution

```python
class Solution:
    def wordSubsets(self, words1: List[str], words2: List[str]) -> List[str]:
        
        ans = []

        # Frequency calculation of words2
        freq_words2 = {}
        for word in words2:
            current_freq_words2 = {}
            for ch in word:
                if not ch in current_freq_words2:
                    current_freq_words2[ch] = 1
                else:
                    current_freq_words2[ch] += 1
            
            for ch in word:
                if not ch in freq_words2:
                    freq_words2[ch] = current_freq_words2[ch]
                else:
                    freq_words2[ch] = max(freq_words2[ch],current_freq_words2[ch])
 
        # Frequency calculation of words1
        for word in words1:
            freq_word = {}
            
            for ch in word:
                if not ch in freq_word:
                    freq_word[ch] = 1
                else:
                    freq_word[ch] += 1
            
            universal = True
            for ch in freq_words2:
                if ch in freq_word and freq_word[ch] >= freq_words2[ch]:
                    universal = True
                else:
                    universal = False
                    break

            if universal:
                ans.append(word)
        
        return ans
```