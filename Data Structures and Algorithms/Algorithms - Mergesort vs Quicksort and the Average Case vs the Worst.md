#dsa #algorithms #datastructures #problemsolutions 

## Mergesort description and implementation
Mergesort is another recursive sorting algorithm that uses the divide and conquer strategy, see here: [[Algorithms - The Divide and Conquer Strategy]].

Simply described, it splits the list into two sub-arrays and then recurs itself on each of those arrays, then it uses a merge helper function to merge the sorted sub-arrays while maintaining the sorting. The base case is returning an array unchanged if it's length is 1 or less, so the merge function is doing the sorting from the smallest fragments.

Here is an anotated implementation in python from chatGPT (I have tested that it works, obv):
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    # Split the array into two halves
    mid = len(arr) // 2
    left_half = arr[:mid]
    right_half = arr[mid:]

    # Recursively sort both halves
    left_half = merge_sort(left_half)
    right_half = merge_sort(right_half)

    # Merge the sorted halves
    return merge(left_half, right_half)

def merge(left, right):
    result = []
    left_idx, right_idx = 0, 0

    # Merge the two sorted arrays
    while left_idx < len(left) and right_idx < len(right):
        if left[left_idx] < right[right_idx]:
            result.append(left[left_idx])
            left_idx += 1
        else:
            result.append(right[right_idx])
            right_idx += 1

    # Append remaining elements of left and right arrays
    # Note that these lines could be replaced to not use the extend method:
    # eg: result += left[left_idx:]
    result.extend(left[left_idx:])
    result.extend(right[right_idx:])

    return result

# Example usage:
arr = [12, 11, 13, 5, 6, 7]
sorted_arr = merge_sort(arr)
print("Sorted array:", sorted_arr)

```
The mergesort function itself is fairly similar to quicksort and a very straight forward example of divide and conquer, the base case is an array of length 1, the recursive case is calling itself on two halves of the array and then returning the merge of those sorted arrays.

The interesting stuff happens in the merge function, where we iterate along two arrays at the same time and because they are internally sorted, we know that the current element of one of the arrays will be the next logical element to add to the output array. We do this till one array is empty, and then we can be sure that the all remaining elements in the other array are larger, so we extend the output array with them and return.

## Mergesort vs. quicksort
