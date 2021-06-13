---
title: Using SerialComm to Communicate between Java and Arduino
---
{% include nav.html %}

# {{ page.title }}

**If you’re confused about the purpose of the SerialComm object you’ll be using for the next few assignments, look through this guide to get a TA’s explanation on the class. If you still have questions, reach out to a TA or instructor during office hours or through Piazza!**

## An Overview on Arduino Communication

Up until now, you have been using Arduino’s Desktop IDE to program your Arduino. The
IDE automatically sets up a connection to your Arduino through the [Arduino’s serial port](https://www.arduino.cc/reference/en/language/functions/communication/serial/), making it easy to quickly create a new sketch, and the built-in serial monitor and serial plotter let you read and write data as your program runs.

You may have noticed, however, that these sketches are saved with **.ino** file extensions. This limits the versatility of your Arduino. For example, if you wanted to use your Arduino as a custom game controller, your entire game would have to be written with Arduino’s C/C++ -based libraries. This can be changed by using the Arduino's serial port to connect to a different program other than the IDE's built-in serial monitor and plotter. In the next few studios and assignments, you will create a connection between a running Arduino sketch and a Java application.

The Arduino Uno comes with **one built-in serial port** that relies on digital pins 0 and 1. Previously in the class, you might have tried to open both the serial monitor and plotter at the same time but couldn’t. **Only one program can interact with the Arduino at any given time through the serial port.** Your Arduino cannot communicate with a Java application and the Arduino IDE’s serial monitor or plotter at the same time. You’ll end up with a Java exception if the serial port is already in use.

(There are some ways around this, like using a board with more ports than the Uno, purchasing extra hardware, or using the [SoftwareSerial library](https://www.arduino.cc/en/Reference/SoftwareSerial), but that’s beyond the scope of this course.)

Sending and receiving data on the Java end will be similar to the reading and writing functions you have already implemented in Arduino for previous assignments, but a little prep is necessary before you can begin transmitting data back and forth.

In the upcoming studio, we provide you with an incomplete Serial Communication class (SerialComm) and the JSSC (Java Simple Serial Connector) library that contains the basic functions you’ll need to communicate with your Arduino. We highly recommend browsing through the methods for the SerialPort class in the [JSSC javadocs](https://classes.cec.wustl.edu/~SEAS-SVC-CSE132/jssc/javadoc/) if you haven't already.

## Breaking down the SerialComm class

The SerialComm class has 2 member variables that shouldn't be changed:

1. A SerialPort object named `port` that is created and opened when the SerialComm object is initialized, _and_
2. A private boolean `debug,` which is initialized to false

A complete SerialComm class should have the following methods:

| method name | purpose |          |
|:------------|:--------|:---------|
| public SerialComm(String name) | the constructor for the SerialComm object, opens the serial port and sets the proper parameters | PROVIDED |
| public void setDebug(boolean mode) | sets the value of the debug variable | PROVIDED |
| public void writeByte(byte b) | accepts 1 byte as a parameter and sends it to the Arduino; <i>if</i> debug was previously set to true, prints a debugging message with the sent byte value after the byte was successfully sent | INCOMPLETE |
| public boolean available() | checks for and returns the availability of incoming bytes <b>without</b> actually reading incoming bytes | INCOMPLETE |
| public byte readByte() | reads and returns a <b>single</b> byte; <i>if</i> debug was previously set to true, prints a debugging message with the received byte value | INCOMPLETE |

Once you have finished these functions, you will be able to send and receive input from a Java application just like you would from the Arduino IDE’s serial monitor. The addition of the debug variable is meant to help troubleshoot any errors that might occur when sending bytes back and forth. We recommend using it to determine if incorrect or missing values are due to serial communication issues or local code errors. Feel free to add extra messages if it helps you.

## Using the SerialComm object

To use your newly finished SerialComm class, simply make a new instance of the object in the class that you run as a Java application.

        SerialComm comm = new SerialComm("My portname");
        comm.setDebug(true); // if you want extra debugging output

If Java can't find the right port, **double check the port name** of your Arduino, especially if you’re using a mac and the name appears to use an O or zero. Use the public functions to relay input from Eclipse’s console to your Arduino and use the setDebug() function if you want to include debugging output. If you’ve changed your Arduino code, be sure to reupload your sketch before running your Java application, otherwise just re-run your Java program to see changes on the Java side.

When running Java alongside your Arduino program, also **check that the previous run of the Java program has been terminated**. Arduinos work really well for projects that require continual input checking, but Java will terminate unless a loop is used to keep it running, meaning you may have to manually stop the Java application each time you run it.

{% include footer.html }}
