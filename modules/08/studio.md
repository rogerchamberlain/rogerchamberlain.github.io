---
layout: default
title: "Studio 8 - Assembly: Just the Basics"
author: Ben Stolovitz, edited by Doug Shook
---
{% include nav.html %}

# {{ page.title }}

![========]({{ "/images/line.gif" | relative_url }})

## Introduction

Click [here](https://wustl.instructure.com/courses/68860/assignments/289487) to access the Canvas page with the repository for this studio.

![========]({{ "/images/line.gif" | relative_url }})

## Computing Concepts

There are some pieces of "common knowledge" that most people have about computers:

1. Computers just follow instructions.

2. Programming is the process of creating a set of instructions that a computer will follow.

3. These instructions are in a binary language that computers "read".

4. Computers follow instructions in an exact order. 

5. Computers have memory (RAM or ROM) to store information.

### The fetch-decode-execute cycle

The fetch-decode-execute process is part of what ties these  concepts together. 

1. You've seen in CSE 131 and CSE 132 that computers do just follow instructions.

2. You've been creating programs in *high level languages* (Java and C++), but these programming languages often express computation in complex ways that a processor can't directly understand.  So another program, a *compiler*, is used to break down these complex programs into simpler steps.  For example, an equation that you might see in a Java program, like:

~~~ c
d = a + b + c;
~~~

May actually be broken down into a smaller, but equivalent set of steps, like:

~~~ c
d = a
d = d + b
d = d + c
~~~

The end result stored in `d` is the same, but each individual step is simpler.  Compilers often do three things a) break your original set of steps into smaller, less complex steps, b) translate these individual steps into a new, different language, and c) build a long list (a compilation) of these simpler steps in the new language.  This compilation in the new language may look something like this:

~~~ c
mov r5,r2	// r5 <- r2
add r5,r3	// r5 <- r5 + r3
add r5,r4	// r5 <- r5 + r4
~~~

In this translation the variables were renamed (from `a`,`b`,`c`,`d`, to `r2`, `r3`, `r4`, and `r5`) and the order and structure of operations was replaced.  For example, the `d=a` is represented with `mov` to indicate that the information in `a` is being *moved* (copied) to `d`.   The word `add` was used rather than the symbol `+` and the word `add` doesn't seem to work quite like `+`  --- `add` seems to modify something by adding to it, like the `+=` in C++ or Java. 

The things on the left, the `mov` and the `add`, are called *opcodes*. They are a code for an operation. The things to the right of the opcodes, like the `r5`, `r2`, etc., are called *operands*.  The *operands* are the data used by the operation. At the end of the lines (after the `//`) are comments.

Data can be stored in different places.  The two most common are the computer's RAM and a small set of special, internal variables called [*registers*](https://en.wikipedia.org/wiki/Processor_register).  The `rX` values above are the latter.

3. This *assembly language* is then given to a program called an *assembler*.  The assembler just translates the values into numbers, producing the binary code for the program.  For simple examples like the one shown above, this process is just a simple substitution code --- each individual part is replaced with a numeric code. In the examples given, a `mov` can be represented with the binary value `0010 1100`, and `add` with `0000 1100`; the four `r` variables can just be represented by the 4-bit equivalent of their numeric value, so `r2`=`0010`, `r3`=`0011`, etc.  With these codes it's easy to represent the entire program as numbers:

| Original Assembly Instruction |  Binary Representation of Machine Code | Hexadecimal Representation of Machine Code |
|-|-|-|
|	mov r5,r2 | 0010 1100 0101 0010  | 0x2C52 |
|	add r5,r3 | 0000 1100 0101 0011  | 0x0C53 |
|	add r5,r4 | 0000 1100 0101 0100  | 0x0C54 |

We've taken our idea of a list of instructions (program) and converted it into a smaller set of steps that we can represent as binary numbers!  This is the type of  *machine language* that computers, like the Arduino's AVR processor, read.  In fact, this example is exactly the language the Arduino understands.  Other computers, like the Intel processor in your desktop, may not use the same types of steps or the same numeric codes.  (The above numbers will do very different things on a different type of computer)

4. A computer's processor reads this machine language and acts on it.  It turns out it's pretty easy to build a machine to do exactly this.  Basically, the machine has to **repeatedly** do three things:
- Fetch the next *instruction* it needs to perform. 
- Decode it: Figure out what the individual parts are and what they mean.  For example, if the number starts with `0010 1100`  the computer needs a way to identify that this means it should do a `mov`.
- Execute it: Actually follow through on the operation the instruction is describing, like copying (moving) data from one location to another.

5. The computer memories are an important part of this process.  A memory stores data *and* the instructions themselves.  For example, you can think of the partial program shown above as just being an array of 16-bit numbers.  The order of data in the array corresponds to the order to do the steps of the program.  The above program could be thought of as an Arduino array of `int`s:

~~~ c
int instructions[] = {
	0x2C52,
	0x0C53,
	0x0C54
};
~~~

The computer can keep track of which step it's currently on by just using an index into this array.  In fact, this is exactly what happens.  The index that keeps track of which step the computer is currently on is called either the *Instruction Pointer* (IP) or the *Program Counter* (PC). Both terms are used and both are pretty clear.  For example, the index "0" for the example above can be thought of as "pointing to" the first instruction (the `mov` that is represented as `2C52`).  Of course, we usually advance from one instruction to another, so this pointer would usually go from the instruction at index 0, then to the instruction at index 1, then to the instruction at index 2, etc. I.e., it's usually "counting" through the program's instructions.

### Assembly

The full manual for the AVR assembly language instructions is available [here](http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-0856-AVR-Instruction-Set-Manual.pdf). It includes detailed descriptions of nearly all aspects of how the AVR processor reads instructions, including all the *opcodes*, how they use *operands*, and how they should be converted to machine language.  Lucky for us, AVR is a **reduced instruction set computer** (RISC) and has physical circuits for most instructions, which means that it has relatively few instructions available compared to a processor like the Intel processors in your desktop or laptop. As a comparison, the AVR has about 123 instructions and a [manual](http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-0856-AVR-Instruction-Set-Manual.pdf) that is about 190 pages long.  A modern Intel processor has more than 500 instructions and a [manual](http://www.intel.com/content/dam/www/public/us/en/documents/manuals/64-ia-32-architectures-software-developer-instruction-set-reference-manual-325383.pdf) that is about 2200 pages long. 

![========]({{ "/images/line.gif" | relative_url }})

## The first assembly

1. 	The [Arduino IDE](https://www.arduino.cc/en/Main/Software) natively supports programming in assembly.
	
Open up `simpleValues/simpleValues.ino` from your repository.

You'll notice that the code still looks like C. That's because it is! Our assembly unit is going to be a combination of C and assembly. If you open the `assembly.S` tab you'll see the first bit of assembly you'll work on today.

Upload this program now. What does it print to Serial Monitor?

2. The Arduino IDE automatically uses all the files in the sketch folder, including any assembly language or C-language files. If you look at `assembly.h`, you see a **function prototype**---just like within any other header file. We've included that header file in the main Arduino file, `simpleValues.ino`, so our C code can call our assembly functions.

This function, however, does not have a corresponding C source file. It's written in *assembly*.

C code always follows certain rules, or [*conventions*](https://en.wikipedia.org/wiki/Calling_convention), when being converted into machine code. If we follow these rules we will be able to call our assembly language code from C or vice-versa without problems.

For example, Arduino C expects any function returning a single byte to place it in register 24. There are 32 "general purpose" registers on the Arduino (named `r0` to `r31`) and each holds just one byte.  There are also special registers called the *status register* and the previously mentioned *program counter*. 

Therefore, if we change the value that gets put in register 24 when the function returns, we'll change the function's return value. The `ldi` instruction loads an immediate value into a register. 

Currently the `giveMeMax` function returns `1`.  Update it so it returns the maximum value that could be stored in a register. 

Rerun the program. What does it print out now?

>### A cop-out

>We strongly considered writing this unit *entirely* in assembly. Since C and C++ both can compile into assembly (but usually go straight to machine code), it is theoretically possible. However, we ultimately decided against it.
	
>Here's why:
	
>- It's quite illuminating to use the two languages together (how do types work in C? What are functions?), and we still get all the assembly gravy anyway.
>- We gain a lot. The Arduino C library (with the functions we use like `digitalWrite()` and `Serial`) is secretly C++, as we've mentioned earlier. C++ gives object-orientation and classes to C, but it does so in a bit of a hacky way. Every function you write in C++ is **name-mangled** into something slightly different (`Serial.begin()` becomes `_ZN14HardwareSerial5beginEmh()` on my computer), which makes it extremely hard for us to use `Serial` in assembly-only programs.

>Actually, that name-mangling is the reason for a lot of the "weird" bits of our program. The header files we provide wrap all the function names in `extern "C" {}` to tell the compiler, no, these are not C++ functions that should be name-mangled, but C functions, which play nicely with assembly.

![========]({{ "/images/line.gif" | relative_url }})

## Mimicking C

The line of assembly language implementation of this function had 4 main parts. In order to create a "C-style" function in assembly, you need all of them.

1. A `.global` pseudo-op with a function name (identical to a C function prototype in a header file elsewhere).
2. A function name as a label, indicating the beginning of a function's code.
3. The actual code of the function
4. A `ret` operation to make the function return.

All lines of assembly code look something like this:
	
`[label:] instruction [operand], [operand]`
	
where square brackets (`[]`) indicate an optional part. For example, the line you edited had the **instruction**, `ldi` (load immediate), followed by two **operands**. The first operand indicated which register to put the result in and the second operand was a specific number to load (a *literal* --- the value was literally 1).
	
Above it was a simple **label**, `giveMeMax`, which just names a line of the program so that other parts of the program can refer to it. In this case they will be able to treat the name `giveMeMax` like a function and the label specifies where the function's code is at.   

Notice that this is identical to the function called in `simpleValues.ino` (i.e., `simpleValue.ino` calls `giveMeMax()`). The  `.global` **pseudo-operation** above the `giveMeMax:` label is used to indicate that the label may be used from other files, like the `simpleValue.ino` file.

Calling a function causes it to start running the function's instructions.  In this case, calling `giveMeMax()`  will start executing instructions at the `giveMeMax` label.  By default it will execute them one-at-a-time and in-order --- the program counter "counts" through the instructions!  It continues linearly by default,  so we need to alter this in order to resume running the calling function (i.e., to get back to the next line of code in `simpleValue.ino`).  We do this with a `ret` operation. The `ret` is much like a `return` statement in a Java or C program --- it causes the function to return.  However, unlike Java and C, `ret` can not be given an operand.  If you want to return a value you have to use other instructions to marshall the value into the appropriate register.
 	
Our header file has a prototype for another function called `giveMeZero()`. 

1. `simpleValues.ino` has commented lines of code using this function. Uncomment them. If you try building your project now  the compiler will give you an error: you have not defined your function yet.
2. To remedy that, define your function in assembly. Make a label called `giveMeZero` *after* the `giveMeMax` code.
3. You need to indicate to the compiler that this label will need to be accessible from another file.  Above your new label, use the `.global` pseudo-op (with the label name as the operand) to tell the compiler that our new label is *global* to the entire program.
4. Assembly runs line after line forever until it is told to *not* do the next line. If you called `giveMeZero()` in your C code (which you can --- try it), the function will never return. Also try moving the `giveMeZero` label *before* `giveMeMax`. 
5. Make sure that `giveMeZero` is defined after `giveMeMax`, add the `ret` operation after your the label to make the function return, and try it again.  What happens? Does it still return a value, even though you did not explicitly set one?
6. Use the `ldi` instruction like you did in `giveMeMax`, but have it return a zero.

What does your program print now?

![========]({{ "/images/line.gif" | relative_url }})

## Calling all functions

You can also *call* functions from assembly. If you `call` a C function (`call functionName`) in assembly your program will call that function. By following a **calling convention**, an agreed-upon standard of how functions should be handled in assembly, it's possible to have your assembly functions have C-style returns (which you did earlier by writing to register 24) and accept C-style arguments.
 
We provide a C function that prints the state of all the registers on your Arduino: `printRegs()`. You can call it from C (just like any other C function) or you can call it from assembly.
***You will need to include a header file in the `simpleValues.ino` file to call it.  It already has a `#include` line commented out.  Simply remove the comment (`//`) to include the file so you can use `printRegs` from C or assembly).***
	
1. Modify one of your assembly functions to call `printRegs` (using the `call` instruction) before and after `ldi`. What is in `r24` before and after? Do any of the **status register**  bits change?
2. There's another function stub in `assembly.h`, `addFour()`. We want it to take its argument, add four, and return the added value.

	Modify `simpleValues.ino` to call and print the result of `addFour()` on a number of your choice. Since we have not written the `addFour()` definition yet, your program won't compile.
3. Write the basics of the `addFour()` function in assembly: the `.global` pseudo-op, the appropriate label, and a `ret` call. Don't worry about actually doing anything yet, just make sure your program compiles.
4. The AVR version of gcc (the software that handles C compilation for AVR) has conventions for what registers are used by the C compiler for what purposes. This [manual](http://ww1.microchip.com/downloads/en/appnotes/doc42055.pdf), in Sections 3 through 6, describes the essentials, including the conventions for **parameter passing and return values** in function call.

	What register do you expect the single byte argument to appear in? Use `call printRegs` to verify your answer.
5. We want to add `4` to this argument and return it. The [`add` instruction](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-7383E2A4-AFC3-4ED1-B462-589CC8453073) is perfect for this (don't worry about overflow or carry), but it can only operate on registers. To use it properly, you will need to use a second register *besides* the argument register. The AVR gcc documentation [sections you looked at earlier](http://ww1.microchip.com/downloads/en/appnotes/doc42055.pdf) has a list of **caller-saved registers** (that they refer to as **call-used registers** in the document). Table 5.1 has errors, so [here]({{ "/files/RegisterUsageConventions.pdf" | relative_url }}) is a corrected version, and we'll stick with the caller-saved nomenclature. The caller-saved registers you can use freely in your assembly language functions that are called from C without worrying about breaking things.

	Use `ldi` to put 4 in a caller-saved register and `add` to properly add four to the function's argument. Function arguments are stored automatically in register pairs. A function's first argument is stored in registers `r24` and `r25`. It second argument is stored in `r23` and `r22`, etc. Arguments of types needing more than 2 bytes use more than 2 registers. E.g., if the first argument is of type `long`, it is stored in `r22`, `r23`, `r24`, and `r25`.`  Since the argument given to `addFour()` is a `byte`, it does not need the upper 8 bits allocated to the first argument (`r25`), and `r24` contains the first argument.  Since the return value is also a `byte`, your function should store this result in register 24. Normally this would require a `mov` operation, but there should be a convenient "coincidence" here.
	
>### Random Errors

>If, when running your code, your output has seemingly random and unexplainable errors, double-check that you are properly using **caller-saved** registers, if you use them at all. Failure to save and restore them properly can cause all kinds of problems.

6. Your `addFour()` function should now work correctly. Test it a bit. What happens when you overflow a byte (like 255+4)? What happens to the status register's **carry flag** (`C`)?

>### Stacking up

>The `call` and `ret` operations (and the `push` and `pop` operations you'll use for the assignment) all operate on the **stack**. This is a *convention* for using memory, though it does blur the line between datatype and convention[^types].

>[^types]: Although you can consider any datatype to be a convention, or, at least, you can consider the *implementation* of a datatype to be a convention. E.g., IEEE floating point.

>The stack's primary purpose is keeping track of function calls: when you `call` a function, the computer puts the location of the next instruction on the stack, then jumps to the called function. When that function `ret`urns, the computer looks at this stored address in the stack and jumps to the correct point in the program to continue execution.

>The way this works is interesting. The stack is simply a segment of memory that we block out arbitrarily (if it gets bigger than this segment you get a **stack overflow**). By keeping track of the latest entry in the stack with a **stack pointer** (just like we keep track of the current instruction with the instruction pointer), we can **push** new data onto the stack and **pop** it off the stack. A push will increment the stack pointer and put this data in the new location, and a pop will read the data then decrement the pointer. Thus a `call` will push the return address onto the stack and a `ret` instruction will pop this address off.

>You can use the stack for arbitrary data storage as well, provided you clear that data off before the stack needs to be used elsewhere. The `push` and `pop` instructions let you push and pop arbitrary registers, but remember that the stack knows nothing about data that you put on it. If you don't `pop` all the data you `push`, there will be extra data on the stack. When your program tries to `ret` or read from the stack next, it will read *your* data, and not the data it was expecting. This is called **stack corruption**. Keep track of your stack.

![========]({{ "/images/line.gif" | relative_url }})

## I/O from the get go


**NOTE:** If you get to this point, consider this studio a success! You can consider yourself done. This section just delves deeper into assembly's capabilities, and is completely optional.


You'll deal with further memory manipulation (loading and storing variables, pointers, references, etc) in this module's assignment, but we're going to cover another aspect of output in assembly.

Like your desktop computer, which connects to multiple forms of output (like your monitor, speakers, wi-fi card, and rumble pads on your Xbox controller), your Arduino can also communicate with output pins---you've been working with these pins all year. The `digitalWrite()` command in Arduino is a C function, but we're going to do something similar in AVR assembly.

AVR assembly provides two ways to access I/O pins. All of them are numbered and can be accessed using their appropriate address in **I/O space**, but they are also mapped to *different* memory addresses and can be modified by editing their values in **memory space**. These pins then have two addresses: their *I/O space* address and their *memory space* address. 

The address you use depends on the commands you use to access them. If you use dedicated instructions for modifying I/O ports (like we will) you use their I/O space address. If you use memory modification instructions (like the sort we will use to modify variables and memory in the assignment), you use the memory space address.

Unfortunately, the constants given to us in `avr/io.h` are the memory space addresses. We will have to convert them with the `_SFR_IO_ADDR()` macro, which justs subtracts `0x20`.

1. Define the `turnOnLight()` function in assembly, then add some code to `simpleValues.ino` to call it.
2. AVR I/O pins are grouped in banks: a large microcontroller could hypothetically have around 20 banks of 8 ports, but ours only has 3. For each bank there are three corresponding I/O registers, each controlling a single aspect of each pin. For example, the `DDR` register controls data direction (`1` for output, `0` for input), while the `PORT` register will write output to an output pin or selectively enable the pull-up resistor for an input pin. The `PIN` register will read an input pin.

	The three banks, `B`, `C`, and `D`, correspond respectively to digital pins 13-8, analog pins 5-0 (two of `C`'s bits correspond to built-in timers), and digital pins 7-0---yup, the order is weird.
	
	We'll be using the `B` bank to control pin 13, since that requires no additional wiring. *Arduino* pin 13 corresponds to bank B's *fifth* pin.
	
	Use the [`sbi` command](https://onlinedocs.microchip.com/pr/GUID-0B644D8F-67E7-49E6-82C9-1B2B9ABE6A0D-en-US-1/index.html?GUID-CBDB6495-9738-4D9A-A306-F6DB8101DF0D) (Set Bit in I/O, which writes a `1` to a single I/O bit in a single bank) to change the data direction of Arduino pin 13 to output: `sbi _SFR_IO_ADDR(DDRB), PIN5`.
	
	`_SFR_IO_ADDR()` converts a memory address of an I/O pin to it's appropriate I/O address, so we are setting the the data direction (`DDRx`  of `DDRB`) of the fifth pin (`PIN5`) on bank B (`xxxB` of `DDRB`) to output (`1`).
3. Now set the correct bit of `PORTB` to `1` to turn on the pin. Remember to use `_SFR_IO_ADDR()`.

	What happens when you upload this program to your Arduino? The LED built into Arduino pin 13 should flicker on and off while the program starts, but should then settle into a specific state.
4. Now add a `cbi` instruction (Clear Bit of I/O register) to turn *off* the LED directly after you turn it on. Rerun the program. What happens to the LED? How long does it take for the change to occur? How does that compare to the 16MHz clock rate of the Arduino?

![========]({{ "/images/line.gif" | relative_url }})

## Check out!

1. Commit your code and get checked out by a TA.


![========]({{ "/images/line.gif" | relative_url }})

## Key Concepts

This is a mental checklist for you to see what the Studio is desgined to teach you. 

- Assembly Code
	- Language Level
		- Machine code
		- Low vs high Level Code
	- Assembly's Purpose and Uses
- AVR
	- RISC 
	- Code Syntax
	- Simple Operations
		- `ldi`
		- `push` and `pop`
		- `add`
		- `mov`
	- PrintRegs
	- Calling Functions
		- `.global`
		- `call`
		- `ret`

{% include footer.html %}
