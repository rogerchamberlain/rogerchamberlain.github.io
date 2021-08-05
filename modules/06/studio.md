---
layout: default
title: Studio 6 - Streaming Data from Arduino to Java
author: Roger Chamberlain
---
{% include nav.html %}

# {{ page.title }}

![========]({{ "/images/line.gif" | relative_url }})

## Introduction

Click [here](https://wustl.instructure.com/courses/68860/assignments/289457) to access the Canvas page with the repository for this studio.

### Objectives

The topic of today's studio is receiving data in a
Java program and familiarizing yourself with a new input sensor (a sonic echo transucer to measure distance).

By the end of this studio you should know:

- how to send and receive data types that are larger than a single byte.
- how to interact with the sonic distance sensor.

![========]({{ "/images/line.gif" | relative_url }})

## The Studio

1. All of the data we've been transmitting so far has been ASCII characters. While it is very possible to communicate entirely using ASCII characters, it is very inefficient. In other words, you *could* convert your `3` to the ASCII character for `3` (`0x33`) and convert it back upon receipt, but that just seems silly. However, that's what happens when you use `Serial.print()`, or, in general, any "print" method: the program converts whatever you want to print *from* it's internal representation *into* a printable representation, like an ASCII character, then sends that to an output stream.

	If you *actually* want to send a `3`, not `0x33`, over a stream, you use `Serial.write()`. From [the documentation for `Serial.write()`](https://www.arduino.cc/en/Serial/Write), it's clear that it can only send one byte at a time: it can only physically send numbers between `0x0` and `0xff`. Anything more than that is truncated: `301` (`0x012d`) is sent as `45` (`0x002d`).

	Make a new Arduino sketch (named `datatypetest.ino` in `CSE132-studio8/datatypetest`) that first `Serial.write()`s then `Serial.print()`s the numbers `0` through `360` in sequence. What are the results when you use your Java program to view the output? What about Serial Monitor? Use a list of ASCII characters (available on the internet or via the command `man 7 ascii` in terminal) to understand the output if it's confusing, and remember that an Arduino sketch needs to `Serial.begin(9600)` before it can use any of the serial commands.

2. The Arduino is unable to `Serial.write()` numbers larger than `255`. This is a serious limitation for those of you who enjoy counting. 

	Let's investigate how to send larger (more than one byte) data types.
An integer (an `int`) in Arduino C is a two-byte type. By bitshifting appropriately, you can send each byte of this number *sequentially*. For example, you can send the number `0x4d21` as `0x4d` and `0x21`. Alter this Arduino sketch to send the two bytes in this way, first the **most significant byte** (`0x4d` in the example), then the **least significant byte**.

	Alter `SerialComm`'s `main` routine to combine the two received bytes and store the result
in a Java integer. You will have to bitshift. Remember, Java's integers are 4 bytes, so there is plenty of space for a two byte number to fit[^negative].

	Output the received integers on the console, and compare that with what you see from `readByte`'s debug output.

3. Extend the Arduino C code to send an Arduino `unsigned long int` of 4 bytes (the return value from
`millis()` would be a good choice).  On the Java side, pull the same trick you did for the two-byte values above, just extend it to 4 bytes.
How well does that work?

[^negative]: This size difference actually has a large impact when dealing with negative numbers, which, in both systems, are stored in two's complement. Since a number's negativity is entirely dependent on its first bit, naively sending the Arduino `int` bytes to Java will not correctly send a negative number. 

	The solution is **sign extension**. Bitshifting right in Java (and on signed values in C) automatically copies the highest bit in all the extended places (`0xff00 >> 16 == 0xffff`, `0x0d00 >> 8 == 0x00d0`). This preserves the value of the number, though that exact proof is left as an excercise for the reader.

### Measuring distance

**Need to add this.**

To measure distance, we will ...

Here is an illustration of the sensor that identifies the pins:

![distance sensor]({{ "15569-Ultrasonic_Distance_Sensor_-_HC-SR04-04_Pins.jpg" | relative_url }})

The following table describes the function of each pin on the distance sensor:

|Pin	|Description |
|:--------|:--------------------------------|
|VCC	|Power (5V) |
|Trig	|Trigger Pulse Input: Sends bursts of ultrasound at 40 kHz. |
|Echo	|Echo Pulse Output: Receives echo signal. Range is calculated by the proportion of trigger signal sent and echo signal received. |
|GND	|Ground (0V) |

![========]({{ "/images/line.gif" | relative_url }})

## Finish up

1. Commit your code and verify in your web browser that it is all there.
2. Get checked out by a TA.

Changes to repo structure:

- `communication/`
	- `SerialComm.java`
- `datatypetest/`
	- `datatypetest.ino`


![========]({{ "/images/line.gif" | relative_url }})

## Key Concepts

- Serial communication
	- Sending bytes from Arduino to Java (`Serial.write`)
- Java Input
	- Reading serial bytes
- `Serial.print()` vs `Serial.write()`
	- conversion and communication efficiency 
- Multi-byte communication
	- byte significance i.e. most significant byte
	- sequencing 

{% include footer.html %}
