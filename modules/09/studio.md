---
layout: default
title: "Studio 9 - Control Yourself"
author: Ben Stolovitz & Bill Siever
---
{% include nav.html %}

# {{ page.title }}

![========]({{ "/images/line.gif" | relative_url }})

Click [here](https://wustl.instructure.com/courses/68860/assignments/289488) to access the Canvas page with the repository for this studio.

![========]({{ "/images/line.gif" | relative_url }})

## Introduction

In the last module we looked at the basic operation of assembly: how you can load values into registers, how to apply basic math and logic operations to values, and how to break code into interoperable functions.

Now we look at the thing that makes computers special: using data to [control the flow](https://en.wikipedia.org/wiki/Control_flow) of execution (the ability to make decisions that control the instructions that are executed or repeated).

### Objectives

By the end of this studio, you should:

- know how to compare two values in assembly,
- know how to use the result of a comparison to branch in your program (i.e., control the "flow" of the program by potentially "branching off" to different instructions),
- know how to translate C-style loops into assembly, and
- know how to translate almost *any* C into assembly.

There are a lot of very important topics today.  Let's get started.

![========]({{ "/images/line.gif" | relative_url }})

## Compares are nondestructive

The key to control flow is the ability to compare values.  AVR assembly language includes a compare instruction: `cp`. Let's use it.

1. Set up your repository. We have written a bare bones project, `CSE132-studio12/programControl` (very creatively named). In it are assembly stubs for four functions. Let's start by writing `lessThan()`.

2. If you carefully review most loops or if-statements you'll notice that they nearly always use a relational operator, like `==`, `!=`, `<`, `<=`, etc.  These operators produce a boolean value based on the relationalship between values: are they the same? are they different? does the first come before the second? is the first either before or the same as the second? etc. 

	In AVR assembly the most general way to do *relative* comparison is the compare instruction, `cp`. (`cp` is the only instruction needed today, but there are other instructions used for comparisons: `tst`, `cpi` and `cpc`. As you may guess from the names, `cpi` and `cpc` work much like `cp`).

	The compare instruction `cp Rd, Rr` takes two registers and *subtracts* them: `Rd - Rr`. The result is thrown away, but the processor sets the various **flags** properly. The flags are just individual bits in a special register called `sreg`, which stands for "status register". 	We don't usually care about the integer value of `sreg`, but it may be helpful to understand how the individual bits of `sreg` may be used by decision making instructions (branching instructions).  You may have noticed that the `printRegs` function prints a line that looks like:

	<pre>
	sreg: C=0 Z=1 N=0 V=0 S=0 H=0 T=0 I=1
	</pre>

	This shows the individual bits contained in `sreg`, which are named with letters that denote their meaning:

	- `Z`: the zero flag, true if the result is `0`.
	- `C`: the carry flag, true if there was an operational carry.
	- `N`: the negative flag, true if the result is considered negative (if it's interpreted as a signed number).
	- `V`: the overflow flag, true if there was overflow.
	- `S`: equivalent to `N XOR V`, the signed overflow flag
	- `H`: the half-carry flag, true if the low nibble carried from the high.

	Once you do a `cp` between two registers these flags are set appropriately. Try using `cp` to compare the arguments passed into the `lessThan` function and use `printRegs` to print the register contents immediately after the `cp`.  (You may want to review  ["What registers are used by the C compiler?"](http://ww1.microchip.com/downloads/en/appnotes/doc42055.pdf)) How are the flags set for each of the test cases in `testLessThan()`?  What happens when you change the order of the registers used by `cp`? 

	We don't expect you to be too familiar with each of the flags because most of the time you don't have to care about them, as you'll see in a minute.

![========]({{ "/images/line.gif" | relative_url }})

## Jumping around (branching)

Assembly languages usually move sequentially, mindlessly executing one line after another. 

Assembly languages control program "flow"---how the program executes different code depending on the situation---by **jumping** and **branching**. 

For example, the command `jmp` takes as its argument a label, which is just a name for a location with the program, and then *jumps* from the `jmp` command to that label without executing any of the code in between. You can jump forwards or backwards to any location in your assembly code merely by placing a label at the location you want to go and jumping there.

~~~ nasm
	jmp myLabel
	nop ; this code will be skipped;
	    ; The jmp instruction  "jumps" 
	    ; right to the "myLabel" line
	    ...
myLabel:
	nop ; this code runs right after jmp
~~~

Labels are used to "label" the locations of many things in a program. Soon you'll be completing the `lessThan()` function and the name `lessThan:` is just a label for the function's instructions.

The `jmp` type of instruction is sometimes called an unconditional jump or unconditional branch because it always jumps (also called branching).  This is in contrast to instructions that *may* skip to a different label depending on some *condition*. 

Conditional branches jump based on the current status of the CPU's flags, like the zero bit and all the other `sreg` flags set by the `cp` instruction. For example, `breq` branches if and only if the zero flag is set. Remember that the `cp` does subtraction and sets the flags based on the result.  If (and only if) the two values are equal, the result will be zero, which will cause the `Z` flag to be true. By clever use of the `cp` and `br**` instructions (where `**` is two letters, like `lt` or `ge`), you can write comparisons for any type of boolean or mathematical tests (<, <=, ==, etc.).

Most of the time you do not need to be clever or pay attention to the flags at all because AVR assembly has convenient branch commands for most mathematical operations *and* includes a cheat sheet for all logical comparisons (Section 3 / page 21 of [the AVR manual](http://www.atmel.com/images/atmel-0856-avr-instruction-set-manual.pdf)). Using the various branch commands (like `brne`, `brlt`), you can do various tests between registers, like `Rd != Rr` or `Rd < Rr`.

Just like a `jmp`, a branch command takes a label as an argument.  Unlike `jmp` though, the branches depend on a condition.  If the condition is true they will jump to the label. Otherwise they will continue to the next instruction.  For example:

~~~ nasm
	cp r18, r20 ; compare r18 and r20
	brne myLabel
	nop ; this code runs if r18 and r20 are equal (i.e. r18==r20)
myLabel:
	nop ; this code runs in either case
~~~

Notice that the logic above is reverse of how you might typically think.  In pre-condition control structures (if-statements, while-loops, for-loops) assembly language uses logic exactly the opposite of what you may do in C or Java.  It jumps to *avoid* the instructions that follow. 

Complete the `lessThan()` in assembly commands to return `1` if `a` is less than `b` (if `r24` is less than `r22`) and `0` otherwise. Make sure the results in `programControl.ino` make sense. Notice that it uses **signed** 8-bit integers. 

The following pseudo-code shows the basic structure using C, but you need to create an equivalent assembly language version (normally this is what the compiler does for you!):
~~~ c		
bool lessThanPseudocode(int8_t a, int8_t b) {
	// Yes, we know this is bad programming form
	// you should just `return (a < b);`
	// but we're illustrating a point here, ok?
	if(a < b) {
		return 1;
	} else {
		return 0;
	}
}
~~~


After completing `lessThan()`, complete a comparable `lessThanOrEqualUnsigned()` that uses *unsigned* 8-bit integers.  A test function (`testLessThanOrEqualUnsigned()`) has already been created for you.  Uncomment both the function and the call to it in `setup()` to test your work.

Agan, here's C "pseudo code" of the basic logic:

~~~ c		
bool lessThanOrEqualUnsignedPseudocode(byte a, byte b) {
    // Note that a & b are bytes here, not int8_t.
	if(a <= b) {
		return 1;
	} else {
		return 0;
	}
}
~~~

![========]({{ "/images/line.gif" | relative_url }})

## Get in the loop.

The `lessThan` code can be thought of as, basically, an `if` statement. If `a` was greater than `b`, return `1`, else `0`. Loops in C can be thought of in the same way. A `while` loop might look like this:

~~~ c
byte i = 0;
while(i < 5) {
	// code
	i++;
}
~~~

But you can also use labels and unconditional jumps (`goto`s) in C to write it with an if-statement (C can have labels, just like assembly, but people prefer not to use them. In fact, using them in places where another control structure can be used is a very bad practice. One of the great pioneering computer scientists, Edsger Dijkstra, even wrote an article titled [Go To Considered Harmful](http://dl.acm.org/citation.cfm?id=362947)):

~~~ c
byte i = 0;
loopBegin:
	if(i >= 5) {
		goto loopEnd;
	}
	// code
	i++;
	goto loopBegin;
loopEnd:
	// end
~~~

Since you should already be comfortable writing `if` statements in assembly, you should now be able to write `while` loops in assembly (and `for` loops, too).

1. We would like to implement multiplication of two numbers as a loop using addition. Again, here's some C-like Pseuso-code that may help you design the assembly language code:

		byte slowMultiplyPseudocode(byte a, byte b) {
			byte sum = 0;
			byte counter = 0;
			while(counter < a) {
				sum += b;
				counter++;
			}
			return sum;
		}

	It turns out that there *is* a multiply instruction in AVR, but we've never been a fan of doing things the easy way in this class. Implement `slowMultiply` with add using the above algorithm. You will need to use additional registers for your `counter` and `sum` (no need to actually allocate memory for them, keeping them in registers is fine).

	We do not care about overflow, and you're only multiplying `byte`s, so no carries either.  Also note that this is an *unsigned* multiply.

	Write and test this function by calling it in `programControl.ino`.	

2. Just to prove once and for all that you understand program control, we want you to implement exponentiation in a similar way. Sure, you could just call your new multiply function the correct number of times (in a loop), but that's no fun. Write `slowExponent` to calculate exponents using addition.  Here's the pseudo-code for an implementation:

		byte slowExponentPseudocode(byte a, byte power) {
			byte sum = 1;
			byte i = 0;
			while(i < power) {
				byte innerSum = 0;
				byte j = 0;
				while(j < a) {
					innerSum += sum;
					j++;
				}
				sum = innerSum;
				i++;
			}
			return sum;
		}

	Again, the test code has been written for you and just needs to be uncommented to run.

3. Once you've completed all that, you should, at least theoretically, be able to translate any C code into assembly with all the AVR you've learned in the past two modules. You are now a compiler! 
		
	You may spend time working on your assignments. 

	Notice that throughout this studio, we often first wrote out programs in C before translating them to assembly. C is a more expressive language, so it's often easier to write out what you want to do first and then directly translate it to assembly. This may be a good way to complete your assembly assignments: understand what you are trying to do, line by line in C, and then implement the assembly. This let's you separate mentally the algorithms you want to implement and the assembly boilerplate you have to write.

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

## Check out!

1. Commit your code and get checked out by a TA.


When done, your code should run all test cases without error.  It should look something like this:

<pre>
 *** Starting program...
 *** lessThan() *** 
4 < 6 is 1
6 < 4 is 0
4 < 4 is 0
-4 < 6 is 1
4 < -6 is 0
-4 < -4 is 0
-3 < -4 is 0
-4 < -3 is 1
127 < -128 is 0
-128 < 127 is 1
 *** lessThanOrEqualUnsigned() *** 
4 <= 6 is 1
6 <= 4 is 0
4 <= 4 is 1
5 <= 255 is 1
255 <= 5 is 0
128 <= 127 is 0
127 <= 128 is 1
0 <= 0 is 1
250 <= 250 is 1
 *** slowMultiply() *** 
4 * 6 is 24
6 * 4 is 24
4 * 4 is 16
1 * 255 is 255
6 * 40 is 240
200 * 1 is 200
1 * 200 is 200
0 * 0 is 0
0 * 1 is 0
1 * 0 is 0
 *** slowExponent() *** 
4 ^ 2 is 16
2 ^ 4 is 16
3 ^ 2 is 9
3 ^ 3 is 27
6 ^ 2 is 36
1 ^ 210 is 1
1 ^ 0 is 1
0 ^ 1 is 0
8 ^ 2 is 64
3 ^ 5 is 243
2 ^ 7 is 128
Ended setup!
</pre>

![========]({{ "/images/line.gif" | relative_url }})

## Key Concepts

This is a mental checklist for you to see what the Studio is designed to teach you. 

- Assembly 
	- Compare
		- `cp`
	- Flags
	- Jumping and Branching
		- `jmp`
		- `brlt`, `brne`, etc.
	- Loops
	- Mimicking Control Structures from C (Ex: if-statements, if-else statements, loops, etc.)
	
