---
layout: default
title: "Studio 10 - Memory Model"
author: Doug Shook
---
{% include nav.html %}

# {{ page.title }}

![========]({{ "/images/line.gif" | relative_url }})

Click [here](https://wustl.instructure.com/courses/68860/assignments/289489) to access the Canvas page with the repository for this studio.

![========]({{ "/images/line.gif" | relative_url }})

## Introduction

Now that we've had some experience with working in assembly, we'll use assembly to perform some more complicated tasks that work with references and get more experience with the C memory model.

### Objectives

By the end of this studio, you should:

- understand what a reference is and how to use it,
- know how to access global variables in assembly, and
- know how to access array elements in assembly

![========]({{ "/images/line.gif" | relative_url }})

## Around the globe

We'll start out by taking a look at how to use global variables.

1. Set up your repository. We have written a bare bones project, `memoryModel`. In it are assembly stubs for three functions. Let's start by looking at increment and decrement.

2. The goal of increment and decrement is simple: we want to add one or subtract one from a global variable. First, define a global variable in your assembly code. We wish to use a single byte for our counter. This counter should have an initial value of 0.

3. Complete the increment/decrement functions. These functions should add or subtract one from the global counter variable that you created. Notice that there are no input parameters for either of these functions. To access the global variable, you will need to get a _reference_ to it, which you can do using the `hi8()` and `lo8()` functions, as explained in the prep work. You will then need to copy this reference into the appropriate registers (called indirect address registers in the [documentation](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-BA6EA9E1-E8BD-4CB3-A406-DEA0855A9413), we typically just say **address registers**) so you can access it using `ld` or `st`. Make sure you are properly using these registers (i.e. pay attention to whether they are call-saved or call-used).

4. After you increment/decrement the value, store the updated value back to the global variable. The return value of these functions should be the value of the global variable after the increment/decrement has taken place. You can check your work by running the tests provided in `memoryModel.ino`.

![========]({{ "/images/line.gif" | relative_url }})

## Arrays

Now that you have some practice using global variables, let's switch to arrays. There's just one assembly function to complete here: 

- `uint16_t sumArray(uint8_t a[], uint8_t length)`: Add all the elements in an array.

Before you start on these functions, it will be helpful to get some practice using pointers in C to understand how they will work in your assembly functions. Complete the C function `sumArrayPointers(uint8_t *a, uint8_t length)`. Notice that this function takes in a _pointer_ (reference). You should use this pointer to access the array values and sum up the array.

Some notes to help you with this:

* Recall that we can add and subtract values from the pointer to move the index. So at the beginning of the function, `a` will be pointing at the first value of the array. If we then run a statement like `a++`, the pointer will now be pointing at the second value of the array. We can use this idea to access all of the array values.

* To see the value that `a` is pointing at, you will need to use the _dereference operator_. A line of code like `uint8_t x = *a;` will grab the value that the array pointer is currently pointing at and store it in a variable. You can then use this value for whatever you like (such as computing a sum).

Note that in practice, we typically would not write `sumArray` using pointers in C. The use of brackets `[]` makes array access much more convenient for us and there is nothing wrong with using arrays in that fashion. We are asking you to write your function using pointers because it is much closer to how the assembly code will operate.

Once you have finished `sumArrayPointers`, complete the assembly functions `sumArray`. Some notes to help you with these functions:

* We now have input parameters, some of which are references, some of which are values. Make sure you understand which parameters fall into each category, and make sure you use them appropriately.

* Finding the length of an array in assembly is doable but tricky. Instead of making you do this, we have simply provided the length of the array as an input parameter.

* Use your `sumArrayPointer` function as a guide - these two functions should behave the same way and will follow a similar path to compute the result.

![========]({{ "/images/line.gif" | relative_url }})

## Debugging

In addition to the `printRegs()` function from the previous assignment, 
the studio also contains a file called `asmMacros.S`.  This file includes
*macros*.  A *macro* is a set of assembly language instructions that can be
added to a file in place of a simple name.  This file defines two
macros, one named `print` and another named `printAReg`.  When either word
is used `assembly.S`, many instructions will be inserted in place of the
name.  As the names imply, they are intended to print out things, which may
help you debug your code. 

`print` is for printing single words or groups of words. To print a single
word, `print WORD`.  To print multiple words, `print [MULTIPLE WORDS]`,
which will print the brackets and the words within.  Note that `print` does
not work well with symbols, like `=`.

`printAReg` will print the contents of a single register. For example,
`pringAReg 22` will print `r22 = 0xXX`.  Note that it prints the value in
hex and only the number is used for the register (`22` rather than `r22`).

These macros and `printRegs` can be used to study how individual values
change as your assembly code executes.

![========]({{ "/images/line.gif" | relative_url }})

## Demo

When done, your code should run all test cases without error.

1. Commit your code and get checked out by a TA.

![========]({{ "/images/line.gif" | relative_url }})

## Key Concepts

This is a mental checklist for you to see what the Studio is designed to teach you. 

- Assembly 
	- Value vs. Reference
		- `ld`, `st` instructions
		- Reference registers
	- Global values in assembly
		- Defining global values
		- Reading and writing global values
	- Arrays
		- Using pointers in C
		- Using pointers in AVR assembly
	

{% include footer.html %}
