#javascript #whileloops #forloops 

## While loops
While loops in JS work pretty much as you'd expect, and follow the same basic formatting rules as  if statements [[JavaScript Basics]]:
```
let i = 0;
while (i < 5) {
	i++;
	console.log(i);
}
```

## For loops
For loops in JS are written in a way that exposes their inner workings much more than pythons for loops. Their general structure is `for (a; b; c){}` where:
- a = the initialization statement. Executes only once at the start of the loop, usually used to set-up a variable that will be incremented with each loop.
- b = the condition statement. This is evaluated once every iteration of the loop and is usually used to define under which condition the loop will terminate.
- c = the final expression. Evaluated once every loop like the condition statement, but at the end of each loop iteration instead of the start, typically used to increment the variable.

### Example for loop - push  numbers 0 to 10 to an array
NOTE: the push method is the JS equivalent of pythons append.
```
const ourArray = []
for (let i = 0; i <= 10; i++) {
	ourArray.push(i);
}
```
