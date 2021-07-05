---
layout: default
title: Studio 5 - Digital Inputs and More Practice with Delta Timing
author: Ben Stolovitz and Shuya Ma; Revisions by Bill Siever, Roger Chamberlain, James Orr
---
{% include nav.html %}

# {{ page.title }}

![========]({{ "/images/line.gif" | relative_url }})

## Introduction

Click [here](https://wustl.instructure.com/courses/68860/assignments/289456) to access the Canvas page with the repository for this studio.

### Objectives

By the end of this studio, you should know:

- how to read digital inputs,
- how to debounce a mechanical push-button switch,
- more about delta timing

![========]({{ "/images/line.gif" | relative_url }})

## Push-button inputs

The first thing we'll do today is explore push-button inputs in more detail.  This includes investigating the bouncing properties of the buttons themselves.

### Mirror

Complete the sketch called `mirror` so that it reads the state of the push-button input and sends it to an LED digital output (i.e., flash an LED on when the button is pressed).  The program should repeat this operation continuously.

Extend this program to count the number of times the push-button has been pressed.  For example, if the button is pressed and released three times it should count to three.  You can detect a press either as the input changes from  0 to 1 or as it changes from 1 to 0.  Each time the count increases, display the current value of the count in the Serial Monitor.

Does the count always accurately reflect the number of times the button has been pressed?  

### Debounce

Complete the sketch called `debounce` so that it reads the state of the push-button using a proper debouncing technique, like the one covered is in this [tutorial](https://arduino.cc/en/Tutorial/Debounce) from Arduino.  (**This example uses an external resistor as a pull-down. We have been using an internal pull-up, which requires different logic!**) In your opinion, does the tutorial use proper delta-time techniques?

Check that your push-button is appropriately debounced by repeating the counting experiment above.  What debounce delay is necessary for your push-button to be reliably debounced?

![========]({{ "/images/line.gif" | relative_url }})

## Handling More Traffic

The rest of his module's studio will be a continuation of the Stoplight Delta Timing Studio from Module 3. Replace the stub stoplight.ino with yours from Module 3. Then wire your Arduino to once again have the red, yellow, and green traffic lights, the "walk" light, and the "don't walk" light as in Module 3's Studio.

Now update your sketch to include a two way intersection, like:

![The crossing diagram.]({{site.url}}{{site.baseurl}}/studios/03/CrossingAnnotated.png)

It should fairly switch betewen: 
1. Allowing traffic to go north/south 
2. Allowing traffic to go east/west
3. Stopping all traffic and allowing pedestrians to cross in either direction

You will need to:
- Add three more LEDs
- Create a new state machine diagram with appropriate states
- Update your code to reflect the new diagram

How many states did you include in your new FSM?  Draw the FSM you have designed [on the FSM designer](https://wilsonem.github.io/fsm/).

![========]({{ "/images/line.gif" | relative_url }})

## Studio Demo 2 

The full, two-way stop light with a crossing should work something like this (Again, *WALK* is Blue, but this time *DONT WALK* is Yellow):

 <iframe src="https://wustl.app.box.com/embed/s/4bpv389eh14gqdi3190p11871zi61rx6" width="500" height="800" frameborder="0" allowfullscreen webkitallowfullscreen msallowfullscreen></iframe> 

![========]({{ "/images/line.gif" | relative_url }})

## Finish up

1. Commit your code and verify in your web browser that it is all there.
2. Get checked out by a TA.

Things that should be present in your repo structure:

- `debounce/`
  - `debounce.ino`
- `mirror/`
  - `mirror.ino`
- `stoplight/`
  - `stoplight.ino`

![========]({{ "/images/line.gif" | relative_url }})

## Key Concepts
- Delta timing
	- Maintaining east/west traffic light
	- Maintaining north/south trafic light
	- Maintaining walk light and blinking don't walk light

- FSMs
	- Transitioning from one state to another


{% include footer.html %}
