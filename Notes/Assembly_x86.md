## Opcode:
Definition:
- Each instruction code that the CPU executes, must include an opcode
- The opcode defines the basic task to be performed by the CPU

Structure:
- Opcodes are between 1 and 3 bytes in length

Example (How it works):

<div align="center"><img src="https://user-images.githubusercontent.com/83297038/216739183-741e509f-c38d-4dc4-b8c9-67d3d8d29364.png"></img></div>		

- In memory address 0x80483de, we have the instruction mov eax, 0x0 which corresponds to the opcodes b8 00 00 00 00. b8 opcode corresponds to mov eax and 00 00 00 00 correspond to 0x0 (little-endian)
- If the instruction was mov eax,0x1 we would have opcode b8 01 00 00 00

## AT&T vs Intel syntax
Examples:
- a) 
	- AT&T: ***movl %esp, %ebp***  [move esp to ebp]
	- Intel: ***mov esp, ebp***  [move ebp to esp]
- b)
	- AT&T: comments start with # symbol
	- Intel: comments start with ; symbol (could change based on the assembler)

## Assembly Programs
Sections:
- Data Section:
	- Used for declaring initialized data and constants (that won't be changing in the future)
	- You can declare constant values, buffer sizes, file names, etc..
- BSS Section:
	- Used for declaring uninitialized data or variables
- Text Section:
	- Used for the actual code sections (instructions)
	- Begins with a global _start which tells the kernel where execution begins

<div align="center"><img src="https://user-images.githubusercontent.com/83297038/217263133-cdd218d9-8a1e-499d-842f-b52a83f0105f.png"></img></div>

Instructions:
- Data Movement Instructions
	- movl: 
		- Semantics:
			- Moves data item referred to by its first operand into the location referred to by its first 
		- Syntax:
			- movl reg, reg
			- movl reg, mem
			- movl mem, reg
			- movl const, reg
			- movl const, mem
		- Examples:
			- movl $1, %eax # move decimal value of 1 into eax 
	- push: 
		- Semantics:
			- Places its operand onto the top of the stack in memory
			- Specifically, push first decrements ESP by 4, then places its operand into the content of the 32-bit location at address [ESP] (decremented and not incremented because the stack grows downwards. See X86_Architecture.Stack)
		- Syntax:
			- push reg32
			- push mem
			- push con32
		- Examples:
			- push eax ; push the contents of eax onto the stack
			- push var ; push the 4 bytes at address “var” onto the stack
	- pop: 
		- Semantics: removes the 4-byte data element from the top of the stack into the specified operand (register or memory location)
		- Syntax: 
		- Examples:
	- leal:
		- Semantics: 
			- places the address specified by its first operand into the register specified in the second operand
			- The contents of the memory location are not loaded, only the effective address is computed and placed into the register
		- Syntax:
			- leal (ebx+4*esi), %edi

Common Instructions:
- movl $1, %eax	 # specifies sys_exit to properly terminate program
- movl $0, %ebx	 # show that the program successfully executed | returns code 0
- int $0x80	 # calls system interrupt vector 0x80 (basically to exit the program)

---
> Diagrams are courtesy of https://github.com/mytechnotalent
