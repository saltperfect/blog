---
title: "Compiler101"
date: 2021-10-03T11:32:47+05:30
---

It is very easy in the hustle and bustle to keep up with all the cloud technologies, to forget about the basic principles of writing instructions for computers.

Yesterday, I sat down with [@aditya1304jain](https://twitter.com/aditya1304jain) and asked him to tell me all about it.

Here are few of the points that you might benefit from. 

# Compiler and Languages

Every compiler has a frontend and a backend. Frontend is platform independent and backend is dependent. Some compiler backend need information about platform at compile time and some don't. This point will come up in our discussion below.

1. C/C++/Go - The cpp/c/go compiler takes in `.cpp/.c/.go` files and optimizes source code, generates a assembly in the process which is them converted to instruction set for certain types of processor architecture. Hence cpp needs the information about which platform the instruction set will run on at compile time.

2. Java - When we do `javac` on any `.java` file it creates a `.class` file. As we know that java runs on virtual machine hence once the byte code is generated ie `.class` file, it is not jvm tasks to take in platform independent file and generate the instruction set for the platform/ process jvm is running on.

3. Python - As most of you know there no intermediate step of creating the binary, what could be happening here? Python abstracts out binary from the end user but internally works quite similar to java. Also you might have run python in interactive mode. The execution of the steps happen line by line as the user is executing them. This is made possible python interpreter. Under the hood python also runs a virtual machine to make it possible to run `.py` files platform independently. 
