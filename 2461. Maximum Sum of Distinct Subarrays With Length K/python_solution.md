# 2461. Maximum Sum of Distinct Subarrays With Length K

You are given an integer array **nums** and an integer **k**. Find the maximum subarray sum of all the subarrays of **nums** that meet the following conditions:

- The length of the subarray is k, and
- All the elements of the subarray are **distinct**

Return the maximum subarray sum of all the subarrays that meet the conditions. If no subarray meets the conditions, return 0.

A **subarray** is a contiguous non-empty sequence of elements within an array.

## Examples

**Example 1:**

Input: nums = [1,5,4,2,9,9,9], k = 3
Output: 15
Explanation: The subarrays of nums with length 3 are:
- [1,5,4] which meets the requirements and has a sum of 10.
- [5,4,2] which meets the requirements and has a sum of 11.
- [4,2,9] which meets the requirements and has a sum of 15.
- [2,9,9] which does not meet the requirements because the element 9 is repeated.
- [9,9,9] which does not meet the requirements because the element 9 is repeated.
We return 15 because it is the maximum subarray sum of all the subarrays that meet the conditions

**Example 2:**

Input: nums = [4,4,4], k = 3
Output: 0
Explanation: The subarrays of nums with length 3 are:
- [4,4,4] which does not meet the requirements because the element 4 is repeated.
We return 0 because no subarrays meet the conditions.

## Solution

```python
class Solution:
    
    def maximumSubarraySum(self, nums: List[int], k: int) -> int:
        
        #Start variables
        res = 0
        count = defaultdict(int)
        cur_sum = 0

        l = 0
        for r in range(len(nums)):
            cur_sum += nums[r]
            count[nums[r]] += 1

            #Window - r represents the last position of the window, 
            #while l represents the first value of the window.
            #When the window over pass k value (windows size), 
            #the value of the nums array that is in the l position is
            #rested and if it is equal to 0, is poped from the count 
            #dictionary and rested from current sum. 
            if r - l + 1 > k:
                count[nums[l]] -= 1
                if count[nums[l]] == 0:
                    count.pop(nums[l])
                cur_sum -= nums[l]
                l += 1

            if len(count) == r - l + 1 == k:
                res = max(res, cur_sum)

        return res
```