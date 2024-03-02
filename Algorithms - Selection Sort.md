#dsa #algorithms #datastructures #problemsolutions 

This note comes mostly from the book "Grokking Algorithms" by Aditya Bhargava. Details of the implementation in JS and Python are from other sources/my interpretation of the pseudocode in the book.

To follow this note, an understanding of the advantages of linked lists vs arrays is necessary, as well as big O notation. See: [[Algorithms - Big O Notation]], [[Data Structures - Linked Lists]], [[Data Structures - Memory and How Arrays Work]]

## A basic search application
Using the example from Grokking algorithms, say that you have a list of musical artists that you like on your computer, and for each one you have an associated play count. You want to sort this list by play count so that it is always easy to see who your favourite artist is at any given time.

The naive approach to this problem would be to go through the list and find the most played artist, then move it to a new empty list, removing it from the original. You would then repeat this process until the original list is empty and the new one is sorted. 

<span style="color: cyan; font-weight: bold;">This naive approach to sorting a list would involve touching every element in the list to be sorted, meaning the algorithm would have LINEAR time, or O(n) time complexity.</span>

