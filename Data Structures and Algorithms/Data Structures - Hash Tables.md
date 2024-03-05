#dsa #datastructures 

This note was put together going through the book "Grokking Algorithms". See also this other note from a simonDev youtube video which covers how they work at a memory level and compares them with dictionaries and associative arrays: [[Data Structures - Hash Tables, Dictionaries and Associative Arrays]]

Hash tables are one of the most common and useful basic data structures, and alot of algorithms rely heavily on them. This note is intended to cover the basic common use cases of them.

## Introductory example - grocery store
Imagine that you work in an old timey grocery store, think pen and paper and no computers. Whenever a customer wants to purchase any item, you need to look up it's current price in the price book. If the book is not in alphabetical order this will take a long time, as you will essentially be doing simple search, with O(n) time. If the book **is** alphabetized, you can use binary search to find the item, which will be alot faster with O(log n).

Bin search is obviously a lot faster, but it is still annoying and time consuming to do this for every item for every customer. What you really need is a friend who has a huge brain and has memorized every item and it's corresponding price, and can just tell you the info you need as you need it. <span style="color: cyan; font-weight: bold;">This person could give you the price of any item, as you need it, in O(1) time.</span>

How would we implement in code, this magical person who can retrieve price information in O(1) time? We could implement an array of tuples like this:
```python
prices = [("apple", 1.50), ("bannana", 10.00), ("lube", 0.50)]
```
If we make sure the array of names and corresponding prices is alphabetized, then we can search for the name price tuple we need with binary search in O(log n) time, but this is still not what we want, O(1) time. To achieve this we need to bring in a hash function.

## Hash functions - what makes a hash table
A hash function is a function that takes in string (or any kind of data as a sequence of bytes) and returns a number. It is mapping strings of bytes to numbers in a predictable and repeatable way. These are the requriements for a hash function:
1) It must be consistent, if you hash "apple" and get back 4, then you should always get 4 when you hash "apple" with the same function.
2) It should map different inputs to different outputs. If you get 4 from hashing "apple" you should not get 4 from hashing any other string in existence.

This is how we can build our magic price knowing person from the grocery store example with an array! Say we start with an empty array, and then use a hash function to hash "apple" getting 4 like above, we can now store the price of the apple at index 4 of that array. We can do this for all the items the store sells, and now the price of each item is stored in an array at the index we get from hashing the name. Now when we hash the name of something a customer is buying, we get it's index in the array and know exactly where to go to get the price!

The third requirement for the hash function in this example is that it must know how big the array is, so that it does not return a hash outside the index space of the array. 

This data structure we have built to store the prices of items is a **hash table**. It is the next step up from arrays in that it has additional logic to it, where arrays and linked lists map straight onto memory, hash maps use the hash function to decide where to store data. 

Hash tables are also known as hash maps, maps, dictionaries, and associative arrays. And hash tables are fast! You can get an item from an array instantly, and hash tables use an array to store the data, so they’re equally fast.

You’ll probably never have to implement hash tables yourself. Any good language will have an implementation for hash tables. Python has hash tables; they’re called dictionaries. You can make a new hash table using the dict function.

## Use cases
