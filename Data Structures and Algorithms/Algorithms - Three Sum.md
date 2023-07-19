#dsa #algorithms #arrays #problemsolutions

3Sum is a very well known algorithm problem in computer science, and a very popular interview question. Here is a problem statement for it from leetcode.com:
<blockquote style="color: cyan; font-weight: bold; font-style: italic;">Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.</blockquote>
Let's go through two solutions to this problem, starting with the most intuitive and easy to grasp, and following up with a more optimized version. Both of these solutions will have a time complexity of **On^2** in the worst case.

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
	triplets = set()

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

## Solution two
This solution is an **improved** version of the first solution, that implements additional checking steps to skip duplicate values for higher efficiency. Here is a run-down of the imporvements made in this version:
1. Before executing any of the code in the for loop, check if the candidate value for this iteration is the same as the previous candidate, and if it is then continue out to the next iteration as all triplets with this candidate have already been computed
2. Inside the else block where a triplet that matches the sum condition is found, use two more while loops to skip over duplicate values of the pointer values

The time complexity here is improved overall, by skipping more duplicates. The formal time complexity is still O(n^2) because we consider the worst case, but the average case performance is much better. Here it is in code:
```python
def three_sum(nums: List[int]) -> List[List[int]]:
	nums.sort()
	triplets = set()

	for i in range(len(nums) - 2):
		if i != 0 and nums[i] == nums[i  - 1]:
			continue
		first = nums[i]
		j, k = i + 1, len(nums) - 1
		while j < k:
			second, third = nums[j], nums[k]
		curr_sum = first + second + third
		if curr_sum > 0:
			k -= 1
		elif curr_sum < 0:
			j += 1
		else:
			triplets.add(first, second, third)
			j, k = j + 1, k - 1
			while j < k and nums[j] == nums[j - 1]:
				j += 1 #skip if the new j is the same as the last j
			while j < k and nums[k] == nums[k + 1]:
				k -= 1 #same skipping logic as above
	return triplets
```