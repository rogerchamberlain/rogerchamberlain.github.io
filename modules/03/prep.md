---
layout: default
title: Module 3 Prep
author: Doug Shook
---
{% include nav.html %}

# {{ page.title }}

In this module we will learn a more accurate timing method used in everything from button counts to video games. We apply it in a simple example case, while focussing on core concepts to Arduino programming like analog input and arrays.

![========]({{ "/images/line.gif" | relative_url }})

## Text

Delta timing is described in Chapter 6 of the course
[text]({{ "/files/cc_v0_06.pdf#page=49" | relative_url }}),
and analog inputs are described in Chapter 5 of the
[text]({{ "/files/cc_v0_06.pdf#page=39" | relative_url }}).

![========]({{ "/images/line.gif" | relative_url }})

## Delta Timing

So far our timing techniques have been rather simplistic: we simply delay when we need to. In some situations, it will be necessary to check for certain events at a specified rate, or it may be necessary for different parts of our system to communicate with each other periodically. In these scenarios, a *delta timing* technique can be used to synchronize these events and make them consistent.

| Video | Description |
|:-----:|:-----------:|
|[Delta Timing](https://wustl.box.com/s/q3bk6mh24wd72nd5p6j2gjjudx70ypdw) | In this video we introduce delta timing and show some examples. |
|[Rolling Average Filter](https://wustl.box.com/s/sutfpw2u3qbdx8h83vr6tr97x5yn562u) | Noise is a big problem when dealing with analog circuits. This video will show you a technique to smooth out the noise. |
|[Fixed Point](https://wustl.box.com/s/1l00ocihfi5okdgm6ms7c7kuitirqqwj) | In our previous discussion about information representation we talked about ints and longs. This video will introduce you to a decimal data type: fixed point. |
|[Floating Point](https://wustl.box.com/s/9uplpfls6l6lz3fy4bwt9jncmapnflbm) | Fixed point has some limitations, namely the range of values that can be represented. This video introduces an alternate way of representing decimal values: floating point. |

{% include footer.html %}
