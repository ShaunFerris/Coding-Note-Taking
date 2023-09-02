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
