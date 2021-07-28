---
layout: default
title: Assignment 9 - Assembly Puzzles Part 2
author: Bill Siever and Doug Shook
---
{% include nav.html %}

# {{ page.title }}

Click [here](https://wustl.instructure.com/courses/68860/assignments/289484) to access the Canvas page with the repository for this assignment.

![========]({{ "/images/line.gif" | relative_url }})

## Overview

In this assignment you're tasked with creating more functions using assembly language. There are two big differences between this assignment and the last:

- This assignment requires control logic (loops, if statements, etc.). 
- This assignment uses arrays (one dimensional). 

As before, you will likely want to keep the [AVR Assembly Reference](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-BA59618D-4850-490B-B176-0BCC3D9438A1) handy as well as the [rules for register usage](http://ww1.microchip.com/downloads/en/appnotes/doc42055.pdf). ***Make sure you are using registers appropriately - we will be checking for this when you demo. Some of the functions you have to complete are both called and call another function --- they need to follow both the call-saved and the call-used conventions!!!  Use the stack!***

![========]({{ "/images/line.gif" | relative_url }})

## Goals

The goal of this assignment is to implement basic functions in assembly language. The functions chosen are examples that may actually be implemented in assembly rather than a higher level language. 

The main concepts are:

- Division:  As you may have noticed, the AVR we're using doesn't have a division instruction! You'll implement two types of division: 8-bit unsigned integer division and 16-bit unsigned integer division.  Both will do division the "slow" way. Remember that multiplication is basically repeated addition.  Division, as the inverse of multiplication, can be accomplished via repeated subtraction.  
	For example: to compute 9/4 we could try to repeatedly subtract 4 from 9:
	<pre>
	Step 1:  9-4 = 5
	Step 2:  5-4 = 1
	We have less than 4 left, so we're done.  
	There were just 2 steps, so 9/4 must be 2.
	</pre>

	Another example. 36/7:
	<pre>
	Step 1:  36-7 = 29
	Step 2:  29-7 = 22
	Step 3:  22-7 = 15
	Step 4:  15-7 =  8
	Step 5:   8-7 =  1
	We have less than 7 left, so we're done.  
	There were just 5 steps, so 36/7 must be 5.
	</pre>

	(This, of course, isn't the approach used by "long division" that you learned in school.  Using the Big-*O* notation covered in CSE 247, the approach shown here does *O*(*n*) steps.  Typically computers use a different approach that is quite like "long division" and only does *O*(log *n*) steps, which is much faster for large enough *n*.  Consequently we're referring to the approach here as "slow").

- Modulus: Modulus is related to division however instead of the quotient, we are looking for the remainder. For this problem, we want you to use the division function you wrote (by calling it from your modulus function), taking the result and performing multiplication (using the `mul` [AVR Instruction](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-C301CF8C-34FD-43A4-BE0F-E705BB9FC85E), NOT the method you wrote in studio), then finally some subtraction to determine the remainder. Note again, that this is not the most efficient way of performing modulus, but it does give you good practice using various AVR instructions.

- Summing odd numbers within a range: You will be given two values, and then must sum all of the odd values within that range (inclusive). For example, if the values you are given are 2 and 7 then the result would be 3 + 5 + 7 = 15.

To keep things as simple as possible all values are *unsigned*.  In C the 8-bit values are `uint8_t` (which is the same as a `byte` and 16-bit values are `uint16_t`.  

![========]({{ "/images/line.gif" | relative_url }})

## The Assignment

In your repository you will find an `cse132-assignment12/assignment12Demo` Arduino project. This project contains the prototypes for the functions we are asking you to write as well as code to test your work.

The Assembly language functions may be easier to write if you have already tested the logic using C. There are four C-functions you'll need to complete in `assignment12Demo.ino`:

- `uint8_t slowDivisionAlgorithm8(uint8_t dividend, uint8_t divisor)`: This function will do division of 8-bit values the "slow" way. 
- `uint16_t slowDivisionAlgorithm16(uint16_t dividend, uint16_t divisor)`: This function will do division of 16-bit values the "slow" way. (And may look a *lot* like the 8-bit version.  None the less, note the differences!)
- `uint8_t slowModulusAlgorithm(uint8_t dividend, uint8_t divisor)`: This function will return the remainder of the dividend divided by the divisor, and should call your `slowDivisionAlgorithm8` function.
- `uint16_t sumOdds(uint8_t a, uint8_t b)`: This function will sum all of the odd values that occur between a and b (inclusive). You should use your `modulus` function to solve this problem.

There are five functions to complete entirely in assembly language:

- `uint8_t slowDivisionUint8(uint8_t a, uint8_t b)`: An 8-bit division using assembly language.  Use the C-code version as a guide for the logic of your assembly language version.
- `bool greaterThanOrEqualUInt16(uint16_t a, uint16_t b)`: Testing logic conditions on multi-byte values is a little trickier than just the `cp` based comparisons we've covered so far.  Create a multi-byte comparison function.  It may help to think carefully about a variety of test cases (and even write them out as 2-byte hex values).
- `uint16_t slowDivisionUint16(uint16_t a, uint16_t b)`: Implement a 16-bit division  algorithm.  Again, use your C-code version as a prototype.  (Hint: You may need to do comparisons of 16-bit values.  You may want to try to `call` on some of your earlier work)
- `unit16_t slowModulusUint8(unit8_t a, uint8_t b)`: Implement a modulus operation on 8-bit operators, using your `slowDivisionUint8` function.
- `uint16_t sumOddsUint8(uint8_t a, uint8_t b)`: This function will sum all of the odd values that occur between a and b (inclusive). You should use your `slowModulusUint8` function to solve this problem.

![========]({{ "/images/line.gif" | relative_url }})

## Getting Started

Assembly language can be challenging.  The provided files include several test cases to help you with your work.  Here's the approach we suggest:

1. assignment12Demo.ino includes a lot of code to help you with testing.  The `setup()` includes calls to all test cases.  There are two styles of tests:
	1. Standalone test cases, like:

			uint8_t dividend8 = 175;
			uint8_t divisor8 = 26;
			uint8_t quotient8ASM = slowDivisionUint8(dividend8,divisor8);
			uint8_t quotient8Alg = slowDivisionAlgorithm8(dividend8,divisor8);
			pprintf("slowDivisionAlgorithm8(%u,%u) is %u (algorithm) and %u (assembly); Should be %u\n", dividend8, divisor8, quotient8Alg, quotient8ASM, dividend8/divisor8);

		These tests will run just a single test case, which is probably what you need to do when debugging.

	2. More rigorous tests are included in "test functions", like on Assignment 10.  They can be run via function call, like:

			// Full Test:
			testSlowDivisionUint8();

		You will want to comment/un-comment test cases so you can focus on each part independently.


2. It's probably best start with the C-code version of 8-bit division. I.e., complete `slowDivisionAlgorithm8()` in assignment12Demo.ino.  Use the test functions to test it (they will indicate the values printed by both the algorithm, which should be correct when you're done with the C-code, and the assembly language, which will still be incorrect)

3. Write and test the assembly language version of 8-bit division: `slowDivisionUint8` in assignment12.S.

4. Try the assembly language version of comparisons --- do `greaterThanOrEqualUInt16` in assignment12.S.  Again, test your work.

5. Do the C version of 16-bit division.  Complete`slowDivisionAlgorithm16()` in assignment12Demo.ino.

6. Now try the assembly language version of 16-bit division: `slowDivisionUint16` in assignment12.S.  This may be the most difficult part of the assignment. (Hint: 16-bit subtraction can be done with just two instructions)

7. Complete the two modulus functions: `slowModulusAlgorithm` and `slowModulusUint8`. Don't forget to use your division functions in your solution - we will be checking for this.

8. Complete the two functions for summing odd numbers within a given range. Keep in mind that the range is inclusive, so if the min or max is an odd number it should be included in the sum. You may assume that the first argument will always be less than or equal to the second argument (you do not have to check for this in your code).

![========]({{ "/images/line.gif" | relative_url }})

## Hints

1. If you do not get the message `Done with tests!!!` at the end of your testing functions, you did not successfully finish all tests. You are probably caught in an infinite loop somewhere...

2. If you call another function from within the function you are writing, keep in mind that the two functions may use the same registers. Be careful not to overwrite data you may need later. Convention is to save all *call-used* registers before calling another function. This can be done using the `push` and `pop` instructions.

![========]({{ "/images/line.gif" | relative_url }})

## A Word on Multiplication

It is in your best interest to read up on the [AVR Multiplication Instruction](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-C301CF8C-34FD-43A4-BE0F-E705BB9FC85E) before attempting to complete the modulus function. 

Note that the multiplication instruction is expecting two unsigned 8-bit values as input, producing a 16 bit result. This does not quite match up with what we will have since one of our values will be an 8-bit unsigned integer (the divisor), but one of them will be a 16-bit unsigned integer (the output from the `slowDivision` function). For the purposes of this function, you may simply ignore the most significant byte of the 16-bit value and perform the multiplication with the least significant byte.

The output of the multiplication instruction will be placed in `r0` and `r1`. Note, however, that `r1` is a special register that is expected to contain zero. What this means is that if you use a multiplication instruction, you must read the value out of `r1` and set it back to zero as soon as possible.

![========]({{ "/images/line.gif" | relative_url }})

## Checkout

1. Commit your code and verify in your web browser that is is all there.
2. Check out with a TA.

### The rubric

- 100pts total:
	- C functions
		- slowDivisionAlgorithm8
		- slowDivisionAlgorithm16
		- slowModulusAlgorithm
		- sumOdds
	- AVR Assembly functions
		- slowDivisionUint8
		- slowDivisionUint16
		- slowModulusAlgorithm
		- greaterThanOrEqualUInt16
		- sumOddsUint8
	- Correct usage of call-saved registers
	- Code style

{% include footer.html %}
