---
title: "Memory management 101"
date: 2021-10-06T19:27:02+05:30
---

This post is in continuation to compiler 101 post. These points came out while discussing language and compiler 
# What do you mean when you say this language has good memory management?

There are two aspects to memory management in perspective of any language.
a. Is the program able to release the unused memory. If a program is unable to do so, it causes memory leaks
b. When trying to allocate some memory for the program is the program able to get memory that is not being used by any other programs

Now lets talk about what strategy languages use to deal with these aspects.

1. C/C++ - The strategy is very simple when it comes to releasing unused memory. Its the responsibility to the author or the program to do so.
After C++11 there have been some advancements to the language which handles the complexity for us.

2. Java - As mentioned in [Compiler101](https://saltperfect.github.io/posts/compiler101/) Java programs run on JVM which sits on the processor. That means in this case JVM has a complete picture of which memory has been allocated, which memory is currently in use and other details, which make it possible of JVM to deallocate the memory for us.

3. Python - It doesn't expose pointers as a feature. That means all immutable objects are passed as value and all mutable values are passed as reference. Compiler keep a count of reference of a variable and check if the variable has gone out of scope. If its out of scope compiler goes ahead and deallocate the memory.   


