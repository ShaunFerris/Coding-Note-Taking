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

First the matrix that will model the position of substrings common to both ur-strings is established, with 0s at every position. The signifigance of the 0s will be covered later on. In the below, `m` and `n` are the lengths of the input strings.
```python
suffix_matrix = [[0] * (n+1) for _ in range(m+1)]
```

For input strings `strx, stry = "ab", "bab"`, the matrix will look like this:
![[Pasted image 20240312211345.png]]
Notice that the outer layer of the matrix does not correspond to any place in either string. These cells in the matrix will always be 0, and they exist to update other cells. 

Now that we have the matrix in place, we iterate through the matrix with a nested loop. The range for i and j in n+1 and m+1 respectively, and we want to start from one so that we skip the outer layer. The loops will look like this:
```python
for i in range(1, n+1):
	for j in range(1, m+1):
		pass
```
We are comparing the string characters associated with each grid square and then using the value in the square diagonally up to the left to update the length of the longest common substring that terminates at the current position. Our first comparison will be `'a' === 'b'`, which is false, so the  cell at `matrix[i][j]` remains 0. The next comparison will be `'a' == 'a'` which is true, so we look up and to the left diagonally (aka `matrix[i-1][j-1])`), add one to the value we find (0), and make that the value of the current cell:
![[Pasted image 20240313093527.png]]
Now skipping ahead to the cell at `matrix[3][2]`, we will be comparing `'b' == 'b'`, which is true so we look diagonally up and update our cell by that value plus one:
![[Pasted image 20240313093755.png]]
This will be the final state of the matrix. You can see how each cell of the matrix represents the length of a common substring and at which position it terminates in a given input string. With the position of it's terminus and it's length, we can grab it from the string and return it.

The full python implementation of this algorithm looks like this:
```python
def dynamiclcs(strx: str, stry: str) -> int:
    """
    This is the better solution, running in O(n*m) time.
    """
    n, m = len(strx), len(stry)
    suffix_matrix = [[0] * (n + 1) for _ in range(m + 1)]
    longest = 0
    terminus = 0

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if strx[i - 1] == stry[j - 1]:
                suffix_matrix[i][j] = (suffix_matrix[i - 1][j - 1]) + 1
                if suffix_matrix[i][j] > longest:
                    longest = suffix_matrix[i][j]
                    terminus = i
            else:
                suffix_matrix[i][j] = 0
    return strx[(terminus - longest) : (terminus)]
```

The solution has O(m * n) complexity for both time and space. The space complexity is due to the memory comitment of storing the matrix. You can use the following tricks to reduce the memory comittment:
- Keep only the last and current row of the DP table to save memory ( O ( min ( r , n ) ) ![{\displaystyle O(\min(r,n))}](https://wikimedia.org/api/rest_v1/media/math/render/svg/a65d323bed8f4bdf9d4e2a97265c76ac7a25361e) instead of O ( n r ) ![{\displaystyle O(nr)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e4be7d00195041b16efc063d83a41f4262ce56f) )
-  The last and current row can be stored on the same 1D array by traversing the inner loop backwards
- Store only non-zero values in the rows. This can be done using hash-tables instead of arrays. This is useful for large alphabets.