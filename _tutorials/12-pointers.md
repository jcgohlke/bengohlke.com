---
title: "Pointers"
permalink: /tutorials/pointers/
excerpt: |
    This lesson discusses pointers and their use in Objective-C for memory management. You’ll learn how to create and use variables that are pointers to objects.
---

## What’s the Point?

So far in this unit, we’ve been dancing around this concept, but never have we stopped to consider it carefully. All those `*` characters before certain variable names actually have a purpose, rather than just being an extra, seemingly unnecessary keystroke. Placing a `*` character before a variable identifier in a declaration means that the variable is in fact a *pointer* to an object, rather than the object itself. This may seem to be an unimportant distinction, but it has profound ramifications to the compiler.

### Memory Management

Before we get into the practical details of pointers in Objective-C, let’s first talk a little about how the computer manages its memory. We’re referring here to the computer’s RAM (random access memory), not its storage (hard drive). Basically, the more memory a computer has, the more it can keep "top-of-mind" at once. Since memory is a finite resource, it is important to manage its use so it doesn’t get cluttered up with data that is no longer needed. Certain languages require the programmer to manage the memory used in the apps you write, and Objective-C is one of them. This is in part because Objective-C is what’s called a *superset* of ANSI C[^syntax], another older programming language.

### Get to the Point Already

Let’s look at an example, which will hopefully clear things up a bit. See the below diagram.

![PointerExample.png](https://tiy-learn-content.s3.amazonaws.com/8b805598-PointerExample.png)

We have an `NSString` variable declared and initialized with an `NSString` literal that reads `Millennium Falcon`.  To store that string, we need space in memory. Since strings can be different lengths, they may take up more or less memory when created, thus the compiler can’t assume any given string will be a certain size. This is where the pointer comes in.

1. The variable `ship` is assigned a block of memory. The space is a fixed size and will not contain the string `Millennium Falcon`, but instead hold the *address* to another block of memory. Since memory addresses are all the same size, we can easily fit any given address in an individual block of memory.
2. Memory is *allocated* for the actual string `Millennium Falcon`. The area marked in red in the diagram spans multiple consecutive memory blocks. This is because the string needs all this room to store the characters that make up the string.
3. The address for the starting block in the red area is placed in the block of memory reserved for the variable `ship`. Thus the variable *points* to the actual span of memory where the string is saved.

When the variable is used later in code, we want the value of the string, not the memory address where it’s saved, so we must *dereference* the pointer. All that means is the computer will look in the block for our variable, use the value found there (the starting block address for our actual string) and jump to that block to read the contents of it and any other related blocks that hold our string value. 

This is analogous to receiving an address for a house on a piece of paper. To get to the house, we enter the address on the paper into a navigation app and follow the directions provided. The address itself isn’t valuable. It’s the house and its contents that we want to access.

## Recap
Let's take a look at the concepts covered in this lesson:
* Pointers and how they are used to allow storage of large objects

[^syntax]: [https://en.wikipedia.org/wiki/Objective-C#Syntax](https://en.wikipedia.org/wiki/Objective-C#Syntax)