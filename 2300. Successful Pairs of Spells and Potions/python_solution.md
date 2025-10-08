# 2300. Successful Pairs of Spells and Potions

You are given two positive integer arrays `spells` and `potions`, of length `n` and `m` respectively, where `spells[i]` represents the strength of the `i<sup>th</sup>` spell and `potions[j]` represents the strength of the `j<sup>th</sup>` potion.

You are also given an integer `success`. A spell and potion pair is considered successful if the product of their strengths is at least `success`.

Return an integer array `pairs` of length `n` where `pairs[i]` is the number of potions that will form a successful pair with the `i<sup>th</sup>` spell.

## Examples

**Example 1:**

> **Input:** spells = [5,1,3], potions = [1,2,3,4,5], success = 7
> **Output:** [4,0,3]
> **Explanation:**
> - 0th spell: 5 * [1,2,3,4,5] = [5,**10**,**15**,**20**,**25**]. 4 pairs are successful.
> - 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
> - 2nd spell: 3 * [1,2,3,4,5] = [3,6,**9**,**12**,**15**]. 3 pairs are successful.
> Thus, [4,0,3] is returned.

## Constraints:

- `n == spells.length`
- `m == potions.length`
- `1 <= n, m <= 105`
- `1 <= spells[i], potions[i] <= 105`
- `1 <= success <= 1010`

## Solution

```python
class Solution:
    def successfulPairs(self, spells: List[int], potions: List[int], success: int) -> List[int]:
        
        n = len(spells)
        m = len(potions)
        pairs = []
        potions.sort()

        for i in range(n):

            potion = math.ceil(success / spells[i])

            if potions[-1] >= potion:
                j = bisect.bisect_left(potions, potion)
                pairs.append(m-j)
                        
            else:
                pairs.append(0)
        
        return pairs
```