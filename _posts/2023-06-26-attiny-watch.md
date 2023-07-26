---
layout: project
title:  "ATtiny402 Binary Wristwatch"
date:   2023-06-26 11:34:18 -0500
categories: projects
tags:
    - project
    - pcb
    - attiny
    - 3d-printing
description: "A simple, ATtiny402 powered binary wrist watch"
image: /assets/images/binary_watch/header_image.webp
repo: tux2603/watch
published: false
---

One of my friends gave me the idea for this project at a game night, so I decided it would be a nice surprise to make him one for his birthday. The basic idea is pretty simple: have a matrix of LEDs that displays the current time in binary coded decimals with a few buttons to set and check the time. The main constraint was that the whole design had to small enough to fit on a wrist and be power efficient enough to run off a small battery for a reasonable amount of time. I'd been wanting to try out one of the microcontrollers in the ATtiny series for a while now, so I decided I would give them a spin with this project.

## Circuit Design and Component Selection

Normally a four-digit binary value represented in BCD would require 16 individual bits, but since we're only displaying time we know that certain numbers will never appear in certain positions. For a twelve hour watch, this means that we can get away with only 12 LEDs. If we added an extra LED we could have support for 24 hour time or an indicator LED for AM/PM, but that would have required an extra pin on the microcontroller. With the 12 LEDs for twelve hour time, I was able to use only 4 GPIO pins to drive the LEDs using a technique called [Charlieplexing](https://en.wikipedia.org/wiki/Charlieplexing). Charlieplexing takes advantage of the fact that electricity can only flow in one direction through an LED and that the GPIO pins on most microcontrollers can be used as tri-state buffers. Using this technique, $$n$$ GPIO pins can be used to drive up to $$n^2 - n$$ LEDs. In this case, we have 12 LEDs that we need to drive, which is the maximum number of LEDs that can be driven with a four pin Charlieplexing matrix. The layout of the LED matrix is shown in [Figure 1](#fig1) below:


<span class="figure" id="fig1">
    ![LED Matrix](/assets/images/binary_watch/led_matrix.png)
    Figure 1: Twelve LED Charlieplexing matrix
</span>

For inputs, I decided to use a two-button setup. One button will be used to wake the watch from its low-power sleep state and the other button will be used to set the time. To keep the number of pins required low, the buttons will share a single analog input pin. When the view button is pressed the voltage on the analog input will be pulled to ground, and when the set button is pressed the voltage will be $$V_{DD} \over 2$$. This is accomplished with two high-value resistors as seen in [Figure 2](#fig2). These resistors need to be a high enough value to not have an excessive current draw when the buttons are pressed, but not so high that they'll be above the recommended input impedance of the ADC. I chose 10kΩ resistors for this reason, which will have a worst-case current draw of $$V_{DD} \over 10kΩ$$, or 0.33mA when the buttons are pressed.

<span class="figure" id="fig2">
    ![Button Circuit](/assets/images/binary_watch/button_circuit.png)
    Figure 2: Button circuit
</span>

The only other pins that will need to be included are the two power pins and a single pin for UDPI programming. This meant I was able to use the ATtiny402 microcontroller, which has exactly the number of pins needed. The ATtiny is available in an eight pin SOIC package, which should help keep the size of the final PCB down, and can be run on anywhere from 1.8V to 5.5V. For power, I initially opted to use a CR1612 coin cell battery, but in the final design I decided to use a CR2032 battery instead. The CR2032 is a larger battery, increasing the overall size of the watch, but is more commonly available and will be able to power the watch for much longer. For the resistors and LEDs I decided to use 0603 footprints. These are pretty small components, but are still manageable for hand-soldering and let me keep the size of the PCB down. I decided to use Panasonic's EVP-AA202K SMD buttons for the inputs, mostly because they were small and I liked the way they looked.

## PCB Design

For the PCB, I wanted a round design that was roughly the size of a US quarter. To keep fabrication simple and cheap, I decided to use a two layer design. The top layer would have the majority of the traces and would be used for the LED matrix, resistors, buttons, and ATtiny. The bottom layer would be used for the battery holder, a ground plane, and some various short traces for when routing traces on the top layer wasn't possible. I also decided to include a set of small solder pads on the top layer for external power and programming connections, and in the second iteration of the PCB I added a fourth pad to connect an oscilloscope probe for easier calibration.

Most of the work that went into the PCB was finding a layout for the LEDs that made routing as easy as possible. I wanted the twelve LEDs and the four corresponding current limiting resistors to be laid out in a four-by-four grid at the center of the watch face. Having the LEDs centered like this makes sense from a design perspective, but it does mean that they will be opposite from the massive central pad on the battery holder. This meant that the traces for the LEDs had to be routed entirely on the front side of the PCB, since any vias to the back side would be connected directly to ground. Eventually, I found a layout that allowed me to route all of the traces on the front side of the PCB, I just needed to run a few traces between the pads of the 0603 footprints. The final layout of the PCB is shown in [Figure 3](#fig3) below. I decided not to include any sort ground plane on the top layer, since I was interested in trying out some PCBs with a clear solder mask.

<span class="figure" id="fig3">
    ![PCB Layout](/assets/images/binary_watch/pcb_layout.png)
    Figure 3: PCB Layout
</span>

## 3D Printed Enclosure

For the enclosure, I wanted to keep the design as simple as possible. I decided to use a two-piece design consisting of the main casing, and a screw-in cover on the back to allow for easy access to the battery. 