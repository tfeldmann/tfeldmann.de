---
# https://www.reddit.com/r/arduino/comments/694syx/how_do_i_add_a_deadzone_to_potentiometers_or/
# https://www.reddit.com/r/arduino/comments/3m12jx/adjusting_the_center_or_a_potentiometer_or/
# https://forum.arduino.cc/index.php?topic=692305.0
title: "How to handle dials with arbitrary center positions"
date: 2020-11-28T20:07:41+01:00
slug: ""
description: ""
keywords: []
draft: true
tags: []
math: false
toc: false
---


I received this nice remote control to control some machinery.
It features a dial to set a rotation speed.
This dial has a center position at the top and can be turned left and right and it is my job to make some motor turn into CW or CCW direction accordingly.

> > Easy stuff (I thought).

All I have to do is turn the motor CW if the dial is on the right, CCW if it is on the left and turn the motor off if the dial is in center position. Then adjust the speed depending on how far left or right the dial is set.

I checked the CAN messages I got from the receiver unit and found the dial values lie between `2` (fully left) to `956` (fully right). The center position is at `503`.

You can see problem.

If the dial is roughly in center position I want the device to _stand still_. Not moving a single bit.
Wiggling the dial a bit yields values between `499` to `509` so apparently the remote control is just giving me the raw ADC readings.

So the first thing you do is this.

```c
#define DIAL_MIN          (2)
#define DIAL_MAX          (956)

#define DIAL_CENTER_START (490)
#define DIAL_CENTER_END   (520)

#define DIAL_OUTPUT_MIN   (-255)
#define DIAL_OUTPUT_MAX   (+255)

int read_dial(int raw) {
    if (DIAL_CENTER_START <= raw
        && raw <= DIAL_CENTER_END) {
        return 0;
    }
    return map(
        raw, DIAL_MIN, DIAL_MAX,
        DIAL_OUTPUT_MIN, DIAL_OUTPUT_MAX);
}
```

```python
def mapTH(x, i1, i2, o1, o2, c, t):

    def mapval(v):
        return map_(v, i1, i2, o1, o2)

    idle = mapval(c)
    i_lower, i_upper = min(i1, i2), max(i1, i2)
    o_lower, o_upper = mapval(i_lower), mapval(i_upper)

    if x < i_lower:
        return o_lower
    elif x > i_upper:
        return o_upper
    elif x < c - t:
        return map_(x, i_lower, c - t, o_lower, idle)
    elif x > c + t:
        return map_(x, c + t, i_upper, idle, o_upper)
    else:
        return idle
```
