#dsa #algorithms #datastructures #problemsolutions 

FizzBuzz is a very very common interview question, around writing an algorithm for a simple number game. The game is this, count up from zero, and for each number that is divisible by 3 replace the number with "Fizz". If the number is divisible by 5 replace it with "Buzz", and if it is divisible by both 3 and 5 replace it with "FizzBuzz". There are also variations that introduce a third word for numbers divisible by 7, often "Bazz".

There are a lot of ways to solve this problem and often the interviewer will want you to write out and explain at least a couple of the solutions, comparing their runtime efficiency and other properties.

For the purpose of this exercise we will be writing solutions that are required to print out the whole game sequence from 1 to 100.

## The intuitive solution - for loop and if/else control flow
```python
for i in range(101):
	if i % 15 == 0:
		print("FizzBuzz")
	elif i % 3 == 0:
		print("Fizz")
	elif i % 5 == 0:
		print("Buzz")
	else:
		print(i)
```
Note that the mod 15 statement covers all numbers divisible by both 3 and 5, there are no numbers lower than 15 that are. Also note that this statement has to come first in the control flow, or else numbers that should be "FizzBuzz" will return out at "Fizz", because they are divisible by 3. this is quite obvious but can be easy to forget and make you look a lil silly.
The same solution in JavaScript:
```javascript
for (let i = 1; i <= 100; i++) {
	if (i % 15 === 0) console.log ("FizzBuzz");
	else if (i % 3 === 0) console.log("Fizz");
	else if (i % 5 === 0) console.log("Buzz");
	else console.log(i);
}
```

## String concatenation
The next approach is really just a modified version of the previous, using string concatenation to remove one of the conditional steps. In python:
```python
for i in range(101):
	out = ''
	if i % 3 == 0:
		out += "Fizz"
	if i % 5 == 0:
		out += "Buzz"
	if not out:
		out = i
	print(out)
```
And in JavaScript:
```JavaScript
for (let i = 1; i <=100; i++) {
	let out = '';
	if (i % 3 === 0) out += "Fizz";
	if (i % 5 ===0) out += "Buzz";
	if (!out) out = i;
	console.log(out);
}
```

## Hashing the conditions - The best approach
This approach is the best way to solve the problem. While it may not necessarily out perform the simple if/else methods, it is the easiest implementation to expand to include more conditions (ie: the Fizz Buzz Bazz variant). The central idea is to map the conditions in an object, and then iterate throught the key value pairs. There are several ways to implement this general idea, and the unifying idea is the conditions hashmap. Here we will go over a version of this approach that uses the `map` and `join` methods, as this is the most intuitive one for me and works similarly in python and JS. It can also be done with `reduce`, but in python this needs to be imported from functools, or with `filter` and `forEach`, but python does not have a `forEach` method.

Here is the implementation with `map` and `join` in python:
```python
condition = {3: "Fizz", 5: "Buzz"}

for i in range(101):
    answer = "".join(map(lambda kv: kv[1] if i % kv[0] == 0 else "", condition.items()))
    if not answer:
        answer = i
    print(answer)
```
And in JS:
```javascript
const condition = { 3: "Fizz", 5: "Buzz" };

for (var i = 1; i <= 100; i++) {
  let answer = Object.entries(condition)
    .map(([key, value]) => i % key === 0 ? value : "")
    .join("");
  if (!answer) answer = i;
  console.log(answer);
}
```
When recalling these answers to the problem it is important that you remember the differences between the `map()` **function** in python and `Array.map()` **method** in JS, as well as the way that the join method works in python compared to JS.

