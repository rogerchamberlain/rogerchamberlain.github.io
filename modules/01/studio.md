---
layout: default
title: Studio 1 - Programming and Bit Manipulation
author: Claire Heuckeroth, Ben Stolovitz, and Sean Schaffer
---
{% include nav.html %}

# {{ page.title }}

Click [here]({{ "/tbd.html" | relative_url }}) to access the Canvas page with the repository for this studio.  

![========]({{ "/images/line.gif" | relative_url }})

## Introduction

Now that you know the basics of the Arduino IDE and its Programming Language, it's time to go a bit more in-depth about how it and computers in general work.

### The C Programming Language

If you took CSE131 or AP Computer Science, you should already be familiar with Java. We will still use a lot of Java in this class, but we're also switching the emphasis to another language called **C**. 

We do this for two reasons:

- You program Arduinos in C
- C is much closer to the hardware

Unlike Java, which runs on a virtual machine *on top* of your existing operating system, C compiles to pure machine language: the direct instruction set that your computer actually runs. In other words, if you'd like to see the exact instructions your computer uses to draw a pixel onscreen in a video game or download your email, you look at a compiled C program.

Lucky for us, *uncompiled* C (like, C code) looks a lot like Java. So much so that you should be able to read C with no problems.

Compiled code is a different story. We'll deal with the human-readable "rendition" of machine-language, Assembly, later in the semester, but for now we'll deal with the more immediate problem of how programs interact with the computer

![========]({{ "/images/line.gif" | relative_url }})

## Studio Proper

### Information representation

But before we can ever talk about I/O (input/output) and programs and compiling things, you need to understand how the computer stores data. In other words, before you can understand how computers manipulate data, you must know how they represent it.

To that end, we've prepared some pen-and-paper exercises to get you thinking about data like a computer does.

If you are having trouble with the concepts behind any of these questions, try reading Chapter 8 in the [course textbook](http://www.cse.wustl.edu/~roger/cse132/cc_v0_06.pdf) or look through the [Guide to Information Representation](/~cse132/guides/intro-to-information.html).


![========]({{ "/images/line.gif" | relative_url }})

## Key Concepts

<aside class="sidenote">
This is a checklist for you to see what the Studio is designed to teach you.
</aside>

- Binary, Hexadecimal, and other Bases 
- Bitwise Operations
	- LEFTSHIFT
	- RIGHTSHIFT
	- INVERSE
	- Overflow 
- FSMs
	- Cases
	- Enums
		- What happens at Runtime
	- `switch`
		- `break`
- Repository 
	- Eclipse 
		- `Team > commit` 


(% include footer.html %}
