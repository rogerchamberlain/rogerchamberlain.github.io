---
layout: default
title: Week 4 Prep
author: Doug Shook
---

{% include nav.html %}

# {{ page.title }}


# Java - Arduino Communication

The focus of this module is communication between the Arduino and Java, which will require that we think carefully about how the data are represented.  We will send data from the PC to the Arduino.  With this functionality, we'll be able to control our circuits from the PC side - a very useful tool. 

We will be using the Java Simple Serial Connector ([jSSC](https://github.com/scream3r/java-simple-serial-connector)) library for serial communications.  A local copy of the JavaDoc for the library can be found [here](https://classes.cec.wustl.edu/~SEAS-SVC-CSE132/jssc/javadoc/).

![========]({{ "/images/line.gif" | relative_url }})

# Text

Read Chapter 12 in the course
[text]({{ "/files/cc_v0_06.pdf#page=149" | relative_url }}).

*Note*, this chapter is only in draft form, meaning there is lots of
information that is helpful to you in completing this week's activities,
but don't consider it complete by any means.

![========]({{ "/images/line.gif" | relative_url }})

# Some Review

| Video | Description |
|:-----:|:-----------:|
|[Text Representation](https://wustl.box.com/s/25nckr4oou8w3dy9456ynnr4o8vei21m) | You saw this video earlier in the semester, but we'll be using more text in this module, so it would be a good idea to review these concepts. <br>**NOTE: This video indicates that 'A' is represented as 0x40 and 'a' is represented as 0x60 in ASCII. They are actually 0x41 and 0x61, respectively.**|
|[Hexadecimal](https://wustl.box.com/s/e1yq97uujl9ungka1yrz4yeuzs63qzil) | We first introduced you to hexadecimal in week 1, but we'll be relying on it more and more in the next few modules, so be sure you understand this concept. |

![========]({{ "/images/line.gif" | relative_url }})

# Input Streams

The following videos are very helpful in understanding how to read user input from the console in Java.

| Video | Description |
|:-----:|:-----------:|
| [Java Input Streams](https://wustl.box.com/s/x5gycrv23j4x2thswtygqa5qyrbazwqs) | In this video we'll discuss how to use input streams to grab information from the user. |
| [Validating Input](https://wustl.box.com/s/o3r41moro28jy0q6wvfejfkrb48vhsde) | The data you receive from your users may not be what you were expecting. This video will give some tips about what to do when that happens. |

## Guide to SerialComm

[This guide]({{ "/SerialCommGuide.html" | relative_url }}) gives some help on how to use the serial communication libraries in Java.

{% include footer.html %}
