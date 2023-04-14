#python #functions #functionalprogramming #paradigms #arrays 


## Map
The `map()` function iterates through all items in the given iterable and executes the `function` we passed as an argument on each of them.

The syntax is:
```python
map(function, iterable(s))
```

We can pass as many iterable objects as we want after passing the `function` we want to use:
```python
# Without using lambdas
def starts_with_A(s):
    return s[0] == "A"

fruit = ["Apple", "Banana", "Pear", "Apricot", "Orange"]
map_object = map(starts_with_A, fruit)

print(list(map_object))
```
This code will result in:
```python
[True, False, False, True, False]
```

As we can see, we ended up with a new list where the function `starts_with_A()` was evaluated for each of the elements in the list `fruit`. The results of this function were added to the list sequentially.

A prettier way to do this exact same thing is by using lambdas:
```python
fruit = ["Apple", "Banana", "Pear", "Apricot", "Orange"]
map_object = map(lambda s: s[0] == "A", fruit)

print(list(map_object))
```
We get the same output:
```python
[True, False, False, True, False]
```

**Note:** You may have noticed that we've cast `map_object` to a list to print each element's value. We did this because calling `print()` on a list will print the actual values of the elements. Calling `print()` on `map_object` would print the memory addresses of the values instead.

The `map()` function returns the `map_object` type, which is an iterable and we could have printed the results like this as well:
```python
for value in map_object:
    print(value)
```
If you'd like the `map()` function to return a list instead, you can just cast it when calling the function:
```python
result_list = list(map(lambda s: s[0] == "A", fruit))
```

## Filter
Similar to `map()`, `filter()` takes a `function` object and an iterable and creates a new list.

As the name suggests, `filter()` forms a new list that contains only elements that satisfy a certain condition, i.e. the `function` we passed returns `True`.

The syntax is:
```python
filter(function, iterable(s))
```

Using the previous example, we can see that the new list will only contain elements for which the `starts_with_A()` function returns `True`:
```python
# Without using lambdas
def starts_with_A(s):
    return s[0] == "A"

fruit = ["Apple", "Banana", "Pear", "Apricot", "Orange"]
filter_object = filter(starts_with_A, fruit)

print(list(filter_object))
```
Running this code will result in a shorter list:
```python
['Apple', 'Apricot']
```

Or, rewritten using a lambda:
```python
fruit = ["Apple", "Banana", "Pear", "Apricot", "Orange"]
filter_object = filter(lambda s: s[0] == "A", fruit)

print(list(filter_object))
```
Printing gives us the same output:
```python
['Apple', 'Apricot']
```

## Reduce
`reduce()` works differently than `map()` and `filter()`. It does not return a new list based on the `function` and iterable we've passed. Instead, it returns a single value.

Also, in Python 3 `reduce()` isn't a built-in function anymore, and it can be found in the `functools` module.

The syntax is:
```python
reduce(function, sequence[, initial])
```

`reduce()` works by calling the `function` we passed for the first two items in the sequence. The result returned by the `function` is used in another call to `function` alongside with the next (third in this case), element.

This process repeats until we've gone through all the elements in the sequence.

The optional argument `initial` is used, when present, at the beginning of this "loop" with the first element in the first call to `function`. In a way, the `initial` element is the 0th element, before the first one, when provided.

`reduce()` is a bit harder to understand than `map()` and `filter()`, so let's look at a step by step example:

1.  We start with a list `[2, 4, 7, 3]` and pass the `add(x, y)` function to `reduce()` alongside this list, without an `initial` value
    
2.  `reduce()` calls `add(2, 4)`, and `add()` returns `6`
    
3.  `reduce()` calls `add(6, 7)` (result of the previous call to `add()` and the next element in the list as parameters), and `add()` returns `13`
    
4.  `reduce()` calls `add(13, 3)`, and `add()` returns `16`
    
5.  Since no more elements are left in the sequence, `reduce()` returns `16`
    

The only difference, if we had given an `initial` value would have been an additional step - **1.5.** where `reduce()` would call `add(initial, 2)` and use that return value in step **2**.

Let's go ahead and use the `reduce()` function:
```python
from functools import reduce

def add(x, y):
    return x + y

list = [2, 4, 7, 3]
print(reduce(add, list))
```

Running this code would yield:
```python
16
```

Again, this could be written using lambdas:
```python
from functools import reduce

list = [2, 4, 7, 3]
print(reduce(lambda x, y: x + y, list))
print("With an initial value: " + str(reduce(lambda x, y: x + y, list, 10)))
```
And the code would result in:
```python
16
With an initial value: 26
```