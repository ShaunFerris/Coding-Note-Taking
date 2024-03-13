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
The above approach can also be simply modified to give return the length, along with all possible common substrings of that length as a set:
```python
def lcs(strx: str, stry: str) -> Tuple[int, Set]:
    """
    This is the naive solution, in O(n^3) time
    It generates every possible substring of one input and checks if it is a substring of the other
    This version is modified to give the length and a set of all possible substrings of that length
    """

    result = 0
    strings = set()

    for i in range(len(strx)):
        for j in range(len(stry)):
            k = 0
            while (
                (i + k) < len(strx)
                and (j + k) < len(stry)
                and strx[i + k] == stry[j + k]
            ):
                k += 1
            if k > result:
                strings = set()
                strings.add(strx[i : i + k])
                result = k
            if k == result:
                strings.add(strx[i : i + k])
    return (result, strings)
```
There is also this more compact implementation of the same idea using itertools:
```python
from itertools import combinations

def longest_common_substring(strx: str, stry: str):
	substrings = [str1[i:j] for i, j in combinations(range(len(str1) + 1), 2) if str1[i:j] in str2]
    return max(substrings, key=len)
```
But despite being a pretier implementation it still has a shit runtime of O(m^2 * n).

## The dynamic approach
This is generally the best way to solve this problem, and is strictly the best if you need to handle long strings. It gets the runtime down to O(n * m) the lowest that can be achieved for this problem. 

The dynamic approach saves computation by encoding the length of an identified common substring and its location in both of the input strings into a matrix, in such a way that the length of any subsequent string is calculated from the previously calculated common substring.

First the matrix that will model the position of substrings common to both ur-strings is established, with 0s at every position. The signifigance of the 0s will be covered later on.
```python
suffix_matrix = [[0 for j in string_b] for i in string_a]
```

For input strings `stra, strb = "ab", "bab"`, the matrix will look like this:
![[Pasted image 20240312211345.png]]
Notice that the outer layer of the matrix does not correspond to any place in either string