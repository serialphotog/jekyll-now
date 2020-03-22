---
layout: post
title: Notes on the Nintendo 64
category: Computer Science
tags: [Computer Science, N64, Emulator]
excerpt_separator: <!--more-->
---

A project that I've wanted to tackle for some time is writing an emulator for the Nintendo 64 video game console. Despite being a console from the mid-90s, this little machine was quite advanced for its time, so I've never had time to undertake this project.

With the unfortunate global Covid-19 crisis, however, I've found myself sitting at home with a whole lot more free time than I'm typically used to. As such, now seems like as good a time as any to at least start looking into building an emulator for the N64. 

This post will serve as some of my research notes on the system. This is mostly for my reference, but perhaps it will prove useful to someone at some point.

<!--more-->

# CPU

The N64 uses a 64-bit NEC VR4300 CPU. This is a licensed version of the MIPS R4300 CPU.

* 64-bits
* RISC 5-stage scalar in-order execution
* 24kb direct mapped L1 cache. (There is not L2 cache)
* The floating point unit exists on the coprocessor. There is no dedicated path on the VR4300 for floating point operations. This task is shared with the integer pipeline, thus floating point operations stall the pipeline
* Clock rate: 93.75 MHz
* Uses a narrow 32-bit system bus
* Fully supports 64-bit operations, but most games stick to 32-bit for performance
* Does not support DMA. All memory requests must go through the coprocessor.
* Instruction set: MIPS R4000, 64-bit
* Addressable memory: 4 GB (up to 1TB virtual memory)
* Has the capacity to execute 93 million operations/second.

# Reality Coprocessor

The coprocessor, called the Reality Coprocessor, largely handles the N64's graphics and audio operations.

* 64-bits
* Clocked at 62.5 MHz
* Split into two components: Reality Display Processor (RDP) & Reality Signal Processor (RSP)

## RSP

The Reality Signal Processor (RSP) controls the 3d graphics and audio functionality.

**NOTE:** Audio can also be handled by the CPU.

* Based on a MIPS R4000, 128-bit integer vector processor (8-bits)
* Programmable through microcode (can add/modify existing instructions)
* Supports: Transformation, clipping, lighting, triangle setup, audio decoding (also able to be done on the CPU).
* Displayable polygons are initially about 100,000 polygons/second without quality loss.

## RDP

The Reality Display Processor (RDP) largley handles rasterization and all pixel drawing operations.

* Z-buffering: Spatial relationship between 3D objects
* Anti-aliasing
* Texture mapping
* Bilinear filtering
* Mip-mapping
* Trilinear mip-map interpolation
* Perspective correct texture mapping
* Environment Mapping
* Gouraud shading

There is a 128-bit internal databus between the RSP and RDP that has a bandwidth of about 1 GB/second.

Supports a resolution from 256x224 to 640x480 pixels.

# Memory

The N64 came equipped with 4.5mb of RAM, but could be upgraded to 9mb with an expansion pack.

* 9-bit databus clocked at 250 MHz
* Because the memory uses DDR technology, there is a maximum bandwidth of 562.5 MB/second
* RAM latency is about 640ns

# That's It... For Now

Note that, as I said at the start of this post, these are largely just my personal research notes for working with the N64. As such, these will continute to be updated as I progress with this project.
