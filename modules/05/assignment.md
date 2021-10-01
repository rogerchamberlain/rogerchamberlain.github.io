---
layout: default
title: "Assignment 5 - More Morse Mayhem:  -&#8202;- -&#8202;-&#8202;- .-. ... . "
author: Julia Vogl, Bill Siever, Jeremy Goldstein, Evan Simkowitz, and James Orr
---
{% include nav.html %}

# {{ page.title }}

Click [here](https://wustl.instructure.com/courses/68860/assignments/289461) to access the Canvas page with the repository for this assignment.

![========]({{ "/images/line.gif" | relative_url }})

## The idea

In this module you will incorporate delta timing into the Morse code assignment 
from Module 4. You will add an LED to your Arduino which will turn on for 
short (and slightly longer) amounts of time corresponding to dots and 
dashes in the Morse encoding of messages sent from Java.

By the end of this assignment you should have a better understanding of how
delta timing works and how it can be used.  In addition, you will practice:
iterating through arrays, working
with ASCII characters, and considering the logic required to properly
implement non-blocking (i.e., delta) timing.

![========]({{ "/images/line.gif" | relative_url }})

## The Arduino Assignment

1. Begin by importing your Java code and MorseCoder.ino from 
Assignment 4 into their appropriate spots in Assignment 5. All 
functionality from Assignment 4 should still be in this assignment. 
Java will still send a user-input message to Arduino to be encoded.
The Arduino will then still encode the message and send it back to Java.

2. Update your C code to have the Arduino blink an LED for the given Morse code.  You will
have to process each character of the Morse code separately.  For example,
the `morseEncode()` function will return a 
[String](https://www.arduino.cc/en/Reference/StringObject).  You will have
to loop through the returned string, retrieve each character, and use the
character to turn on an LED for the appropriate amount of time.  You can
access an individual letter by using array-like notation.  For example, this
would print each letter in a string one-at-a-time:

~~~ java
String words = "Hello World!";
for(int i=0;i<words.length();i++) {
    Serial.print(words[i]);
}
~~~

3. The timing of Morse code should be as follows:
- you chose the duration of 1 unit;  500 ms is a good choice
- a dot, `.`, turns the LED on for 1 unit
- a dash, `-`, turns the LED on for 3 units
- a space ensures the LED is off for a total of 7 units
- between the dots and dashes of a single symbol's code, turn the LED off
for 1 unit.  For example, the code for `A` is `.-`.  The LED needs to be off
for 1 unit between when it is on for the `.` and then on again for the `-`.
- between symbols the LED should be off for 3 units.  For example, if coding
two letters, like `AB` you would have to blink the code for `A`, which is
`._`, and the code for `B`, which is `-...`.  The LED should be off for
three units of time between the final symbol of `A` and the start of `B` to
indicate the end of a letter.
- when not displaying a dot or a dash the LED should be turned off.

Here are two visualizations showing the full encoding of `OH HELLO`:

<pre>

Time:       1234567890123456789012345678901234567890123456789012345678901234567890123
Message:    O----------   H----       H----   E   L--------   L--------   O----------
Signal:     HHHLHHHLHHHLLLHLHLHLLLLLLLHLHLHLLLHLLLHLHHHLHLHLLLHLHHHLHLHLLLHHHLHHHLHHH
             ^     ^      ^       ^             ^
            dash   |     dot      |             |
             symbol space     word space  letter space
</pre>


| Character | Complete Morse Code | Each Symbol | LED On Time | LED Off Time|
|-----------|---------------------|-------------|-------------|-------------|
|O | `---` | `-` (1st `-`) | 3 | 1|
|" |  | `-` (2nd `-`) | 3 | 1|
|" |  | `-` (3rd `-`) | 3 | 1|
|(between letter) | | | | 2 (total of 3 including above)|
| H | `....` | `.` (1st)| 1 | 1 |
| " |   | `.` (2nd)| 1 | 1 |
| " |   | `.` (3rd)| 1 | 1 |
|(between letter) | | | | 2 (total of 3 including above)|
| ` ` (space) |  |  |  | 4 (total of 7 including above)|
| H | `...` | `.` (1st)| 1 | 1 |
| " |   | `.` (2nd)| 1 | 1 |
| " |   | `.` (3rd)| 1 | 1 |
|(between letter) | | | | 2 (total of 3 including above)  |
| E |`.`| `.` | 1 | 1 |
|(between letter) | | | | 2 (total of 3 including above)  |
| L | `.-..` | `.` | 1 | 1 |
| "  |  | `-` | 3 | 1 |
| "  |  | `.` | 1 | 1 |
| "  |  | `.` | 1 | 1 |
|(between letter) | | | | 2 (total of 3 including above)  |
| L | `.-..` | `.` | 1 | 1 |
| "  |  | `-` | 3 | 1 |
| "  |  | `.` | 1 | 1 |
| "  |  | `.` | 1 | 1 |
|(between letter) | | | | 2 (total of 3 including above)  |
|O | `---` | `-` (1st `-`) | 3 | 1|
|" |  | `-` (2nd `-`) | 3 | 1|
|" |  | `-` (3rd `-`) | 3 | 1|


Although you may initially use `delay()` timing to get going with the assignment, when you're done you should only be using delta timing (i.e., **NO `delay()` allowed**).


![========]({{ "/images/line.gif" | relative_url }})

## Demo

The following video shows a brief demo of the expected timing:

<iframe src="https://app.box.com/embed/preview/05q80asuta3ycqnmdgkfn887efo5tfc6?theme=dark" width="640" height="360" frameborder="0" allowfullscreen webkitallowfullscreen msallowfullscreen></iframe>

- The pane on the right shows the text being entered on the console by the user (but not the dots and dashes printed back out).
- The horizontal band shows the Morse code being typed out.  The "\|" indicates the time.  The red one is the current time.
- Immediately above the \| is the current symbol being sent. A dot takes a single time period and a dash takes three time periods (so a dash is over three \|s). Note the time between dots/dashes of a single letter (one \|), between different letters of the same word (a total of three \|s), and between different words (a total of seven \|s).
- Above the dot/dash indicators is the specific letter being sent.
- The light is in the lower right corner.  Note that for a dash it is lit for three \|s and for a dot it is lit for a single \|.

![========]({{ "/images/line.gif" | relative_url }})

## The check-in

Verify that you have completed the following files:

<!-- <section class="tree"> -->
        - `MorseCoder/`
                - `MorseCoder.ino`
                - `MorseCodes.cpp`
                - `MorseCodes.h`
        - `communication/`
                - `SerialComm.java`

<!-- </section> -->

1. Commit all your code. **Do not ask to be checked out by a TA until
after you have made certain that your work is committed**. Failing to
do this may result in you losing points on your assignment.

1. Follow the checklist below to see if you have everything done before demo your assignment to a TA.
	- Your sketch is able to read input from Java
	- Your sketch correctly iterates over the string returned by `moreseEncode()`
	- Your sketh sends each `'.'` `'-'` and `' '` to your Java program over the Serial port
	- Your LED lights up for one unit of time for a dot
	- Your LED lights up for three units of time for a dash
	- Your LED is off for one unit of time between dots/dashes
	- Your LED is off for three units of time between symbols
	- Your LED is off for seven units of time between words
	- Your sketch uses delta timing
	- Your Java program reads lines of characters and sends them through the serial port
	- Your Java program uses `debug` to display the outgoing and incoming data
	- All of your files are committed


1. Check out with a TA.

{% include footer.html %}
