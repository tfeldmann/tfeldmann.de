---
# https://www.reddit.com/r/arduino/comments/694syx/how_do_i_add_a_deadzone_to_potentiometers_or/
# 
# https://forum.arduino.cc/index.php?topic=692305.0
title: "How to add software deadzones to potentiometers"
date: 2020-11-28T20:07:41+01:00
slug: "how-to-add-software-deadzones-to-potentiometers"
description: ""
keywords: []
draft: true
tags: ["embedded", "algorithm"]
math: false
toc: true
---

<style>
.dial {
    width: 50%;
    margin: 0 auto;
}
</style>
{{< figure src="images/Remote.jpg" caption="Remote control featuring a `trim` dial which needs a center position" width="100%">}}

## The problem

I received this nice remote control to control some machinery.
It features a dial with a center position at the top (value `0`) which be turned either left (value `-100 %`) or right (value `+100 %`).

There are some recurring problem with potentiometers used like this:

**What you want:**

{{< figure src="images/nice.svg" class="dial" >}}

- a linear range of output values from -100 % to +100 % without big steps
- a clearly defined center position at 0 % without wiggling
- clearly defined outputs of +/- 100 % at either side

**What you have:**

{{< figure src="images/monster.svg" class="dial" >}}

- not the full reading range (e.g. `12...986` instead of `0...1024` on a 10 bit ADC)
- a center value that may flicker and change with temperature.

## Finding a solution

The number one tip you [find](https://www.reddit.com/r/arduino/comments/694syx/how_do_i_add_a_deadzone_to_potentiometers_or/) [quite](https://www.reddit.com/r/arduino/comments/3m12jx/adjusting_the_center_or_a_potentiometer_or/) [often](https://forum.arduino.cc/index.php?topic=692305.0)
is to define a dead zone in which you set your values to zero.
This 

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

## Solution and verification

## Try it out!

## Next steps
