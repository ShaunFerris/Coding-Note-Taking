#datastructures #memory #optimization #highlevelconcepts #arrays 

For a look at how to actually use arrays, the basic fundamentals of what they are, and some common useful methods for them in both python and JS, see [[Data Structures - Using Arrays]].

The contents of this note originally came from [this video](https://www.youtube.com/watch?v=247cXLkYt2M&list=PLRL3Z3lpLmH0FiSWovfiBtxNczQg0Hzry&index=1) by simondev, title **Memory, cache locality and why arrays are fast**.

## Topics covered
This is intended to be the first part of learning about data structures and optimization, and will cover:
- Understanding performance
- Talking about arrays
- Cool things your CPU does
- Why arrays are fast and when they are not

## Basics of arrays and memory
Consider a very basic array:
```python
prices = [1.99, 2.99, 5.99]
```
In this array we have a list of three prices. In general the main property of an array is that data is stored together in a group, and elements in the array can be accessed by their 0 numbered index.

What does this have to do with memory?

When we talk about memory here, we will be talking in terms of languages like C++ and python, where arrays occupy **contiguous** blocks of memory. (I think this is also true of JS but do not know for sure.) 

In a simplified view of how memory works on a computer, you have a giant block of places where data can be stored that makes up your ram chip. The block is subdivided into billions and billions of slots that fit one sigle byte. Each slot has an address which is incrementally numbered as you move along the chip, like house numbers on a street, that lets your computer identify where a specific byte of data lives. Every byte in memory is therefore addressable.

When you create a variable, what you are doing is allocating space for that variable somewhere in memory. So if you create a variable to store a single standard 32 bit float, you are allocating a block of 32 bits, or 4 bytes of memory to store it.

## Cool things your computer does or how memory access works
Example: 
Let's say that you are my assisstant, and I ask you to go and get me a chapter from a book in the library. You can to the library, find the book, copy the chapter and bring me the photocopy. Now I ask you for the next chapter. In general, if your job is to go to the library and get crap for me, how can you save some time?

Instead of just copying the chapter I want, check out the entire book, that way if the next thing I want happens to be from the same book, you can get me my request in a fraction of the time.
This is what the **L1, L2 and L3 caches** in a cpu are for. 

The other method that could work is for you to predict what I might be likely to ask for next and get it before I even ask. This is **pre-fetching**.

### The L1-L3 caches and the prefetcher unit
Most modern CPUs come with a small amount of built in memory, in the form of these caches. These are areas of superfast memory, kept close to the cpu, which the cpu can use to save things. (The L1 is the closest to the CPU.) 

When you do a read from memory, you don't just just pull a single byte, or whatever it is you asked for from memory. What actually hapens is a series of things. The cpu checks the L1 cache for the requested data, then the cpu sequentially checks the L2 and L3 caches if the data is not found. Each of these steps is sequentially more costly in terms of time, but each of the caches are also sequentially **bigger** and all cache checks are much faster than checking main memory (1-4ns vs 100ns for main mem.) 

If the cpu gets a hit in one of the caches, it returns the data and moves on, but if it misses in all of them, it reaches out to main memory, which is like 100 times slower. At that point, insted of only pulling in the data you asked for, it pulls in what is called a **cache line** which is 64 bytes and caches it. This means that on subsequent requests, even if they are for complely different things than the first one, you may get a cache hit! 

The CPU will also have a prefetcher unit, that lives roughly between the core proccessor and the caches, looking at what you request from memory. This allows it to recognize simple patterns in what you are doing, for example iterating over an array, and pull into cache things that you haven't specifically requested yet but are likely to. 

## What does this have to do with arrays?
CPUs are very good at working with contiguous chunks of memory, this is why operations like iterating over an array are very fast. Accessing array elements randomly is also fast for similar reasons. Apending items or removing items to an array is also fast, assuming that you are operating at the end of the array, because it only involves one operation.

### So what sucks when it comes to arrays?
There are situations where arrays are pretty bad. Finding an element in an array is pretty slow, because you kind of have to check each element one by one. This might not be a big deal with small arrays, but quickly becomes problematic if you have say, billions of entries.

Inserting or removing items in an array from anywhere except the end also gets progressively slower and slower the further you get from the end. Say you want to remove the second to last item in an array, you simply copy the last item to the second to last slot, and then mark the array as being one slot smaller. It follows then that if you have to do an operation at the beginning, you are now required to do a lot more operations. Every slot between the one you are working on and the end of the array will need to be modified due to the contiguous nature of memory allocation for the array.

## How to improve on the weaknesses of arrays?
More complex data structures! Like [[Data Structures - Linked Lists]] or [[Data Structures - Hash Tables Dictionaries and Associative Arrays]]
However it is not possible to understand these more complex structures without a solid understanding of how arrays work.