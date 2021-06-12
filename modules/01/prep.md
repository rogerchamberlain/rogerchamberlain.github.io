---
layout: default
title: Module 1 Prep
author: Doug Shook
---
{% include nav.html %}

# {{ page.title }}

Today we will briefly review the class resources and get you set up with the necessary tools that you need to complete the course, including a GitHub account, git repository, and both the Eclipse and Arduino IDEs.

This module is about exploring the many different ways that data can be represented on a computer.

![========]({{ "/images/line.gif" | relative_url }})

# Text

Read Chapter 8 in the course
[text]({{ "/files/cc_v0_06.pdf#page=75" | relative_url }}).

![========]({{ "/images/line.gif" | relative_url }})

# Information Representation Videos

In order to better understand what our computers are doing, it is important that we understand how computers store information. Watch the following videos to find out more about this very important topic.

| Video | Description |
|:-----:|:-----------:|
|[What is binary?](https://wustl.box.com/s/o7j88s22mtwexs86ozlw3z8bqtmfar4r) | Before we can understand how the computer represents data, it is important to understand the binary system. |
|[Binary Number System](https://wustl.box.com/s/ttozc7bzghgyj044yxxzlc7m8rir172x) | Now that we know a bit about binary, we can use it to start representing numbers |
|[Negative Numbers](https://wustl.box.com/s/lmjw1er18gmpgoe3elzwf8b6075jl3wr) | Representing negative numbers is not as straight forward as you might think. Watch this video to find out why. |
|[Hexadecimal](https://wustl.box.com/s/e1yq97uujl9ungka1yrz4yeuzs63qzil) | Computer scientists prefer hexadecimal to binary - and for good reason! |
|[Text Representation](https://wustl.box.com/s/25nckr4oou8w3dy9456ynnr4o8vei21m) | See how text is represented on the Arduino, as well as in Java <br>**NOTE: This video indicates that 'A' is represented as 0x40 and 'a' is represented as 0x60 in ASCII. They are actually 0x41 and 0x61, respectively.**|


## Guide to Information Representation

[This guide](https://classes.cec.wustl.edu/~SEAS-SVC-CSE132/guides/intro-to-information.html) on information representation covers some of the same material covered in the videos, but it goes more in depth on a few topics.

![========]({{ "/images/line.gif" | relative_url }})

# Finite State Machines

| Video | Description |
|:-----:|:-----------:|
|[Intro. to Finite State Machines for Modelling](https://wustl.box.com/s/yv7x0g9d8svdx2rl0669w7032hxua836) | This video introduces Finite State Machines as a way of modelling behavior |
|[Reading Input](https://wustl.box.com/s/5pxqe15sbbioawkkwvjxnwp4h54zmt8q) | In this video, you will see how to read input from the keyboard and send it to the arduino |
|[FSMs on the Arduino](https://wustl.box.com/s/5fytd1n76nfbqxmyjufombmeih1e2mjt) | This video shows you how to take an FSM and turn it into code that can be run on an Arduino |

## Guide to Finite State Machines

[This guide]({{ "/intro-to-FSMs.html" | relative_url }}) on Finite State Machines is also quite useful.

{% include footer.html %}
