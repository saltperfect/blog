---
title: "Compiler101"
date: 2021-10-03T11:32:47+05:30
---
This is a my first blog on my first hugo site.

It is very easy in the hustle and bustle to keep up with all the cloud technologies, to forget about the basic principles of writing instructions for computers.

Yesterday, I sat down with [@aditya1304jain](https://twitter.com/aditya1304jain) and asked him to tell me all about it.

Here are few of the points that you might benefit from. 

# Compiler and Languages

Every language has a frontend and a backend. Front is platform independent and backend is dependent. Some language backend need information about platform at compile time and some don't. This point will come up in our discussion below.

1. C/C++/Go - The cpp compiler takes in .cpp files and optimizes source code, generates a assembly in the process which is them converted to instruction set for certain types of processor architecture. Hence cpp needs the information about which platform the instruction set will run on at compile time.

2. Java - When we do `javac` on any `.java` file it creates a `.class` file. As we know that java runs on virtual machine hence it's the work of virtual machine to understand what type of byte code `.class` file should be generated to successfully run the code.

3. Python - As most of you know there no intermediate step of creating the binary, what could be happening here? Python abstracts out binary from the end user but internally works quite similar to java. It caches the binary and compiles and run it for the user. It also has virtual machine just like java and is platform independent. Running code on virtual machine makes memory management easy for the language. We will see this point below.   
