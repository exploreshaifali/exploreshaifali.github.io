---
layout: post
title:  "Understanding Python Generators"
date:   2017-06-20
tags:
  - python
  - generators
  - iterators
---

Few days back I gave a talk in [BangPypers][1] meetup (Bengaluru Python group) about [generators][2]. The focus was to discuss what generators are and how does they work in python or why does they work the way they do. This was the first meetup I have ever attended for Python in Bengaluru so I was excited more than I was nervous for speaking. :P

Here I am sharing first half content that I have presented in the talk, keep reading if you want to understand what generators are. Slide is present [here][3].

### Warm-Up Exercise
I started with a warm up exercise, where I wrote an easy-peasy code. Here is the simple function `mycount`

```
def mycount():
    n =0
    numbers = [] # 3 get rid of
    while n < 10:
        numbers.append(n) # yield
        n += 1
    return numbers # rid of it

for num in count():
    print("Got number", num)

# OUTPUT
```

The `mycount` function basically returns list of numbers from 0 to 9. And then I called that function and print out each number inside that list it returned. So here we have a function that returns a list and then something (the for loop) which consumes that list; the function returns something we can loop over. Everything simple pretty much?

Now I will tweak the `mycount` function by adding one more print statement inside loop.

```
def mycount():
    n =0
    numbers = [] # 1 get rid of
    while n < 10:
        # print statement added
        print("In loop", n)
        numbers.append(n) # 3 yield
        n += 1
    return numbers # 2 rid of it

for num in count():
    print("Got number", num)

# OUTPUT
```

It is printing all `In loop` first and then all `Got number`. So what’s happening here is that we are making a list inside the `mycount` function and then immediately after we made the list, we are iterating over that list and then throwing it away. We never look it again. Whenever you see this type of scenario in your code, if there is a function returning a list, you can make that function into a generator, as long as there is no other code relay on it being a list.

### Writing generator function

So to make above `mycount` function to return a generator instead of list, following 3 steps need to be followed:  
    * get rid of the empty list assignment, at line 3 in above code.  
    * get rid of return statement at line 9 in above code.  
    * change append  into `yield` at line 7 in above code.  

`yield` is a special keyword in python, that is basically means produce/generate. And it always goes along with generators, so whenever there is yield there is generator going on and wherever there is generator there is probably `yield` happening there.

Generator's code

```
def mycount():
    n =0
    while n < 10:
        print("In loop", n)
        yield n
        n += 1

for num in count():
    print("Got number", num)

# OUTPUT
```

Notice the output, “In loop 0, got number 0, In loop 1, got number 1....so on”. So what happening this time, when we call generator, it starts executing `mycount` function and once the `yield` is encountered it stop executing and pause by yielding the value after `yield` (if any) and when we iterate again it `yield` value again and pause again till the time it returns.

A little, complex thing, so to understand it lets start with iterables.

### Iterables

You know that we can loop over many many things in python, like we can loop over a list,like  
```
l = [1,2,3]
for i in l:
    print(i)
```
it will iterate one element at a time. Similarly we can loop over a string, we can loop over a dict, a set. We are able to loop around them because all of them are iterables. Not only this, open file objects, open sockets are also iterables essentially. 

#### So what so special about iterables?  

It’s just that, that when we call them with the standard built-in `iter()` method, they returns iterators. And iterator is something around which we can loop over. So just keep in mind that iterable is something which returns itrerator, and iterators are very powerful. They allows us to iterate over a loop, they allows us to  perform  iteration one by one over and over.

we can loop around with iterator. Its a value factory, because, every time we ask it for "the next" value (in next iteration), it knows how to compute it and that’s what it gives back to us. Every iterator have a built-in method, `next()`, when we call that `next()`, method it returns the next value of the iterator, if there are no more values it raises `StopIteration` exception. And thats how for loop works in Python (under the hood).


### Generators are Iterators
But why was we talking about iterators? We are suppose to talk about generators! Well, because generators are a special type of iterators. They simplifies the creation of iterators.  And since a generator is an iterator, it can be loop over with. Thus in our example of warm up exercise,  we were able to iterate via a generator object.

`yield` is the keyword, which is used to create a generator function, any function which have `yield` keyword inside it is a generator function. Generators are also known is **lazy factories**, because they don’t store any value in memory, whenever we ask for a value they compute it and simply give it back to the calling environment. They don’t keep values computed in advance.

So our function from warm up exercise, `mycount()` is now a generator function which returns a generator object (a lazy factory). But that’s not the only way of creating a generator object. We can also create a generator with **generator expressions**, its very similar to list comprehensions but instead of square bracket, to create generate object simple use round brackets. 



[1]: https://www.meetup.com/BangPypers/
[2]: https://wiki.python.org/moin/Generators
[3]: https://docs.google.com/presentation/d/1fSA4ojdAARkD15hTzvc1S1E_qrouaogdo8PHhf_CMII/edit?usp=sharing