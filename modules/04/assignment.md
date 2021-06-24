---
layout: default
title: "Assignment 4 - Dot Dot Dot, Dash Dash Dash, Dot Dot Dot"
author: Julia Vogl, Bill Siever, Jeremy Goldstein, & Evan Simkowitz
---
{% include nav.html %}

# {{ page.title }}

Click [here]({{ "/tbd.html" | relative_url }}) to access the Canvas page with the repository for this assignment.

![========]({{ "/images/line.gif" | relative_url }})

## The idea


In this assignment you'll use the combined power of Java and Arduino's
communications.  Java will send user input data to the Arduino, and the 
Arduino will process those bytes and display their contents in [**Morse
code**](https://en.wikipedia.org/wiki/Morse_code).  Morse code is an example
of a [substitution cipher](https://en.wikipedia.org/wiki/Substitution_cipher) 
(like the hex to binary substitution in the studio)
because individual letters are replaced by a specific, fixed Morse code. 
Finally, the Arduino will send the Morse code back to Java to be displayed
in the console.

By the end of this assignment you should have a better understanding of how
data streams work and how they can be used.  In addition, you will practice:
iterating through arrays, working
with ASCII characters, and transmitting data from and receiving data in
 both Arduino and Java.

![========]({{ "/images/line.gif" | relative_url }})

## The background

### The Java side of the communication

We will be using the Java `SerialComm` class, which you created during the
last studio.  If you don't yet have a functionally complete `SerialComm`
class (that supports `debug` of both  outgoing and incoming bytes), you will
want to finish that first.

### The Arduino

#### Serial stream

The primary functions for interacting with Arduino's incoming `Serial` data
are [`Serial.available()`](https://www.arduino.cc/en/Serial/Available) and
[`Serial.read()`](https://www.arduino.cc/en/Serial/Read).  The `Serial`
object operations are also built on top of
[`Stream`](https://www.arduino.cc/en/Reference/Stream) objects.

`Serial.available()` returns the number of bytes currently available to the
`Serial.read()` function.  `Serial.read()` reads the next byte from the
stream.  

#### Additional Functions

You will also use `morseEncode(char symbol)`, which has already been written
for you.  `morseEncode()` will return a `String` that contains data
corresponding to the Morse code for a given symbol.  Since Morse code
doesn't have codes for lowercase letters, you will author a `toUpper()`
function that will convert lowercase characters to uppercase.  This will
enable the `morseEncode()` function (which already calls `toUpper()`, meaning
you don’t have to edit `morseEncode()`) to translate the alphanumeric
characters inputted to morse code.  `toUpper()` should have the following
parameters:


~~~ c
// Argument: Any character
// Return Value: Either:
//    1) If the character is a letter, 
//       the upper case equivalent.  
//    2) If the character is not a letter, 
//       the original value.
char toUpper(char c) {
    ...
}
~~~

The easiest way to complete `toUpper()` is to use the properties of ASCII values.  If you look closely at the [ASCII Table](http://www.ascii-code.com/) you'll notice that a lower case letter can be converted to an upper case letter by changing just one bit, so you may want to review bitwise operations. Since the letters are numeric and in lexicographic order they can also be compared via the normal relational operators, like:

~~~ c
char c;
...
// Does this character come "at or after" 
// the character for 0? (The character 1 would 
/  come after 0, etc.)
if(c>='0') { 
~~~

Using these properties of ASCII (being able to do comparisons, lexicographic ordering, and being able to manipulate bits) you can complete `toUpper()` with just a few lines of code. 

#### Including files

There are three files in the `MorseCoder/` directory:

- `MorseCoder.ino`: This contains a nearly empty sketch.  You will complete all your Arduino work here.
- `MorseCodes.h`: This is called a <u>h</u>eader file (hence the ".h" extension) and contains things that need to read at the top (or head) of a source file to define features used later in the file.  The `MorseCoder.ino` Arduino file "includes" it to have access to the function defined in `MorseCodes.cpp`.  A header file and corresponding `#include` are used somewhat like the `import` statement in Java. **Read through the comments which describe the function(s) you may need to use for this assignment.**
- `MorseCodes.cpp`: This contains a Morse code table and code that uses it. You do not need to modify it.  **Although you don't need to understand the exact details of how `morseEncode()` works to be able to use it, take a few minutes and read through it. You should notice that it uses the properties of ASCII in a few ways.**

Header files are an important part of code organization in C. They're usually paired with a **source file** (Often with a ".c", ".cc", or ".cpp" extension). This separation helps for several reasons: it speeds up compile time, keeps code organized, and it can prevent errors due to order of declarations. Most importantly, the header is usually used to specify a public **interface** to functions whereas the corresponding source file provides private **declarations**.  Often when you just need to use a function and don't care about how it works you can simply refer to the header file.  

Often a library of related functionality is contained in one source file (.cpp file) that may be used in many different projects. The MorseCodes files are a library that can be used to encode and decode Morse code. 

![========]({{ "/images/line.gif" | relative_url }})

## The Arduino Assignment

### Getting the Arduino to recognize input from the Serial Monitor
1. Start by making the sketch in `MorseCoder.ino` read in characters from the Serial Monitor and echo them back, as in the studio.

2. Complete the `toUpper()` function and test it. Use it to ensure all the echoed characters are in upper case.  For example, if the following is entered:
	<pre>    Don't shout at me! </pre>
	It would be echoed back as:
	<pre>    DON'T SHOUT AT ME!</pre>

3. Use the `morseEncode()` function to echo back the Morse code of the characters instead of the characters themselves.  `morseEncode()` will use your `toUpper()` function, so there's no need for you to call `toUpper()` yourself.  Read the comments in `MorseCodes.h` for details of how you need to call `morseEncode()`.  You will
have to process each character of the Morse code separately.  For example,
the `morseEncode()` function will return a
[String](https://www.arduino.cc/en/Reference/StringObject).  

4. Use the Serial Port to send the newly-translated Morse code from your Arduino back to your Java program running on your PC. The entire received message should be on one line, so make sure to use `Serial.print()` rather than `Serial.println()`. Please include one space `' '` between each letter of a word. So, "HI" would be sent as:
    <pre>
    .... ..
    H    I
    </pre> 
    Additionally, use three consecutive spaces `'    '` to indicate the end of a word. "HI MOM" would then be:
    <pre>
    .... ..   -- --- --
    H    I    M  O   M
    </pre> 
5. The reason your entire message should be one line is that we will be using the **newline** character `'\n'` to indicate the end of a transmitted message. It acts as a way for both Java and the Arduino to know that the current message is over. It acts as a very simple example of a communication **protocol** which we will discuss in more detail in the coming weeks. For now, just know that it is important that both programs know when a transmission is over, so whenever your Arduino program receives a newline character `'\n'`, make sure to transmit it back to Java. This can be accomplished by calling `Serial.println()` with nothing in it or `Serial.print("\n")`.

![========]({{ "/images/line.gif" | relative_url }})

## The PC Assignment

Your final task is to author a Java program that performs very close to the
same function as the Serial Monitor.

1. Import your `SerialComm.java` into the `communication` directory, replacing the stub that is currently there.  It should have the following abilities:
- Be sure that it has its own main() method.
- It will need to prompt the user for a line to encode.
- It will read an entire line at a time from the console.
- It will send each character to the Arduino via the serial port.
- It will need to send the newline character `'\n'` after it has finished sending all characters from the user message to indicate that it has finished transmitting the message.
- It will then receive data coming back from the Arduino and should print it to the console.
- It should continue receiving data back from the Arduino until it has finished receiving the encoded version of the message it just sent. It will know that it has finished receiving the message once it receives the `'\n'` character. 
- Once it has received the `'\n'` character indicating that it is done receiving the encoded message, it should prompt the user for another message to encode.
- The `debug` in `SerialComm` should be set to `true` so that you can see the bytes as they go into and out of the Java program (to/from the Arduino).

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

	- Your sketch is able to read input from Java or the Serial Monitor
	- `toUpper()` works as it should
	- `Serial.read()` should only be invoked when data is `available()`
	- Your sketch correctly iterates over the string returned by `moreseEncode()`
	- Your sketh sends each `'.'` `'-'` and `' '` to your Java program over the Serial port 
	- Your sketch sends one space between every letter and three spaces between every word
	- Your Java program reads lines of characters and sends them through the serial port
	- Your Java program uses `debug` to display the outgoing and incoming data
	- All of your files are committed
	<br />


1. Check out with a TA.

![========]({{ "/images/line.gif" | relative_url }})

## The rubric

Note: the formal rubric is on Canvas, this is just the highlights.

- Did the demoed assignment work?
    - `toUpper()` meets requirements
    - Sketch properly receives data from Java and passes it to `morseEncode()`
    - Proper timing between dots/dashes, between symbols, and between words
    - Arduino code is well-organized, variables are appropriately named, and comments are used to aid understanding
    - Java code reads from the console and sends data to the Arduino
    - The `SerialComm` class has an appropriate `readByte()` method
    - The `SerialComm` class has an appropriate `writeByte()` method
    - The `SerialComm` class has an appropriate `available()` method

