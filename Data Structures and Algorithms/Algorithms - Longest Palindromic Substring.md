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

