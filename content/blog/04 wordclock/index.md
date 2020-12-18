---
title: "IoT Wordclock With ESP32"
date: 2020-11-27T16:20:15+01:00
slug: "esp32-iot-wordclock"
description: ""
keywords: []
draft: true
tags: ["ESP32", "IoT", "project"]
math: false
toc: true
---

At my company we built some nice wordclocks as christmas presents for our families.
This article is some kind of a build log where we document what we learned for anyone interested.

## Pictures

Here some pictures of the finished product:

{{< ps-gallery >}}
{{< ps-img src="images/*" >}}
{{< /ps-gallery >}}

### Features

- 11 x 10 white LED matrix
- Four corners for the minutes with white, red, green and blue LEDs
- Each LED is individually dimmable
- Based on the ESP32
- Low power consumption of 10W with full brightness and enabled wifi
- Can be driven via Micro-USB-B or a DC jack
- 150 mA @ 5V at full brightness with wifi enabled. It can easily be powered by any USB-port.

### Technological overview

- ESP 32
- Platform.io
- KiCad for the PCB

## How it works

_Description of the display schematics_

## PCB

The PCB is mostly SMD with just a few big THT components on the backside. That's because we have a automatic placer in the company.

## Firmware

## Housing and light array

## Home Automation Integration

So this clock features a powerful and relatively expensive BME680 sensor which measures ambient temperature, humidity, air pressure and air quality.

So I added some code to add this to my Home Assistant installation and I love it. It's fun to see the historic sensor measurements.
