---
layout: post
title: Aery32 makes the development with 32-bit MCUs fun
description: Aery32 is a combination of the development board, which has AVR32 microcontroller, and software framework for rapid prototyping.
author: Kim
tags:
- Aery32
- AVR32
---

32-bit microcontrollers (MCUs) are often considered hard to use and cumbersome to adapt. Thus 8-bits are still popular among hobbyist and hardware designers, although 32-bits offer way more functionality and uses even less power. We have used 32-bits in our projects and decided to share our knowledge to help you to make fun with 32-bits. To smooth the adaptation of 32-bits we have designed a development board named as Aery32. It represents an excellent PCB design and good quality components and thus can be even a part of the end product. In addition to the development board we have also developed a software framework. The ambition of the software framework is to provide a lean library and project structure for agile embedded software development.

__The best part!__

Aery32 will be completely open sourced. The development board will be licensed under [Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0)](http://creativecommons.org/licenses/by-sa/3.0/) and the software framework will be subject to [the new BSD license](http://www.opensource.org/licenses/BSD-3-Clause).

The Aery32 development board in detailed:

- Atmel AVR32 UC3A1 128KB
- 12 MHz oscillator
- USB 2.0 (full speed). Also used to program the board so the external programmer is not needed.
- 2 x 40 pin headers. Allows stackable additional boards.
- Reset button
- Slide switch to select whether a bootload or program in the ram is started when board is powered
- JTAG-interface

<a class="fancy" href="/images/aery32top.png" title="Aery32 top side">
<img itemprop="image" src="{{ site.url }}/images/thumbs/aery32top.png" alt="Aery32 top side" /></a>
<a class="fancy" href="/images/aery32bottom.png" title="Aery32 bottom side">
<img src="/images/thumbs/aery32bottom.png" alt="Aery32 bottom side" /></a>

We are about to start assembling next week and looking forward to soft release the software framework in GitHub for early adopters. Consider this like a release candidate. The final release is likely happen in March at www.aery32.com. Early adopters may like to write us (comment below) what kind of project you would like to start by using Aery32. We are sharing few boards free and you may be one of the first having it :)

