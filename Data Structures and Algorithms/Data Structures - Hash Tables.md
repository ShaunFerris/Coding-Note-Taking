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
### Mapping sets of data
Hash tables are also called hash maps, or simply maps, and as this implies, they are perfectly suited to mapping one set of data to another. A classic example of this kind of use-case is the contacts app in your phone, which maps names to phone numbers, or DNS servers, which map names of websites to IP addresses.

### Preventing duplication of data
Another use case for hash maps is to quickly lookup submitted data in a hashmap to prevent recording duplicates. Say for example you are running a voting both and don't want people to vote more than once. If check every prospective voter in a hash map and only let them vote when they aren't in it, then add them to it after they vote you can achieve this. This example is straight from "grokking", see the copied illustration:
![[Pasted image 20240306100908.png]]

### Caching
The final example use case for hash maps is data caching. This application is commonly seen in webservers, where responses to requests to the same static page or data are cached in the browser to cut down on perceived latency and compute resources.![[Pasted image 20240306102325.png]]

## Collisions and performance
We've already touched on the fact that most if not all languages come with an implementation of hash maps, like Pythons dictionaries, so you won't need to implement your own. However you will still care about how they perform, and in order to understand hash map performance you first need to understand collisions.

### Collisions
To get into collisions, we need to expose a white lie in our initial explanation of hash functions. We said that the hash function should never assign two keys to the same slot in the underlying array, and while this would be ideal, it is almost impossible in practice. Say you have a 26 slot array and the hash function assigns slots based on alphabetical order of the first letter of the key. If you want to store the prices of apples, and avocados, you will get a collision where they are both assigned to index 0 of the array.  There are a few ways to handle collisions like this, the simplest way is this: <span style="color: cyan; font-weight: bold;">When you have a collision in a slot, store a linked list that points to the collided elements in that slot.</span>

With this solution, searching for elements that did not have a collision is still very fast, O(1), and searching for elements that have had a collision is still pretty fast, but slightly slower as you have to traverse the linked list. The lesson here is that the hash function is very important, if it hashes everything to the same slot then your performance will be exactly the same as a linked list, ideally it will hash keys evenly across the whole array.

