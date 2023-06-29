#dsa #datastructures #memory #optimization #highlevelconcepts 

This note is intended to explain what hash tables are, how they work on a system level, and how they relate to dictionaries and associative arrays in the same way as the notes on linked lists and arrays and memory do: [[Data Structures - Linked Lists]] [[Data Structures - Memory and How Arrays Work]].

These notes are adapted from [this video by simonDev](https://www.youtube.com/watch?v=S5NY1fqisSY&t=46s).

## What is a hash table?
Hash tables are the next logical step after learning about arrays and linked lists, and we will start by looking at exactly what is the problem that they are meant to solve.

Hash tables are a way of storing a bunch of related crap in a way that lets us access it later on, quickly, using a key. This is obviously very similar to an array, which is a group of data stored for later where each entry is accessed through it's index (the key in this case). The problems here that hashmaps solve are that the key in the case of arrays is entirely dependant on the insertion order, and if you don't know the index of a given entry ahead of time, you have to search the whole array to find it.

Hash tables use a hashing function to compute the key for which a value will be stored. A very simple and kinda dumb but illustrative example of this is a hash function for storing the names of the characters on futurama. We can hash these values based on their length, so `Bender` would be stored at index 5, `fry` would be stored at index 3 etc. This 