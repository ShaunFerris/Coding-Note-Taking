#dsa #problemsolutions #algorithms 

## Problem statement
Given two strings as input, determine the longest substring that is common to both.

For example, given: `stringOne, stringTwo = "abdegdsdjkl", "gdsdjabcvcv`
Return: `"gdsdj`

## The naive approach
The simplest approach to the problem is to generate all possible substrings of one input and then check if they are substrings of the second input. This approach has runtime O(n^3), where n is the length of the longest input. This makes this approach wholely unsuitable for use on long strings.

This solution is implemented simply with nested for loops:
```python
def lcs(strx: str, stry: str) -> int:
    """
    This is the naive solution, in O(n^3) time
    It generates every possible substring of one input and checks if it is a substring of the other
    """

    result = 0

    for i in range(len(strx)):
        for j in range(len(stry)):
            k = 0
            while (
                (i + k) < len(strx)
                and (j + k) < len(stry)
                and strx[i + k] == stry[j + k]
            ):
                k += 1
            result = max(result, k)
    return result
```