---
layout: post
title: Let's Write a Simple Disassembler for the Intel 8080 Microprocessor
category: Development
tags: [computer science, programming, c, disassembly]
excerpt_separator: <!--more-->
---

Typically my weekends are spent <a href="https://serialphotog.com/" target="_blank">hiking with my camera</a>, but this past weekend I was knocked down by a pretty nasty cold. As a result, I found myself toying around with various programming projects I’ve had laying around. Here for awhile I’ve been curious how emulators work, so I took this opportunity to start venturing down that rabbit hole. During this excursion, I decided to start my foray into emulators by writing one for the game Space Invaders, which ran on an Intel 8080 microprocessor. It seemed only natural to get started by writing a disassembler for the 8080 to get things started.

![_config.yml]({{ site.baseurl }}/images/intel-8080-disasm/Intel8080DisassemblerOutput-1.png)

What I came up with is a pretty simple (albeit long) bit of C code to disassemble binary code compiled for the Intel 8080. So, without further ado, let’s jump on in and see how I did it!

<!--more-->

# The Intel 8080 Microprocessor

Released in 1974, the Intel 8080 microprocessor was the second of the 8-bit microprocessor chips released by Intel. It offered a clock frequency of up to 2 MHZ.

The chip contains seven 8-bit, general-purpose registers (A, B, C, D, E, H, and L). In addition, the CPU contains a 16-bit PC (program counter) and a 16-bit SP (stack pointer) register. All instructions are encoded using a single byte and it’s a little endian architecture. Operations can be followed by one or two bytes of data, which can be a memory address, port number or operand.

In order to write a disassembler for a microprocessor, we will of course need a good reference for the instruction set. <a href="https://pastraiser.com/cpu/i8080/i8080_opcodes.html" target="_blank">Here is the one I used</a>.

# Step 1: Basic Program Structure

As with any software project, it’s best if we sit down and do some pre-planning before we just start banging on the keyboard. In order to disassemble the binary files, we will need to read the files into some sort of buffer and iterate through the bytes in that buffer. To keep track of our place in that buffer we will need some sort of counter.

Additionally, we will need a method that will take a reference to that buffer and the current position in the buffer that will disassemble the current opcode. This method will need to return the amount that we need to increment our counter. With that in mind, let’s set up the basic program structure:

```c
#include <stdio.h>
#include <stdlib.h>
// Forward declaration for the disassembler routine
int disassemble(unsigned char *buffer, int pc);
int main(int argc, char **argv) 
{
    FILE *f = fopen(argv[1], "rb");
    if (f == NULL)
    {
        printf("Error: couldn't open %s\n", argv[1]);
        exit(1);
    }
    // Read the file into a memory buffer
    // Perform the disassembly
    return 0;
}
// Performs the actual disassembly of an instruction
//
// Params:
//      unsigned char *buffer - The buffer containing the machine code
//      int pc - The program counter. Used to track our place in the buffer
//
// Returns:
//      int - The new program counter location. The program counter is incremented to track
//            where the next instruction is, particularly after a multi-byte instruction is
//            encountered.
int disassemble(unsigned char *buffer, int pc)
{
}
```

This code should be pretty straightforward, so I won’t spend too much time going over it. First, on lines 1-2 we import the libraries we are going to need. On line 5 we declare a forward declaration for the method that will perform the disassembly on a given instruction from the buffer. It takes in an unsigned char *, which is a pointer to our buffer, and an int, which is our program counter that tracks the current place in the buffer.

Within the main method we attempt to open the first command line argument as a file. We also set up the implementation for the disassemble method, but it is currently empty. We’ll have to fix that soon!

# Step 2: Read The Binary Data Into The Buffer And Start Disassembly

```c
int main(int argc, char **argv)
{
    [...]
    
    // Read the file into a memory buffer
    fseek(f, 0L, SEEK_END);
    int fsize = ftell(f);
    fseek(f, 0L, SEEK_SET);
    unsigned char *buffer = malloc(fsize);
    fread(buffer, fsize, 1, f);
    fclose(f);
    // Perform disassembly
    int pc = 0;
    while (pc < fsize)
    {
        pc += disassemble(buffer, pc);
    }
    return 0;
}
```

This is the part where we read the actual binary data from our file into a memory buffer and start the disassembly process.

On lines 6-8 we determine the size of our binary file and store that value in the fsize variable. We then create a buffer of this size on line 10 and read the data into said buffer on line 11.

Lines 15-10 are where we are starting the disassembly process. We create a pc variable and initialize it to a value of 0. This will be our program counter, which will track our current position within the buffer. We then loop over the entire buffer. During each iteration of the loop we disassemble the current spot of the buffer and increment the program counter by the amount returned by our disassemble function.

This will hopefully become more clear after we dig into the disassemble function.

# Step 3: Starting The Disassemble Function

```c
int disassemble(unsigned char *buffer, int pc)
{
    unsigned char *opcode = &buffer[pc];
    int opbytes = 1;
    printf("0x%04x  ", pc);
    switch (*opcode)
    {
        default:
            // We encountered an unknown instruction
            printf("Unknown Instruction: 0x%02x", *opcode);
            break;
    }
    printf("\n");
    return opbytes;
}
```

Here is where we are setting up the basic structure of the disassemble function. This is the real meat of our disassembler!

First off, on line 3, we initialize a pointer, unsigned char *opcode, to point to the current position in the buffer. We then create a variable called opbytes, which we initialize to 1. This is where we are going to track how much the program counter needs to be incremented by for the next iteration of the loop. Remember, some instructions are just a single byte opcode, while others can take 2 or 3 bytes.

On line 5 we are simply printing out the current offset into the disassembly.

This brings us to our switch statement. This is where all of the disassembly magic happens! For now, I have just populated it with a default route for when an unknown instruction has been encountered. If you were to run the disassembler at this point, it would simple say that every instruction is an unknown one, since we haven’t added any actual instructions to our switch statement yet. That’s what we’ll do in the next and final step.

Finally, we return the value of opbytes, which will be used to increment the program counter back in the main while loop.

# Step 4: Add Instructions To The Disassembler

Now it’s time for the fourth and final step, which is to populate that switch statement with all of the instructions for our CPU architecture. This is where that instruction set reference I mentioned at the beginning of this post come in handy! I’m going to leave adding all of the instructions up to you, but I’ll do the first few here to get you started.

```c
switch (*opcode)
{
    case 0x00:
        printf("NOP");
        break;
    case 0x01:
        printf("LXI\tB, #$0x%02x%02x", opcode[2], opcode[1]);
        opbytes = 3;
        break;
    case 0x02:
        printf("STAX\tB");
        break;
    case 0x03:
        printf("INX\tB");
        break;
    case 0x04:
        printf("INR\tB");
        break;
    case 0x05:
        printf("DCR\tB");
        break;
    case 0x06:
        printf("MVI\tB, #$0x%02x", opcode[1]);
        opbytes = 2;
        break;
    case 0x07:
        printf("RLC");
        break;
    case 0x09:
        printf("DAD\tB");
    default:
        // We encountered an unknown instruction
        printf("Unknown Instruction: 0x%02x", *opcode);
        break;
}
```

I think what’s going on here in this switch statement is pretty straightforward. One thing to notice is that, in the cases in which there are two bytes of data for the instruction, we have to present the data from the buffer in reverse order (such as is the case with the LXI operation). This is because our architecture is little endian.

That’s really all there is to writing a simple disassembler, though! I’ll leave it as an exercise for you to implement the full instruction set.