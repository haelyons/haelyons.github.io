---
id: 4
title: "Thoughts on Networked Modular Synthesis "
subtitle: "Routing control voltages over the internet via MaxMSP"
date: "2021.10.08"
tags: "max, hardware, synthesizer, Eurorack"
---
### Objectives
This post will discuss a solution adopted during group construction of a Eurorack standard modular synthesizer, which allowed for limited simulation and testing of modules despite members being in isolation.
Personally, I built a Triple VCA (Voltage Controlled Amplifier) – used for multiplying signals to create interaction between parameters from various modules – and a Digital Control Voltage Sequencer, designed to program melodies using voltages sent over a timestep determined by the Clock Generator.

### Problem & Background
Several strategies were initially discussed to deal with the constraints placed on practical laboratory work and access to necessary materials. Among them, the implementation of DIY oscilloscopes and signal generators was considered a possible route to self-sufficient prototyping and audio performance tests, as all members had left over ELEC2856 MBeds (LPC1768) which could be converted into functional scopes.[^1] Contacting manufacturers for more detailed models of components was also mentioned; I contacted THAT Corporation and received a series of detailed models and application circuits for the initial design of the VCA. Unfortunately, it was not possible to acquire the physical component, due to lead times being heavily impacted by COVID, resulting in the final design instead using more common amplifier ICs – JFET, OTA. The schematic below shows a modified THAT2180 IC application circuit, implemented with a highly detailed SPICE model directly sourced from the manufacturer. The schematic below does not reflect the final VCA, but gives a clear illustration of its functions, with 2 inputs (control voltage, and main signal) and a single audio out (the multiplied signals).

![THAT2180B VCA](https://raw.githubusercontent.com/haelyons/Website-Content/master/EURORACK1A.png)

### Solution
Several software solutions were proposed wherein the modular synthesizer would be moved to an entirely digital environment. Environment in this sense refers to programming languages designed for sound synthesis, such as Juce (C++), the Matlab Simulink package, and MaxMSP. The first two were not implemented due to several factors, including the learning curve for new programming languages during term, time already invested in conventional circuit simulation using LTSpice and a prioritisation of completing modules with physical components. However, Max was tested in the context of transmitting control voltages over the internet, considered useful both for constructing simulations of module functionality and for the possibilities of networked, distributed testing of said modules.

![Distributed Signal Patch](https://raw.githubusercontent.com/haelyons/Website-Content/master/EURORACK1B.png)

MaxMSP is a visual, object-oriented programming language designed for use with Ableton Live. With group member Daniel Burgoyne, responsible for the Clock and Envelope Generators, sending control voltages over the internet using the Open Sound Control (OSC) protocol was tested. By port forwarding the local router on 5050 and sending Daniel the relevant IP address, we were able to find the latency of this networked modular synthesis method. The results are displayed below, taken from the ‘patch’ where the testing was conducted. The left-hand side is the signal source, or oscillator. In the implementation discussed for this system it was agreed that due to latency being too high for real-time audio transmission (~150ms) the VCO simulator (Jack O’Connor) would receive CVs from all the other modules, processing them with a local version of the code. This would minimise latency as audio would not have to be sent back and forth over the network, instead relying on much simpler control voltages, with audio output being routed back through zoom.

![Sequencer Patch](https://raw.githubusercontent.com/haelyons/Website-Content/master/EURORACK1C.png)

This method was intended to be expanded to read control voltages over I2C (serial communication) from the microcontroller-based modules, allowing networked modular interaction. Figure 2 (from the Max serial object documentation) outlines testing to check the serial connection, and subsequently receiving serial values from port ‘A’ at a baud rate of 19200. [^2] A patch was also developed to simulate networked communication between the clock generator and sequencer modules, shown in Figure 3. The matrix object on the left is a 16-step sequencer, with the Y axis nodes corresponding to notes from the 12-tone chromatic scale used in Western music, and the X axis corresponding to the time step. The ‘udpreceive’ object accepts messages sent to port 5050 by the clock generator. Its 4 trigger outputs were mapped to a counter object, incrementing the ‘step’ as messages were received.


### Discussion
While this solution mitigated some of the COVID-based limitations on project work, it did not provide a solution which was accurate or reliable enough
for true networked testing of hardware modules. Rather it prompted a distanced jam-session and made me question if the interactivity between modules that Eurorack and modular synthesizers use to great effect could be applied over the internet.
A possible example could be the direct interaction of a live, virtual audience with a modular synthesizer, which would likely increase engagement and create a democratised music production environment.

[^1]: http://www.ijera.com/papers/Vol5_issue3/Part-1/T50301106109.pdf
[^2]: https://docs.cycling74.com/max7/refpages/serial
