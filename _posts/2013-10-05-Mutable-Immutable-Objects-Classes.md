---
layout: post
title:  "Mutuable Immutable Objects/Classes"
date:   2013-10-05
tags:
  - java
  - classes
  - mutable_objects
  - immutables
---

Immutable objects are those whose contents/field values can't be change after their creation/instantiation, their values remain same up till  they resides in memory. They don't have any setter method, but do have getter methods to access them.
Mutable objects in contrast are those whose fields can be change, they have both setter and getter methods.


Eg of immutable classes in Java : all wrapper classes String, Integer, Byte, Short, Long, Character, Decimal, Float, BigDecimal, BigInteger.

### Immutable objects at good parts:  
* Since they don't change their values/state, programmers often use them in multi-threading programs, we can share them with any number of programs without fear of any inconsistency problem. In other words whenever  sharing comes immutable objects play vital role as they are always consistent.They eliminate the code to make mutable objects safe, useful in concurrent programming.  
* They cannot be corrupted by thread interruptions, automatically thread safe.
* No need of making defensive copies when returning or passing objects to other functions.  
* String Pooling concept is safe because String objects are immutable.

### At bad parts:
* They can't be modified, thus deferentially a big hurdle when we need to change values of our instances(eg in gaming programs).
* Always a new object is created, memory and thus performance/efficiency issues can occur.
* Our perception of the real world is inevitably based on mutable objects. When you fill up your car with fuel at the gas station, you perceive it as the same object all along (i.e. its identity is maintained while its state is changing) - not as if the old car with an empty tank got replaced with consecutive new car instances having their tank gradually more and more full. So whenever we are modeling some real-world domain in a program, it is usually more straightforward and easier to implement the domain model using mutable objects to represent real-world entities.  

### How to create Immutable classes(in Java):   
1. Very first the variables/fields in class must be private.
2. Make class final so that methods inside class can't be overridden.
3. Set the value of the properties using constructor only and don't use any setter method, otherwise it be no more immutable.
4. Protect mutable fields in your class(if any):
   * Don't share references to the mutable objects. Never store references to external, mutable objects passed to the constructor; if necessary, create copies, and store references to the copies. Similarly, create copies of your internal mutable objects when necessary to avoid returning the originals in your methods.
   * Don't provide methods that modify the mutable objects inside class, i.e,  don't forget overload/override the methods of mutable class (whose reference you are using), which can alter values of existing object. 


Refrences:
* [If immutable objects are good, why do people keep creating mutable objects?][1]
* [Mutable and Immutable Objects][2]
* [Immutable Class Interview Questions][3]
* [http://javarevisited.blogspot.in/2010/10/why-string-is-immutable-or-final-in-java.html][4]

[1]: https://softwareengineering.stackexchange.com/questions/151733/if-immutable-objects-are-good-why-do-people-keep-creating-mutable-objects
[2]: http://www.javaranch.com/journal/2003/04/immutable.htm  
[3]: http://java-questions.com/ImmutableClass_interview_questions.html  
[4]: http://javarevisited.blogspot.in/2010/10/why-string-is-immutable-in-java.html 
