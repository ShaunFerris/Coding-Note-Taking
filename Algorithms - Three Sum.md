#dsa #algorithms #arrays #problemsolutions

3Sum is a very well known algorithm problem in computer science, and a very popular interview question. Here is a problem statement for it from leetcode.com:
<blockquote style="color: cyan; font-weight: bold; font-style: italic;">Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.</blockquote>
Let's go through three solutions to this problem, starting with the most intuitive and easy to grasp, and slowly adding optimizations as we go. All of these solutions will have a time complexity of **On^2** in the worst case.

## Solution one
This approach will use the two pointer approach, which simply means that for each element we are considering as a candidate for an output triplet, we set two other pointers that move through the array and check the sum of these three elements for our inclusion conditions (summing to 0).

That is the general idea anyway, here is a more granular step-by-step:
1. Sort the array in ascending order - in python we can just use the built-in sort method. This is necesarry to efficiently apply the two pointer method
2. Initialize a set to hold the unique triplets. Using a set will keep the output free of duplicates, so long as the triplets are all internally sorted before adding to the set
3. Iterate through the array to the -2 index. We don't go to the -1 index as this will be checked by one of the two pointers
4. The iteration variable `i` will be the index of the candidate, and we will initialize our two pointers `j` and `k` to `i + 1` and `arr[-1]` respectively. 
5. Assign `nums[i], nums[j], nums[k] = first, second, third`
6. Use a while loop that moves `j` and `k` towards each other checking triplet combinations as you go
7. If the sum of the current triplet is > 0, decrement k, if the sum is < 0 increment j and if it == 0 then export the current triplet. Note that we should always export the triplet as `(first, second, third)` so that each triplet is sorted in the same order, and thus by simply being added to the output set, duplicates will be excluded.

Here it is in code:
```python
def three_sum(nums: List[int]) -> List[List[int]]:
	nums.sort()
	triplets = {}

	for i in range(len(nums) - 2):
		first_number = nums[i]
		j = i + 1
		k = len(nums) - 1
		while j < k:
			second_number = nums[j]
			third_number = nums[k]

			curr_sum = first_number + second_number + third_number
			if curr_sum > 0:
				k -= 1
			elif curr_sum < 0:
				j += 1
			else:
				triplets.add((first_number, second_number, third_number))
				j += 1
				k -= 1
	return triplets
```