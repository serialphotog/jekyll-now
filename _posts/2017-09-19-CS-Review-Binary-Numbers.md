---
layout: post
title: CS Review - Binary Numbers
category: Computer Science
tags: [math, computer science, programming]
---

Welcome to what will be the first in an ongoing series aimed at reviewing computer science topics. Today we will be diving into binary number representation. Understanding binary is an important step in gaining a better understanding of how the computer works on a lower level. 

Recall that a computer is made up a whole bunch of integrated circuits, which are in turn made up of a bunch of transistors. These transitors can have one of two states: on or off. We represent these states as a 1 or a 0, respectively. This means that a computer can only work with two different values, 1s and 0s. This is where binary comes in. 

# Quick Review of Number Systems

Binary is nothing more than a number system. To better understand this let's just take a moment to review number systems. 

The system you and I are used to counting in as a part of everyday life is known as base-10 (which we refer to as decimal). Given a number system of base-*k*, we have the digits 0-*k-1* available for use. So, for example, in base-10, we have the digits 0-9 available. 

Binary is nothing but a base-2 numbering system. This simply means that we only have two values available for use, 0 and 1. This also means that everything in binary is represented as a power of 2. 

## Converting base-k to base-10

In general, we can convert any given base-k number into a base-10 number using the following formula:

$$ xyzw_e = x * e^3 + y * e^2 + z * e^1 + w * e^0 $$

# Unsigned Magnitude Binary Representation

Okay, now that we understand that binary is nothing more than a base-2 numbering system, let's look at the simplest of binary representations, **unsigned magnitude**. Unsigned magnitude allows us to store unsigned integers in a binary representation. *NOTE:*, we will cover ways to represent signed numbers and floating point numbers later in this post. 

## Converting From Unsigned Magnitude to Decimal

Consider that we have the binary number: 11001101 and we know it's an unsigned magnitude binary number. How do we go about converting it back into decimal notation (base-10)? Well, it's actually quite simple.

All we have to do is, for each 1 in the binary number, add the corresponding power of 2. This will make more sense when you actually see this in action.

**Example:** Convert 11001101 to decimal.

![_config.yml]({{ site.baseurl }}/images/binary-cs-review/unsigned_magnitude_example.png)

The above figure shows our binary number and the corresponding index for each bit in the number. Notice that we have a one at the following indecies: 0, 2, 3, 6 and 7. This means that to convert our binary number back into decimal, all we have to do is the following:

$$ 2^7 + 2^6 + 2^3 + 2^2 + 2^0 = 128 + 64 + 8 + 4 + 1 = 205 $$

## Converting Decimal to Unsigned Magnitude Binary

Now that we've seen how we can convert from unsigned magnitude to decimal, let's take a look at how to do the opposite. The process is really quite simple. 

In general, you find the largest power of 2 that will fit in your number and write a 1 in that column. Subtract that power of two from the number you are converting and find the next largest power of two that will fit. Do this until you reach 0. Fill all remaining places in with 0s. This will be a lot more clear when you actually see it in action. 

**Example:** Convert 91 to unsigned magnitude.

* The largest power of 2 that fits into 91 is 64. 91-64 = 27
* The largest power of 2 that fits into 27 is 16. 27-16 = 11
* The largest power of 2 that fits into 11 is 8. 11-8 = 3
* The largest power of 2 that fits into 3 is 2. 3-2 = 1
* The largest power of 2 that fits into 1 is 1. 1-1 = 0

This means the powers of two we used are:

$$ 2^6, 2^4, 2^3, 2^1, \mbox{ and } 2^0 $$

This means we simple place a 1 at position 6, 4, 3, 1, and 0 and a 0 at positions 5, and 2:

![_config.yml]({{ site.baseurl }}/images/binary-cs-review/91_in_unsigned_magnitude.png)

# What About Signed Integers? 

Now we've seen how we can represent unsigned integers using unsigned magnitude, but what how do we represent signed integers? Well, as it turns out, there are three possible ways to do this.

## Representing Sign

For representing signed integers we add a sign bit to the front of our binary number. If the sign bit is a 0, the number is positive. If the bit is a 1, then the number is negative. 

This means if we have an 8-bit binary number, we'll need 9 bits to represent this to account for the sign bit. 

## Signed Magnitude

The first way we can do this is through a method called signed magnitude. This is the simplest method. All we do is take our unsigned magnitude representation and throw a sign bit onto the front of the number. 

**Example** 3 = 0011 and -3 = 1011

### Problems

There are two big problems with this method of representing numbers:

1. This gives us two ways to represent 0: 0000 and 1000. This means that we lose the ability to store and extra number, since we have two ways to represent 0.

2. You can't simply perform arithmetic operations using this method. In order to do these operations we will have to first strip off the sign bit and then insert the correct sign bit back in. This would require extra hardware.

## One's Complement

To try to compensate for the issues with signed magnitude, we came up with another method called one's complement. In this method, we store positive numbers just as we would in unsigned magnitude. In the case of negative numbers, however, we do things differently.

For a negative number we invert all the bits. 

**Example:** 19 = 010011 and -9 = 101100

### Problem

We still have a problem with this method: we can still represent 0 as 000000 and 111111. This is why we have yet another method.

## Two's Complement

Two's complement is how we solve the problems with signed magnitude and one's complement. Like one's complement, we again represent positive numbers in the same way and represent negative numbers differently. 

For negative numbers, we flip all the bits and add 1. 

**Example:** 19 = 010011 -19 = 101101

To convert it back, we simply flip all the bits and add 1.

This method is harder, but it solves all the problems we had with the previous methods or representing signed binary numbers. 