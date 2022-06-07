---
layout: default
title: Assignment 8 - Assembly Puzzles
author: Doug Shook & Bill Siever (modifications)
---
{% include nav.html %}

# {{ page.title }}

Click [here](https://wustl.instructure.com/courses/68860/assignments/289483) to access the Canvas page with the repository for this assignment.

![========]({{ "/images/line.gif" | relative_url }})

## Overview

It's easy to take high level programming languages, like C and Java, for granted. They make writing code  convenient for humans at the expense of adding in an extra step (compilation) that turns your code into something that the computer can execute, like machine language.

For this lab we are taking out the compiler! You are tasked with writing a series of functions **entirely in assembly**. You will definitely want to keep the [AVR Assembly Reference](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-BA59618D-4850-490B-B176-0BCC3D9438A1) handy. We're going to ask you to use some instructions that you probably haven't seen before.

Please note that **you should not use any branching instructions**. You may use the "skip" instructions, however (and in fact may be required to do so!).

An important note on registers: as summarized [here]({{ "/files/RegisterUsageConventions.pdf" | relative_url }}) (and described in the [documentation](http://ww1.microchip.com/downloads/en/appnotes/doc42055.pdf) but watch out for errors in Table 5.1), there are different kinds of registers in assembly. A caller-saved register may be used and modified by a subroutine, so any *calling* function must save them before calling a subroutine, either by `mov`ing them to another register or `push`ing them to the stack. A callee-saved register should not be affected by calling a subroutine (without likely crashing the calling C code), so your subroutine must save them before modifying them and restore them before returning. In otherwords, if you change the values in any callee-saved registers, like `r28` or `r29`, you must restore them back to their original values before your function returns.

The easiest way to save a register is to use the **stack**. Interaction with the stack utilizes two primary instructions: [push](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-51635F91-00DA-4C97-A39C-783551EFD2DE) and [pop](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-BA3B66E9-4A18-4C02-90FC-CCF42561A202). Push will put the contents of a register on the the top of a stack. Pop will remove the last value placed on the stack and put it in a register. 

This particular stack is often referred to as the [call stack](https://en.wikipedia.org/wiki/Call_stack) or the run-time stack.  The call stack is an important concept in computer security --- some vulnerabilities take advantage of it (this is covered in CSE 361S).  The call stack acts like the [stack data structure](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) you may have seen in other classes.  

![========]({{ "/images/line.gif" | relative_url }})

## The assignment

### Assembly Language

In your repository you will find two files: `assemblyFunctions.h` and `assemblyFunctionTestCases.ino`. The .h file contains the prototypes for all of the functions we are asking you to write for this lab. The test file contains some examples of those functions in action. You'll notice that a lot of stuff is commented out in the test file. As you complete the functions, you should uncomment the corresponding lines to test your code.

You need to create a file called `assemblyFunctions.S` and set it up so that you can start defining your functions in that file. If you're confused on how to do this, refer back to the studio.

Below is a brief description of each of the functions we're asking you to implement (some of which should look familiar!).   Each can be done without using a branch (`br...` or `jmp`) instruction, but the various skip instructions (`cpse`, `sbrc`, `sbrs`) may be useful.  ***DO NOT USE the braching instructions on this assignment***.

**`boolean hasAOne(byte num)`**: Returns a Boolean `true` if the binary representation of the given 8-bit value contains a one, `false` otherwise. (Hint: look at the comparison instructions).  When using `boolean` return values a `0` represents a `false` and a `1` represents a `true`. (Hint: you can use the *skip* instructions to "skip over" an instruction that will change the return value)

**`char byteToAscii(byte num)`**: Takes in an integer value from 0-9 (you can assume that the given `byte` will always be in that range - no need to check for errors), and returns the corresponding ASCII value as a `char`.

**`int int8ToInt(int8_t num)`**: Takes in an 8-bit **signed** value and converts it to a 16 bit **signed** value. 

**`int addInt8ToInt(int8_t num1, int num2)`**: Adds an 8-bit **signed** value to a 16-bit signed value and returns a 16-bit **signed** result. To do this properly, you should convert the 8-bit argument to a 16-bit argument before performing the addition. Use your **int8ToInt** function to accomplish this. Be careful with your registers! Even if nothing bad happens when you use caller-saved or callee-saved registers we will be checking to make sure that you used them properly when you demo.

**`int add4Int8(int8_t num1, int8_t num2, int8_t num3, int8_t num4)`**: Adds the four 8-bit **signed** values and returns an 16-bit **signed** result. *this must use `int8ToInt()` (1 call) and `addInt8ToInt()` (3 calls)*.

**`byte average(byte num1, byte num2)`**: Averages the two **unsigned** 8-bit values and returns an **unsigned** 8-bit result.  Look into the shifting and rotation operations for division. 

Although not necessary, we challenge you to complete each with as few instructions as possible.  As a reference, it's possible to do:
- `hasAOne` in 4 instructions
- `byteToAscii` in 2 instructions
- `int8ToInt` in 4 instructions
- `addInt8ToInt` in 4 instructions
- `add4Int8` in 12 instructions
- `average` in 3 instructions

### Testing 

In addition to assembly language, you should test your work reasonably thoroughly.  

It's easy to exhaustively test `hasAOne`, `byteToAscii`, and `int8ToInt`.  That is, it's possible to test every possible test case.  The provided code includes exhaustive tests for `hasAOne` and `int8ToInt`, but the code for `byteToAscii` is only partially complete.  You must finish it. (In this case "exhaustive" just refers to the valid input values --- the integers 0 through 9)

It's more difficult to exhaustively test `addInt8ToInt()`, `add4Int8()`, and `average()`, but it's easy to test a reasonable set of test cases.  The provided code includes test cases for each, but you should modify the code to do more extensive testing.

![========]({{ "/images/line.gif" | relative_url }})

## The check-in 

1. Commit your code and verify in your web browser that it is all there.
2. Check out with a TA

- `assemblyFunctionTestCases/`
	- `assemblyFunctions.S` (added)		
	- `assemblyFunctionTestCases.ino` (modified)		
	- `printRegs.h` (unchanged)		
	- `printRegs.ino` (unchanged)		

### The rubric

- 100pts total:
	- `hasAOne`
	- `byteToAscii`
		- Exhaustive test cases
	- `int8ToInt`
	- `addInt8ToInt`
		- Additional test cases
	- `add4Int8`
		- Additional test cases
	- `average`
		- Additional test cases
	- Correct usage of caller-saved and callee-saved registers
	- Code style

{% include footer.html %}
