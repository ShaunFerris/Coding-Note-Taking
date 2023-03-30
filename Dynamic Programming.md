#algorithms #algorithmoptimization #highlevelconcepts

Initial notes for this section are coming from a basic transcription of [this](https://www.youtube.com/watch?v=6z4ePR7YYa8) video by SimonDev.

Example explanation from the video: 
Your boss asks you to watch seasons 1 and 2 of rick and morty, and count how many times rick burps. You figure you can do this in about 2 days. Your boss catches up with you in about 2 days and says actually, you need to count burps for seasons 2 and 3 instead. You say you can have that done by the end of today. **Why is this second task of the same size going to be done so much faster?** Obviously, because you have your season 2 notes from the first task you did, and can just focus only on the new work this time around.

In real life this is just called using your **common sense**, in programming it is called **Dynamic programming.** The name was literally chosen in part because it sounds impressive.

## What dynamic programming actually is
DP is essentially breaking problems down into sub-problems, which are in turn broken down into sub-problems and so on an arbitrary number of times, then solving those sub-problems and remembering the answers so that you never have to solve the same thing multiple times.

The two key attributes required for dynamic programming to apply to a problem are:
1. Optimal substructure
2. Overlapping sub-problems

### Optimal substructure
A problems has optimal sub-structure if the optimal solution to the problem is some combination of the optimal solution to it's sub-problems, ie: if you solve all the sub-problems in the best way possible, you have solved the main problem.

### Overlapping sub-problems
In computer science, a problem is said to have overlapping subproblems if the problem can be broken down into subproblems which are reused several times or a recursive algorithm for the problem solves the same subproblem over and over rather than always generating new subproblems. These sub-problems also have to be deterministic. A deterministic algorithm is _an algorithm that, given a particular input, will always produce the same output.

## Memoization
Memoization is essentially an optimization, wherby we avoid having to compute the same thing over and over again. This means caching the returned result of some function, so that if the same function is ever called again with the same parameters, you can simply return the previously cached result.
Example in JS:
```
// Example function
function computeStuffSlowly(stuff) {
	// Do a bunch of calculation
	return stuff * 2;
}

// Memoized version of example function
const cachedResults = {};
function memoizedComputeStuffSlowly(stuff) {
	if (stuff in cachedResults) {
			return cachedResults[stuff];
	}
	const result = computeStuffSlowly(stuff);
	cachedResults[stuff] = result;
	return result;	
}
```

## Tabulation
Basically the same idea as memoization, except that with recursive solutions you tend to work top down, and with iterative ones you tend to work bottom up. Working toward the solution by building up from smaller solutions, typically by filling out a table or array. So tabulation is literally just filling out a table or array or whatever with answers to a problems sub problems ahead of time.

## Top down Vs Bottom up
Top down and bottom up are to different approaches to dynamic programming, but both are dynamic programming and both are valid approaches. Top down is exactly what the name implies, breaking the top problem, aka the problem, down into smaller ones and so on into ever smaller problems that can then be solved. A good example is the simpsons season 3 episode 10 where the town has a pidgeon problem. Lizards are brought in to kill the pidgeons, but then become a problem themselves, so chinese needle snakes are bought in to kill the pidgeons, and then gorillas are bought in to deal with the snakes, and then when winter rolls around the gorillas will die out.

The bottom up approach starts further away from the problem, solving small, easy problems that then piggy back to the next set of slightly harder problems, working up to solving the main problem. Like homer with his giant pile of sugar: First you get the sugar, then you get the power, then you get the women. In this instance the base case, or small easy problem, is having a giant pile of sugar, and this is leveraged to work towards the ultimate goal of getting women.

## Solving problems dynamically
Start by breaking down a problem into sub problems, then see if those can be further broken down into sub problems. Then imagine solving sub problems with function calls and look for ones that might be performing essentially the same task in a given state and implement memoization. The below diagram illustrates this idea, with nodes on the tree representing sub problems being solved with function calls and the orange arrows representing nodes that are computing the same thing at some point.
![[Pasted image 20230330151003.png]]
And here is an example of this kind of solution map for dynamic programming applied to computing the fibonnaci sequence:
![[Pasted image 20230330151418.png]]
And here is the solution to this problem in JS for illustration:
```
const memoizer = {};

function fib(i) {
	const key = '' + i;
	if (key in memoizer) {
		return memoizer[key];
	}

	if (i == 0) {
		return 0;
	} else if (i == 1) {
		return 1;
	}

	const result = fib(i  - 1) + fib(1 - 2);
	memoizer[key] = result;
	return result;
}

function calculateFibonnaci(i) {
	return fib(i);
}
```