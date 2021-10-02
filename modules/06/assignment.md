---
layout: default
title: "Assignment 6 - Protocols, or What did you say?"
author: Roger Chamberlain, Ben Stolovitz, and Evan Simkowitz
---
{% include nav.html %}

# {{ page.title }}

Click [here](https://wustl.instructure.com/courses/68860/assignments/289462) to access the Canvas page with the repository for this assignment.
You will also need to copy your completed `SerialComm.java` file from studio into the `communication` package in order to get this assignment to work.

![========]({{ "/images/line.gif" | relative_url }})

## The idea

When two computers send data from one to another, it is the job of a **protocol** to specify how bytes transmitted by the sender are to be interpreted by the receiver. The idea is simple: if we specify *very, very* concretely how we expect our data, it becomes easy to communicate. Despite the vast differences between our Arduino and Java programs, so long as we create and follow a well-defined protocol, the differences barely matter.

In this assignment, you will build a pair of programs---one Arduino, one Java---that harvest real-world data, transmit it, and then receive it on another processor. Though this assignment focuses mostly on the data transmission, this centralized design is very useful in many circumstances. WIFI networks, at least in the small scale, rely on this, where many devices connect to a centralized hub.

Using a provided protocol, you will design an Arduino program to send and a sister Java program to receive real-world measurements.

![========]({{ "/images/line.gif" | relative_url }})

## The background

### Protocols

No matter what we want to communicate, someone, somewhere, somehow has to decide upon a way of communicating it. Someone created a **protocol**. This is clear when looking at something like language: it has rules, syntax, grammar, symbols, definitions, and a slew of implicit and explicit rules. Though not as murkily defined as language, all forms of computer communications have the same structure.

Because we could never express the definition of one accurately enough ourselves,
from [Wikipedia](https://en.wikipedia.org/wiki/Communications_protocol),
**protocols** are "the rules that define the syntax, semantics and synchronization of communication and possible error recovery methods." In a word: **rules**.

If we define every bit of our protocol precisely enough---from the size of `int`s to the encoding used for `char`s to the ordering and grouping of data---we can easily (and independently) write software to both produce and consume that data.

We detail this module's protocol below.

### Our protocol

We chose to design a protocol that centers around the sending of individual **messages**. Each message we send will have a 1 byte **header** and a variable length **payload**. The header is basically used for book-keeping and the payload is the actual information being sent.

In our case, the payload will send a **key-value pair**: a **key** indicating the meaning of our value (is it a distance measure or a string?, etc.) and a **value** for that key. A value might be a
raw reading (e.g., time for a distance measurement, a processed distance value,
a potentiometer reading, a timestamp, an error message, etc.

*Note*: In this assignment, we are using the terminology **key-value pair**; however, another nomenclature that is also very common is to call them **tag-value pair**.  Don't be surprized if you see these terms used for essentially the same purpose.

#### Header


As stated above, the message header helps the
communication protocol do some book-keeping. In our case, the header is
designed to help us sync up the beginning of a message both
at the sender and the receiver.

Consider what happens if the sender is delivering a series
of two-byte integers, high byte first then low byte second,
and the receiver somehow misses one byte.
If this happens, all the integers that follow will be
interpreted incorrectly by the receiver.

We will significantly decrease (but not completely eliminate)
the possibility of this happening by starting each message
with a **magic number**, a constant value
that is known to both sender and receiver as always present
at the start of a message.

When the sender wishes to transmit a message, it must start
the message with the magic number.
**For our protocol, the magic number is `0x21`, or an ASCII `'!'`.**

When the receiver is ready to interpret an incoming stream
of bytes, it knows that the first byte of a valid message
will be the magic number.  When it reads a byte from the
stream, if the byte is not the magic number, it can discard
that byte (and all subsequent bytes that are not the magic
number) until it reads a byte that *is* the magic number.
In this way, it is reasonably assured to be aligned with
the beginning of a message.

<!-- <aside class="sidenote"> -->
>##### Magic numbers

>Magic numbers can be found not only in **communication protocols**, but in places all over computer science. One such place is in files that are expected to be of a particular type.

>You can understand the arbitrary nature of file extensions by renaming a `.jpeg` file `.txt`---it will be garbage, but your computer doesn't have any "checker" when using files. What could happen if you accidentally ran a `.txt` file or a JPEG as a compiled program? What if some of the bytes in the file translated to machine instructions to destroy a file?

>To prevent that, ELF files (compiled Linux programs) and their Windows equivalents, `.exe`s, both have magic numbers at the beginning. ELF files begin with 4 bytes of magic numbers: `0x7f 0x45 0x4c 0x46` (or `DEL E L F` in ASCII---`DEL` is a special character). Windows executables: `0x4d 0x5a` (`M Z`, the initials of the designer).

>If you consider the similarity between **file formats** and protocols, it's not surprising that [many, many recognizable filetypes](https://en.wikipedia.org/wiki/Magic_number_(programming)) have magic numbers, too.
<!-- </aside> -->

#### Payload

The payload in our protocol is comprised of a **key-value pair**,
where the **key** defines both the meaning and the format of the subsequent **value**.

The key is a fixed-length field: one byte.
The value is a variable-length field, the number of bytes depending
upon a number of factors.

For this assignment, we define the following keys: **NOTE: need to add sonic sensor**

- `0x30`	info string in UTF-8 format, maximum of 100 characters long
- `0x31`	error string in UTF-8 format, maximum of 100 characters long
- `0x32`	timestamp, 4-byte integer, milliseconds since reset
- `0x33`	potentiometer reading, 2-byte integer A/D counts
- `0x34`	raw (unconverted) ultrasonic sensor reading (i.e., time), 4-byte unsigned integer &mu;s.

As the semester progresses, we might add to the above list, but we
won't change the meaning of these already-defined keys.

For string values in UTF-8 format, the first two bytes are a two-byte
integer that gives the length (number of characters) of the string.
This is then followed by the ASCII characters of the string.
In our protocol, characters are restricted to the range `0x01` to `0x7f`
(i.e., `NULL`---`0x00`---is not allowed, nor are characters in the extended
range of the UTF-8 standard[^ascii]).
In Java, you will need to create a `String` from the input ASCII characters.
Put the characters in a byte array, e.g., `byte[] chars`, and use
the following form of the `String`
constructor: `String(chars, StandardCharsets.UTF_8)`.

For more information about how string types are represented in Arduino C, take a look at this video:

<iframe src="https://app.box.com/embed/preview/1ecs8vqr8xxcx8ak1tmbay9gfh3f50na?theme=dark" width="640" height="360" frameborder="0" allowfullscreen webkitallowfullscreen msallowfullscreen></iframe>

[^ascii]: In reality, a UTF-8 string can consist of *a lot* more than ASCII characters, like emoji, Greek letters, [Klingon](http://www.evertype.com/standards/csur/klingon.html), and more. However, the designers of Unicode were careful to ensure that the first 128 characters exactly matched those of ASCII, so ASCII strings are identical to their Unicode counterparts.

For multi-byte values that represent a single number (e.g., 2-byte integers,
4-byte integers), the order of the bytes is most significant
byte first and least significant byte last.
In Java, make sure you have finished Studio 8. 

## The assignment

Locate and open the `MsgReceiver.java` file in your repository. This is where most of your work will go on the Java side. Import your SerialComm.java into the same directory and copy out its `public static void main(String[] args)` method, as that is now in `MsgReceiver.java`.

>### Sidebar for all you hackers

>We want to provide two ways of completing this assignment: the guided way in the main writeup, and a more "free-form" way. In this sidebar we just detail what we expect the protocol to do and what your programs should output, but in the main writeup we guide you through the process in more detail. Choose whatever you like.

>1. Wire up your potentiometer and ultrasonic sensor to your Arduino. The wiring is identical to before.
>2. In a 1 Hz delta-time loop (in `sender.ino`), send---using our protocol:

>- the timestamp of the loop (do not call `millis()` a second time, store the first call as an `unsigned long` and use that),
>- the potentiometer reading,
>- the raw time value from the ultrasonic sensor

>Finally, if the potentiometer reading is over a certain value of your choice, send an error string (key: `0x31`) `High alarm` at the very end of the delta-time loop.
>3. Edit the `run()` method of `MsgReceiver` in Java to read these messages and print them out as it receives them. Make sure to output both the type of message (e.g., info string, error string, potentiometer value, raw ultrasonic sensor value) and the value (the string value or integer value). 

>4. On the Java side, convert the ultrasonic readings to an actual distance (in cm, just like you did in studio, just in Java this time), then print out the computed distance.

>	Have it print a visually distinct error message if an unknown key is used.

>That's the entire lab. If it seems simple, make sure to use a finite state machine on the Java side, as we will probably be extending this later in the semester.

### And now we return to the main writeup

1. Author enough of an Arduino sketch `sender.ino` to send info messages (i.e., send the magic number `!`, the key `0x30`, and then a hard-coded UTF-8 encoded string, prefixed with its length). String s need to be hardcode for a couple of reasons. First, strings cannot be dynamically sent via the Serial Monitor from the Arduino IDE while the Java side. Second, the messages will correspond with specific actions (e.g., indicating which button has been pressed), so there will be a finite amount.


	Send a few info messages, making sure you're confident doing so. Can you write a function called `sendInfo()`? It can accept a string literal as a parameter by taking a `char*` as an argument. Note: if you are unfamiliar with strings in C, review the relevant part of Chapter 8 in the [text](http://www.ccrc.wustl.edu/~roger/cse132/cc_v0_06.pdf#page=97), and be aware that a `char*` argument is the same as an array of characters, `char[]`, with a variable length. I.e., the sketch needs to look at individual entries in the array to find the `NULL` character (`'\0'`) used as the end marker.


	If the Serial Monitor isn't helpful enough (it won't be, because you are no longer just sending ASCII characters), alter the Java `MsgReceiver`'s `run()` method to continuously read bytes from the input port. Ensure `debug` is `true` in your `SerialComm` so input bytes are printed in hex form.

2. Author enough of the Java program (`MsgReceiver`'s `run()` method) that you can receive an info message and reliably observe the info string on the console window.
	Send a few more info messages.
3. Set up a delta-time loop, running at about 1 Hz, that sends a message containing the `millis()` value used to control the loop (i.e., save the return value from `millis()` in a variable so that you can use it both for the `if` test
in the delta-time code and send it to the PC. Make sure to use an `unsigned long`). This is the `0x32` message.
4. Alter your Java code to parse the message and print it out nicely every time it's received. Include the type of message (e.g., info string, error string, potentiometer value, raw ultrasonic value) as well as the value
(the string value or integer value).

	Structure your receiver program as a **finite state machine**. We recommend reading our
[Introduction to FSMs]({{ "/intro-to-FSMs.html" | relative_url }}) guide, as it
will make your program *significantly* easier to reason about.

	Messages that do not conform to the protocol should generate an
error printed to the console that is visually distinct from the
protocol-supported error message. Indent it and use a different format. I like `!!!!! Error` but that may be a little dire for your tastes. &nbsp;  
5. Attach the potentiometer and ultrasonic sensors to your Arduino.
The wiring is the same as in the past.
6. Update your delta time loop to also read the value of the potentiometer, sending it after the `millis()` message. This is the `0x33` message.
7. Likewise update your Java code to read this number and print it out nicely.
8. Continue doing this for the other keys: send and a receive a raw time reading from the ultrasonic sensor. This is the `0x34` message.
9. When the potentiometer reading is above a threshold
value (feel free to use whatever value you chose in that assignment, or
pick a new value) send a message with the error string "High alarm".
This key = `0x31` message should come at the end of all output.
10. On the Java side, when you receive a distance value from the ultrasonic sensor you should convert it to the appropriate engineering units (&mu;s), then display the computed distance values. **Note**, your conversion code is now in Java, not in C. 

### Guidelines

1. Try to develop an intuition for the raw numbers behind ASCII. For example, what ASCII characters do our protocol's **keys** correspond to?

	This will be helpful if your Java program is behaving strangely and you need to look at Arduino Serial Monitor. You should be able to get a sense of the messages as they come through: you'll see the `!` as the magic number, then the ASCII character of the **key**, then "gibberish." Understanding which numbers go to which ASCII characters will help you make sense of that gibberish.
2. Make sure your program generates errors if it receives input it didn't expect. While silently failing is sometimes good (see [Defensive Programming](https://en.wikipedia.org/wiki/Defensive_programming)), in your case it will make it harder to understand where your program is going wrong. 

	Be loud! Print error messages! User error is the best type of error because it's not your fault.
3. Read your analog values, parse them, filter your data, and *then* print all your messages. It will make it easier for us to grade your assignment (and also easier for you to debug).
4. Send info strings *a lot*. Since they are the first thing you implement, they are also the first thing you can be confident *works*, so if something is broken, falling back to them may be just what you need. 
	
	Use them only for debugging and small program notes, though. They're "info strings" for a reason, not info paragraphs!

![========]({{ "/images/line.gif" | relative_url }})

## The check-in

1. Commit all your code (make sure to add any new files to your repo first).
2. Check out with a TA.


### The rubric

Note, this is merely advisory, the precise rubric is on Canvas.

- 100pts total:
	- Messages adhere to the protocol described in class
	- Messages are interpreted correctly by the Java program
	- Message types supported
		- Info string in UTF-8 format
		- Error string in UTF-8 format
		- Timestamp represents the number of milliseconds since reset, 4-byte integer
		- Potentiometer reading in A/D counts, 2-byte integer
		- Raw (unfiltered) temperature reading in A/D counts, 2-byte integer
	- Proper conversion and filtering of temperature on Java side
	- SerialComm debug showing received bytes
	- Correct wiring and sensor readings

Make sure you commit to Github before demoing.

{% include footer.html %}
