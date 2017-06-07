---
layout: post
title:  "Space Complexity"
date:   2014-02-13
tags:
  - algorithms
  - analysis
  - space_complexity
  - complexity
---

### What

It is just the amount of memory used by a program; the amount of computer memory that is main memory required by the algorithm to complete its execution with respect to the input size.

### Why

In todays world space is no more a hurdle what matters a lot is only time. So does one need to calculate space complexity?
Yes! If a program takes a lot of time, you can still run it, and just wait longer for the result. However if a program takes a lot of memory, you may not be able to run it at all, so this is an important parameter to understand.

{% include youtubePlayer.html id="CzD2KgvY-q0" %}

### How 

Space Complexity(s(P)) of an algorithm is total space taken by the algorithm to complete its execution with respect to the input size. It includes both Constant space and Auxiliary space.

S(P) = Constant space + Auxiliary space

Constant space is the one which is fixed for that algorithm; generally equals to space used by input and local variables.
Auxiliary Space is the extra/temporary space used by an algorithm.

{% include youtubePlayer.html id="L6rr_wGJpP0" %}

### Examples

We assume that space required for one variable is one unit for simplicity.

Case 1:    
```
def calculate(a,b,c):
    return a+b/c
```   
S(p) = 1 + 1 + 1 = 3 ==> O(1) 

Case 2:  
```
def sum(a, n):
    s = 0
    for i in range(n):
        s += a[i]
    return s
```   
S(p) = (n*1 + 1 + 1) + 1 ==> O(n) and auxiliary S(p) = O(1)

Case 3:   
```
 def Rsum(a,n):
    if n <= 0:
        return 0    
    return a[-1] + Rsum(a[:-2], n-1)
```

`(# of stack frames)*(space per stack frame)`

When the function calls itself there are 3 things that are stored in the stack.
The array pointer a,
The size of the array, n and
The value at the last index a[n].

So space per stack frame equals to `sizeof(a)+sizeof(n)+sizeof(a[n])`

This function calls itself till n becomes 0
so,  `number of stack frames = n (from n-1 to 0)`

Therefor the amount of space required to store execute is 
n times the space required to store above 3values once, 

i.e., `(n)*{sizeof(a)+sizeof(n)+sizeof(a[n])}`

Since the value in the braces remain a constant for a particular i/p, 

we say that the space complexity of this is directly proportional to n.
i.e auxilary S(p) = O(n)