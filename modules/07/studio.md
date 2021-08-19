---
layout: default
title: Studio 7 - From a Motor to a Car
author: Roger Chamberlain
---
{% include nav.html %}

# {{ page.title }}

![========]({{ "/images/line.gif" | relative_url }})

## Introduction

Click [here](https://wustl.instructure.com/courses/68860/assignments/289486) to access the Canvas page with the repository for this studio.

### Objectives

The topic of today's studio is to experiment with controlling something in the physical world. We will start with controlling a motor, and then use a pair of motors to control a semi-autonomous car.

By the end of this studio you should know:

- how to control an analog output.

![========]({{ "/images/line.gif" | relative_url }})

## The Studio

We will start by following the instructions provided by SparkFun in their [Motor Basics tutorial](https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v41/circuit-5a-motor-basics). The sketch is available in your repo for this studio under the name `MotorBasics`.

Notice how different speeds are delivered to the motor via the `analogWrite()` method. Let's explore how that works in a little more detail.

**FIMXE: add detail.**

### Build a Car

Our next task is to use the motors (two of them) to drive the wheels of a car.  OK, not a very big car, but a mobile device that can move on its own.  Here is a picture of what it will look like:

![car](SIK_Updates-5B-Remote_Controlled_Robot.jpg)

SparkFun's [Remote Controlled Robot tutorial](https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v41/circuit-5b-remote-controlled-robot) shows how to do the physical construction.
The sketch is available in your repo for this studio under the name `RemoteControlRobot`.

![========]({{ "/images/line.gif" | relative_url }})

## Finish up

1. Commit your code and verify in your web browser that it is all there.
2. Get checked out by a TA.

Changes to repo structure:

- `MotorBasics/`
	- `MotorBasics.ino`
- `RemoteControlRobot/`
	- `RemoteControlRobot.ino`


![========]({{ "/images/line.gif" | relative_url }})

## Key Concepts

- Analog Output
	- Pulse Width Modulation (PWM)

{% include footer.html %}
