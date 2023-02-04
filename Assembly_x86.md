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

---
> Diagrams courtesy of https://github.com/mytechnotalent
