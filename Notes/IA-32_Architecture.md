## Introduction
What is Assembly language:
- Instructions that the CPU can execute
- Compatiblity: > Intel 8000 series of microprocessor
	
Assembly language syntax types:
- AT&T: dominant in the Unix world
- Intel: dominant in MS-DOS and Windows env
	
Syntax differences:
- AT&T: source comes before the destination
- Intel: destination comes before the source

## Table of Contents

<!-- TOC -->
- [Introduction](#introduction)
- [Bits and Memory](#bits-and-memory)
- [x86 Basic Architecture](#x86-basic-architecture)
- [General Purpose Registers](#general-purpose-registers)
- [Segment Registers](#segment-registers)
- [Instruction Pointer Register (EIP)](#instruction-pointer-register-eip)
- [Control Registers](#control-registers)
- [Flags](#flags)
- [Stack](#stack)
- [Heap](#heap)
<!-- /TOC -->

## Bits and Memory
What is a:
- Nibble: 4 bits
- Byte: 8 bits
- Word: 2 bytes - 16 bits
- Double word: 2 words - 4 bytes - 32 bits
- Quad word: 4 words - 8 bytes - 64 bits

In a computer:
- Memory is divided in bytes
- Each byte of memory has its own unique address
- 32-bit CPU can fetch a double word from a memory address, execute it then fetch another

Example (GDB debugger) examination of ESP register:
```
(gdb) x/1xw $esp
0xffffd040:   0xf7fac3dc
Memory location of 0xffffd040 has the value of 0xf7fac3dc
```
## x86 Basic Architecture
Components:
- CPU
- Memory
- I/O devices (Input/Output)
- System bus

Notation:
- Little-endian notation: lower-value bytes appear first in order eg: 0x01 = 01 00 00 00

CPU:
- The CPU consists of:
	- Control Unit: Retrieves and decodes instructions from the CPU and then stores and retrieves them to and from memory
	- Execution Unit: Where the execution of fetching and retrieving instructions occurs
	- Registers: Internal CPU memory locations used as temporary data storage
	- Flags: Indicate events when execution occurs
- CPU's execution steps in x86 systems:
	- Fetch a double word from the memory
	- Execute the procedures that the double word indicates
	- When execution complete, re-fetch a double word
	- The CPU register called EIP (Instruction Pointer) contains the address of the next double word to be fetched
	- The fetch and execute process is tied to the system clock
Memory 
- Memory Models:
	- Flat Memory Model
	- Paged Memory Model
	- Segmented Memory Model
- Flat Memory Model:
	- Protected mode
	- Executed programs receive a 4GB address space to which any 32-bit register can potentially address any of the four billion memory locations except for those in protected areas defined by the operating system
	- Physical memory may be larger than 4GB however a 32-bit register can only express 4,294,967,296 different locations (2**32).
	- In the case where physical memory is larger than 4GB, the OS must arrange a 4GB region within memory and the programs are limited to that region. This task is completed by the segment registers (5.) under the supervision of the OS.

Fetching:
- The Process:
	- The processor retrieves instruction codes from memory based on the CS register value (5.Types.CS)  and an offset value contained in the Instruction Pointer (EIP)
- Controlling the flow of EIP:
	- Is a popular technique used in malware
  - Can allow us to alter the functioning of the program
 
## General Purpose Registers
Role:
- Used to temporarily store data as its processed in the processor (CPU)

Structure:
- Each register is 4 bytes in length
- Each of the lower 2 bytes of EAX, EBX, ECX and EDX registers can be referenced by AX, BX, CX and DX and then each subdivided by the names AH, BH, CH, DH for high byte and AL, BL, CL and DL for the low byte (each 1 byte in size)

<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216736015-ef1b10b7-5438-41a4-9ce1-5d9c9549dd4e.png"></img></div>

- Example: EAX would have AX as its 16-bit segment which can be further divided to AL for the low 8 bits and AH for the high 8 bits
- The ESI, EDI, EBP and ESP can be referenced by their 16-bit equivalent which is SI, DI, BP and SP

<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216736104-48d0c4c5-305c-4a5b-b7d3-d7bd46620f08.png"></img></div>

Types in x86:
- EAX: 
  - Known as Accumulator 
  - Used in arithmetic calculations
  - Holds results of arithmetic operations and function return values
- EBX:
	- Known as Pointer register
	- Points to data in the DS segment
	- Stores base address of the program
- ECX:
	- Known as Counter register
	- Often used to hold a value representing the number of times a process is to be repeated
	- Used for loop and string operations
- EDX:
	- General purpose register
	- Used for I/O operations
	- Extends EAX to 64-bits
- ESI:
	- Known as Source Index register
	- Pointer to data in the segment pointed to by the DS register
	- Used as offset address in string and array operations
	- Holds the address from where to read data
- EDI:
	- Known as Destination Index register
	- Pointer to data in the segment pointed to by the DS register
	- Used as offset address in string and array operations
	- Holds the implied write address of all string operations
- EBP:
	- Known as Base Pointer
	- Pointer to data on the stack (in the SS segment)
	- Points to the bottom of the current stack frame
	- Used to reference local variables
- ESP:
	- Known as Stack Pointer (in the SS segment)
	- Points to the top of the current stack frame
	- Used to reference local variables

## Segment Registers
Role:
- Used specifically for referencing memory locations
- Contain the pointer to the start of the memory-specific location
- Considered part of the operating system
- Can neither read nor be changed in almost all cases

Structure:
- Each segment register is 16-bits

Types:
- CS:
	- Code Segment register
	- Stores the base location of the code section (.text section) which is used for data access
	- Contains the pointer to the code segment in memory (where the instruction codes are stored in memory)
	- No program can explicitly load or change the CS register
	- The processor assigns its values when the program is assigned a memory space
- DS:
	- Data Segment register
	- Stores the default location for variables (.data section) which is used for data access
	- Used to point to data segments
	- Helps the program separate data elements to ensure that they do not overlap
	- Loaded by the program with the appropriate pointer value for the segments and an offset to reference individual memory locations
- ES:
	- Extra Segment register
	- Used during string operations
	- Used to point to data segments
	- Helps the program separate data elements to ensure that they do not overlap
	- Loaded by the program with the appropriate pointer value for the segments and an offset to reference individual memory locations
- SS:
	- Stack Segment register
	- Stores the base location of the stack segment
	- Used when implicitly using the Stack Pointer (ESP) or Base Pointer (EBP)
	- Contains data values passed to functions and procedures within the program
- FS:
	- Extra segment register
	- Used to point to data segments
	- Helps the program separate data elements to ensure that they do not overlap
	- Loaded by the program with the appropriate pointer value for the segments and an offset to reference individual memory locations
- GS:
	- Extra segment register
	- Used to point to data segments
	- Helps the program separate data elements to ensure that they do not overlap
	- Loaded by the program with the appropriate pointer value for the segments and an offset to reference individual memory locations
	
## Instruction Pointer Register (EIP)
Role:
- Points to the next instruction to execute
- Controlling the flow of EIP can allow to alter the functioning of a program

## Control Registers
Role:
- Determine the operating mode of the CPU and the characteristics of the current task being executed
- The values in each control register can't be directly accessed however the data in the control registers can be moved to one of the general-purpose registers and once the data is in the GP registers, the bit flags can then be examined to determine the operating status of the processor relating to the current running program
- If a change is required to a control register flag value, the change can be made to the data in the GP register and then this register can be moved back to the CR

Types:
- CR0:
	- System flag that controls the operating mode and various states of the processor
- CR1:
	- Currently not implemented
- CR2:
	- Memory page fault information
- CR3:
	- Memory page directory information
- CR4:
	- Flags that enable processor feathers and indicate feature capabilities of the processor

## Flags
Usage:
- Help control, check and verify program execution 
- Flags are a mechanism to determine whether the operations performed by the processor are successful or not

Register:
- EFLAGS:
	- 32-bit register
	- The bits are mapped to represent specific flags of information
	- Register that contains a group of status, control and system flags

Flags Types:
- Status flags
- Control flags:
	- Used to control specific behavior in the processor
- System flags:
	- Used to control OS level operations
	- Should never be modified by any program

Status flags:
- CF:
	- Known as Carry Flag
	- This flag is set when a math operation on an unsigned integer value generates a carry or borrow for the most significant digit (which is an overflow condition for the register involved in the operation) - (when this occurs the data remaining the register is not the correct answer to the math operation)
- PF:
	- Known as Parity Flag
	- Can be used to determine whether there's corrupt data as a result of a math operation in a register
	- If a "set" parity flag indicates even number of (1) bits in the last operation, a cleared parity flag indicates an odd number of (1) bits (can change based on the machine). In this case if the result of the last operation was (11010), the parity flag would be set to 0 and if the result was (11000) the parity flag would be set to 1
- AF:
	- Known as Adjust Flag
	- Used in Binary Coded Decimal math operations and is set if a carry or borrow operation occurs from bit 3 of the register used for the calculation
- ZF:
	- Known as Zero Flag
	- Is set if the result of an operation is zero
- SF:
	- Known as Sign Flag
	- Is set to the most significant bit of the result which is the sign bit
	- Indicates whether the result is positive or negative
- OF:
	- Known as Overflow Flag
	- Is used in signed integer arithmetic when a positive value is too big or a negative value is too small to be represented in the register

Control flags:
- DF:
	- Known as Direction Flag
	- Used to control the way string are handled by the processor
	- When set, string instructions automatically decrement memory addresses to get the next byte in the string
	- When cleared, string instructions automatically increment memory addresses to get the next byte in the string

System flags:
- TF:
	- Known as Trap Flag
	- Is set to enable single step mode
		- In single step mode, the processor performs only one instruction code at a time and waits for a signal to perform the next instruction
		- Essential when debugging
- IF:
	- Known as Interrupt enable Flag
	- Controls how the processor responds to signals received from external sources
- IOPL:
	- Known as I/O Privilege Level
	- Indicates the input/output privilege level of the currently running task
	- Defines access levels for the input/output address space which must be less than or equal to the access level required to access the respective address space (In case where its greater than the access level required, any request to access the address space will be denied)
- NT:
	- Known as Nested Task
	- Controls whether the currently running task is linked to the previously executed task
	- Used for chaining interrupted and called tasks
- RF:
	- Known as Resume Flag
	- Controls how the processor responds to exceptions when in debugging mode
- VM:
	- Known as Virtual-8086 Mode flag
	- Indicated that the processor is operating in virtual-8086 mode instead of protected or real mode
- AC:
	- Known as Alignment Check flag
	- Used in conjunction with the AM bit (?) in the CR0 control register to enable alignment checking of memory references
- VIF:
	- Known as Virtual Interrupt Flag
	- Replicates the IF flag when the processor is operation in virtual mode
- VIP:
	- Known as Virtual Interrupt Pending flag
	- Used when the processor is operating in virtual mode to indicate that an interrupt is pending
- ID:
	- Known as Identification flag
	- Indicates whether the processor support the CPUID (?) instruction

## Stack
Definition:
- A contiguous section of memory set aside for a program being executed
- The stack bottom is the largest valid address of the stack
- The stack grows downward in memory (between the largest address: the stack bottom and the smallest address: ESP)
Stack Pointer (ESP):
- A register that contains the top of the stack
- Contains the smallest address of the stack

<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216737115-cb473a75-222a-4af9-8c52-e9af1282dc7e.png"></img></div>

Stack limit:
- Smallest valid address of the stack
- If the stack pointer gets smaller that the stack limit, there's a stack overflow which can corrupt a program to allow an attacker to take control of a system

Operations on stack:
- Push:
	- Push a register or more = setting the stack pointer to a smaller value
	- Done by subtracting four times the number of registers to be pushed into the stack and copying the registers to the stack (?)
- Pop:
	- Pop one register or more = copying the data from the stack to the registers then add a value to the stack pointer
	- Done by adding four times the number of registers to be popped on the stack (?)

How it works:
- For each function call, there is a section of the stack reserved for the function called stack frame  (or activation record)
- When we execute the following program

<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216737299-56c393aa-e662-47eb-ab05-fc122b2a3431.png"></img></div>

- This is what happens in the stack (no stack frame for unreachableFunction bcz it was not called in main()
	
<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216737325-1c6c5143-e892-4e08-b289-ed1151647ba6.png"></img></div>

- The stack frame exists whenever a function has started but is yet to complete
- Example:
	- Let's consider this program
		
<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216737390-950fe9f2-f5fa-4174-bd86-b4fcde5d8a52.png"></img></div>

  - When it starts executing, a stack frame is created for main(). main() then calls the function int addMe(int a, int b). The stack frame for main() increases to include the arguments that will be passed to addMe(int a, int b) and the return value of this function.
		
<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216737425-831836c0-212b-430b-a98a-07fbf6f16980.png"></img></div>		

  - Once the execution of int addMe(int a, int b) is ready, a stack frame is created for it (as it may need local variables, so it needs to push some space on the stack)
		
<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216737432-af13c7be-bb46-44e1-ac70-53d92f1e17da.png"></img></div>

  - Now int addMe(int a, int b) can access the arguments sent by main() because main() placed these arguments just as int addMe(int a, int b) expects it
  - To do that, the frame pointer (FP) is used. It points to the location where the stack pointer (SP) was before int addMe(int a, int b) was called (which moved SP to execute int addMe(int a, int b)). We can use the frame pointer to compute the locations in memory for both arguments as well as local variables. Since FP doesn't move the computations for those locations is some fixed offset from it.
  - Once it's time to exit int addMe(int a, int b), the stack pointer is set to where the frame pointer is which pops off the int addMe(int a, int b) stack frame.

## Heap
Definition:
- Region of the computer's memory that a program process can use to store data in some variable amount that won't be known until the program is running
- This area is not managed automatically (like the stack)
- Unlike the stack, the heap grows upwards (see diagram below)

Usage:
- To allocate memory on the heap, you must use functions malloc() or calloc() in C. Once allocated, it's our job as well to free (de-allocate) it with function free() in C (if not done, we would have what's called a memory leak which is memory in the heap set aside and not used)
	
<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216737639-b4480c20-3b50-42c5-8777-a770818de250.png"></img></div>
		
- Unlike the stack, the heap does not have size restrictions (other than the physical limitations of the computer)

Notes:
- Heap memory is slightly slower to be read from and written to
- Unlike the stack, variables created on the heap are accessible by any function anywhere in the program
- Heap variables are essentially global in scope

---
> Diagrams are courtesy of https://github.com/mytechnotalent
