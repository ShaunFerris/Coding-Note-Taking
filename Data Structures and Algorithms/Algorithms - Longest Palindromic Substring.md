#dsa #algorithms #problemsolutions 

This problem is similar to the longest common substring of two strings problem, and can be solved dynamically in a similar but different way. See here for a dicsussion of that problem: [[Algorithms - Longest Common Substring]].

## The naive, bruteforce approach
The simple and obvious approach to this problem is to brute force it, by looking at every single possible substring of the input and checking if it is a palindrome.

To generate every possible substring, we can use a nested loop structure like this:
```python
for i in range(len(input)):
	for j in range(i, len(input)):
		pass
```
This will take O(n^2) time. But we also have to check if the current substring is a palindrome. This requires a full scan of the length of the substring, so it will complete in O(n) time. This gives us a total runtime of O(n^3). 

The bruteforce solution is implemented in python like this:
```python
def bruteforce_lps(input: str) -> str:
    if len(input) < 2:
        return input
    else:
        longest = ""
        for i in range(len(input)):
            for j in range(len(input)):
                if input[i : j + 1] == input[i : j + 1][::-1] and len(
                    input[i : j + 1]
                ) > len(longest):
                    longest = input[i : j + 1]
        return longest


test = "aacabdkacaa"
print(bruteforce_lps(test))
```
This solution works for all test cases, but is slow as shit.

## The expanding window approach - using two pointers
We can improve the runtime of the above solution by simply changing the way that we check if something is or is not palindromic. This approach will give us a solution with runtime O(n^2).

Consider the palindrome `"bab"`. Rather than checking that it is a palindrome by scanning from index 0 to the end, or using the string reverse slice like in the above example, we could start at the outside. If both outside characters are the same, and continue to be the same until we get to the middle, then it is a palindrome.

Equally, we could start from the middle and go outwards. This has the added benefit of quickly exluding chars at the ends of the string as being the center of a palindrome. The only issue is that we will need to handle the edge case of even length palindromes, because they will not have a cleanly centered pivot to compare around. 

<span style="color: cyan; font-weight: bold;">This is the approach we will use here, loop through the string, and for each character consider it the center of a theoretical substring. Expand outwards from the center and check if there is a character on each side, if there is are they the same? If so, this is a palindrome, record it if it is the largest, and continue expanding.

For each character we are considering it the middle of a substring and expanding out up to n times. This gives us runtime of O(n^2), a significant improvement!</span>

### Implementation of the middle out approach
The simplified course of the algorithm:
1. iterate through the input string
2. check for an odd length palindrome by scanning out from i with a left and right pointer
3. check for an even length palindrome by scanning out from i and i +1 with a left and right pointer
When implemented it looks something like this:
```python
def middleout_lps(input: str) -> str:
    longest = ""

    for i in range(len(input)):
        left, right = i, i  # this handles odd length palindromes with a center pivot
        while left >= 0 and right < len(input) and input[left] == input[right]:
            if len(input[left : right + 1]) > len(longest):
                longest = input[left : right + 1]
            left -= 1
            right += 1

        left, right = (
            i,
            i + 1,
        )  # this handles even length palindromes by starting the pointers staggered
        while left >= 0 and right < len(input) and input[left] == input[right]:
            if len(input[left : right + 1]) > len(longest):
                longest = input[left : right + 1]
            left -= 1
            right += 1
    return longest
```