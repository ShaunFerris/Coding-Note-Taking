#dsa #algorithms #arrays #problemsolutions

The sliding window technique is a common strategy for algorithms that need to identify patterns in an array of data that relate to the relative order of elements in the array. It is a more optimal method for situations where the naive approach would involve nested for loops.

A good simple example of this technique is the following problem from leetcode.com:
<blockquote style="color: cyan; font-weight: bold; font-style: italic">Given a string `s`, find the length of the longest substring without repeating characters.

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.</blockquote>
We can solve this problem by placing a window on the string that starts at index 0 and ends initially at index 1. The end of window moves with the iterations of the loop and the start of the window moves forward whenever we dont find a substring that is longer than the previous best.

Notice also how the solution uses sets to check for duplication of characters in a possible substring, by comparing the length of a set of a substring to the length of the original substring.

Here is the solution:
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        count = 0
        w_one = 0
        for w_two in range(1, len(s)+1):
            if len(s[w_one : w_two]) == len(set(s[w_one : w_two])):
                count = len(s[w_one : w_two]) if len(s[w_one : w_two]) > count else count
            else: w_one += 1
        return count
```