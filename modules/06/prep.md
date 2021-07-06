---
layout: default
title: Module 6 Prep
author: Doug Shook and Roger Chamberlain
---
{% include nav.html %}

# {{ page.title }}

This module we introduce protocols, which provide a consistent form of communication between the Arduino and the PC (actually, between any two processors).

We will again be using the Java Simple Serial Connector ([jSSC](https://github.com/scream3r/java-simple-serial-connector)) library for serial communications.  A local copy of the JavaDoc for the library can be found [here](https://classes.cec.wustl.edu/~SEAS-SVC-CSE132/jssc/javadoc/).

![========]({{ "/images/line.gif" | relative_url }})

## Text

Read Chapter 12 in the course
[text]({{ "/files/cc_v0_06.pdf#page=149" | relative_url }}).

*Note*, this chapter is only in draft form, meaning there is lots of
information that is helpful to you in completing this module's activities,
but don't consider it complete by any means.

![========]({{ "/images/line.gif" | relative_url }})

## Data Representation, Protocols, and Observability

| Video | Description |
|:-----:|:-----------:|
|[Data Elements](https://wustl.box.com/s/hjvoo9cq7reyxjkzc0slfro1ifhc20p3) | It is important to understand how data is represented on the Arduino side and the Java side, so this video offers a comparison. |
|[Protocol Design](https://wustl.box.com/s/c2hqdkrm267r7m3n0eymcia0ngek3b2p) | You're now ready to think about designing and implementing protocols to ensure consistent communications from the arduino to Java. |
|[Observability](https://wustl.box.com/s/098tf670ehwqglcgm96yviqgbey0hjsi) | How do we know that our protocol is working as intended? Let's watch it in action! |

{% include footer.html %}
