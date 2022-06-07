---
layout: default
title: Assignment 10 - Assembly Puzzles Part 3
author: Doug Shook
---
{% include nav.html %}

# {{ page.title }}

Click [here](https://wustl.instructure.com/courses/68860/assignments/289485) to access the Canvas page with the repository for this assignment.

![========]({{ "/images/line.gif" | relative_url }})

## Overview

In this assignment you're tasked with creating more functions using assembly language. There are two big differences between this assignment and the last:

- This assignment requires the use of global variables 
- This assignment uses arrays (one dimensional). 

As before, you will likely want to keep the [AVR Assembly Reference](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-BA59618D-4850-490B-B176-0BCC3D9438A1) handy as well as the [rules for register usage]({{ "/files/RegisterUsageConventions.pdf" | relative_url }}). ***Make sure you are using registers appropriately - we will be checking for this when you demo. Some of the functions you have to complete are both called and call another function --- they need to follow both the caller-saved and the callee-saved conventions!!!  Use the stack!***

![========]({{ "/images/line.gif" | relative_url }})

## Goals

The goal of this assignment is to implement basic functions in assembly language. The functions chosen are examples that may actually be implemented in assembly rather than a higher level language. 

The main concepts are:

- Globals: You will need to create a global variable in your assembly code. One function will take in a parameter and add it to this global variable. Another function will return the current value of the global variable.

- String length: Find the length of a "C-style" string.  C stores strings as an array of characters. Rather than using a separate variable to keep track of the length, C uses a "sentinel value". That is, a special value is used to mark the end of the string.  The most used value is usually referred to as the "[null terminator](https://en.wikipedia.org/wiki/Null-terminated_string)" and is a byte with the decimal value 0 (i.e. the bits are all zeros, not the ASCII character `'0'`). To compute the length of the string you just start at the beginning and advance through each character until you encounter the sentinel value.  The number of times you advance indicates the length of the string.

- Adding arrays: Given two arrays with values and an array to store results, sum corresponding the elements of the arrays with values together and put the result in the corresponding location in the result array. Also known as vector addition. For example, if you are given arrays [1, 2, 3] and [4, 5, 6] the result would be [5, 7, 9].

- Dot product: Given two arrays, compute the dot product of those arrays by multipling corresponding values together and summing the products. For example, if you are given two arrays [1, 2, 3] and [4, 5, 6] the result would be (1 * 4) + (2 * 5) + (3 * 6) = 32.

![========]({{ "/images/line.gif" | relative_url }})

## The Assignment

In your repository you will find an `assignment10Demo` Arduino project. This project contains the prototypes for the functions we are asking you to write as well as code to test your work.

The Assembly language functions may be easier to write if you have already tested the logic using C. There are three C-functions you'll need to complete in `assignment10Demo.ino`:

- `uint8_t cStringLengthAlgorithm(const char aString[])`: This function will compute the length a C-style string by using a loop to search for the sentinel value. 
- `void cSumArrays(uint8_t *a, uint8_t *b, uint8_t *c, byte length)`: This function will add arrays a and b together, placing the result in c.
- `uint16_t cDot(uint8_t *a, uint8_t *b, byte length)`: This function takes in two arrays and computes the dot product of those arrays. You should complete this function using only pointers (no [] allowed!).

There are five functions to complete entirely in assembly language:

- `void updateGlobal(uint8_t a)`: Adds the given value to a global variable.
- `uint8_t getGlobal()`: Returns the current value of the global variable.
- `uint8_t cStringLength(const char aString[])`: Computes the length of a C-style string in assembly.
- `void sumArrays(uint8_t *a, uint8_t *b, uint8_t *c, byte length)`: This function will add arrays a and b together, placing the result in c.
- `uint16_t dot(uint8_t *a, uint8_t *b, byte length)`: This function will compute and return the dot product of a and b.

![========]({{ "/images/line.gif" | relative_url }})

## Getting Started

Assembly language can be challenging.  The provided files include several test cases to help you with your work.  Here's the approach we suggest:

1. assignment10Demo.ino includes a lot of code to help you with testing.  The `setup()` includes calls to all test cases.

2. Complete the global functions first, and make sure they pass the tests.	You will need to create a global variable in `assignment10.S` before you can use it in your assembly functions.

3. For the remaining functions, it is probably best to start with the C-code versions of the functions. This will give you an idea of how to manipulate the pointers accordingly when you go to write the assembly functions.

4. Write and test the assembly language version of finding the length of a string: `cStringLength` in `assignment10.S`.

5. Write and test the assembly language version of `sumArrays`. Note that you have three array pointers here: two arrays to be added and one array for the result. You need to be careful with how you manipulate these pointers - make sure you don't lose any of the pointers! Conveniently, there are also 3 addressing registers (X, Y, Z).

6. Write and test the assembly language version of `cDot`. You will need to do multiplication for this problem, so make sure you are using the `mul` instruction appropriately, as you did on the previous assignment.

![========]({{ "/images/line.gif" | relative_url }})

## A Word on Multiplication

It is in your best interest to read up on the [AVR Multiplication Instruction](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-C301CF8C-34FD-43A4-BE0F-E705BB9FC85E) before attempting to complete the `cDot` function. 

The output of the multiplication instruction will be placed in `r0` and `r1`. Note, however, that `r1` is a special register that is expected to contain zero. What this means is that if you use a multiplication instruction, you must read the value out of `r1` and set it back to zero as soon as possible.

![========]({{ "/images/line.gif" | relative_url }})

## Checkout


1. Commit your code and verify in your web browser that is is all there.
2. Check out with a TA.

### The rubric

- 100pts total:
	- C functions
		- cStringLengthAlgorithm
		- cSumArrays
		- cDot
	- AVR Assembly functions
		- updateGlobal
		- getGlobal
		- cStringLength
		- sumArrays
		- dot
	- Correct usage of caller-saved and callee-saved registers
	- Code style

{% include footer.html %}
