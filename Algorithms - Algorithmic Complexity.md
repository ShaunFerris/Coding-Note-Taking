#dsa #algorithms #algorithmoptimization 

NOTE: The content of this note is heavily based on the book "Slither into Data Structures and Algorithms", available [here.](https://www.slitherintopython.com/)

An important part of algorithm design is estimating the efficiency of the proposed algo in terms of space (memory) and time (number of operations).

The time based measure of algo performance is called <span style="color: cyan;">runtime performance</span>, or <span style="color: cyan;">time complexity</span>, and will be the focus of this note, while space complexity will be considered later in this note: [[Algorithms - Space Complexity]].

## Measuring time complexity
The most obvious way to measure time complexity is to measure the amount of time that the algo takes to run on a given set of data. This method works in the context of a single machine, but practically we need a way of quantifying this measure in a way that is agnostic of the hardware, because different machines with different operating systems and scheduling of processes will give different results even with the same input.

The second obvious method is to count the number of operations that the aglorithm needs to complete, however there is also a flaw in this strategy: There is no way to quantify what is or is not an operation. This is due to the implementations of different programming languages and styles of programming. Despite the flaws of trying to count operations in a way that is independant of language or style, we can use this concept as the basis for a method that will work, by combining it with the expectation that <span style="color: cyan;">as the size of the input increases, the runtime will increase in a specific way</span>.

<blockquote style="font-weight: bold;">In other words, there is a mathematical relationship between the size of the input, <span style="font-style: italic;">n,</span> and the time it takes for the algorithm to run.</blockquote>

This metric is based on three principles:
1. Worst case analysis (make on assumptions about the size of the input data set)
2. Ignore or supress constant factors and lower order terms. This comes from the fact that with large input sizes, the effect of the higher order terms will dominate in influence on the runtime performance
3. Focus on problems with large input sizes.

Worst case analysis give a tight upper bound that the algorithm is guranteed not to exceed. Ignoring the lower order terms and constants puts the focus on terms that have the largest effect on the overall runtime efficiency, and thus have the greatest impact on performance.

The formal method for characterizing runtime performance/time complexity is Big O notation, see: [[Algorithms - Big O Notation]].