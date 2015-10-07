
## Key Points
- Arrays
- Call Stack / Runtim
- Stack
- Calling/Returning from Subroutines
- Printing stuff to the screen

## Arrays
If the address of A[0] is $1 and each element is w bytes, 
then A[1] is w($1), A[2] is 2w($1), etc....
- The address of $1 is the base address.
- Looking for the nth element, each element is w bytes, $1 holds the address of A[0], add nw to $1. 

## Subroutines
### Challenges
- How do we ensure that data in registers is not lost?
    + RAM
- How do we call and return from subroutines?
    + `jalr` and `jr`
- How do we pass parameters to the subroutine?
    + Call stack.
- How do we return values from a subroutine
    + Using a register. 

### Storing Essential Data
- A subroutine can call another subroutine (or itself)
- Registers are **global**; we need to save the current execution context before calling our subroutines, and restore them upon exiting the subroutine
- We need to use a stack (RAM)
- Stacks usually grow downwards, the address of the bottom is stored in the Stack Pointer Register (SP) $29 usually - but **$30** in this course


### Passing Arguments, Returning Results
- Document everything - we'll pass parameters on the stack. 
## Memory Mapped I/O 

- I/O devices are typically treated like memory - reading and writing to memory. 
    - MIPS `lw` and `sw`, but to specific memory addresses 
- Store words to `0xFFFF000C` (video out, one character at a time).
