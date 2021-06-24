---
layout: default
title: Module 5 Prep
author: Doug Shook and Roger Chamberlain and James Orr
---
{% include nav.html %}

# {{ page.title }}

In this module we gain additional practice, combining three concepts we have looked at earlier: FSMs, delta-timing, and commmunications.

![========]({{ "/images/line.gif" | relative_url }})

# Text

Delta timing is described in Chapter 6 of the course
[text]({{ "/files/cc_v0_06.pdf#page=49" | relative_url }}).

# Digital Inputs
You can ignore the references to interrupts in the first video, we are not doing that this semester.

# Delta Timing

Rather than using blocking delay timing techniques, in some situations, it is be necessary to check for certain events at a specified rate, or it may be necessary for different parts of our system to communicate with each other periodically. In these scenarios, a *delta timing* technique can be used to synchronize these events and make them consistent.


# Videos

| Video | Description |
|:-----:|:-----------:|
|[Digital Inputs](https://wustl.box.com/s/35w87ffmt2o34fsug1zwfpy8569ezj4k) | This video gives a review of analog vs. digital signals and discusses digital inputs. |
|[Delta Timing](https://wustl.box.com/s/q3bk6mh24wd72nd5p6j2gjjudx70ypdw) | In this video we review delta timing and show some examples. |
