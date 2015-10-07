# Implementing an Assembler

### Key Ideas
- Pass 1 Psuedocode
- Dealing with labels (Symbol Table)
- Pass 2 Pseudocode
- Creating instructions

## General Strategy
- Test **everything** on the reference sheets
    + Eg. Ints are in the proper range
- Know the language well 
- Just throw errors, don't need details
- Don't think of every case - be very specific with what you **expect**, everything else is an error.

### Input Format
- label + instruction + comment
- each are optional - there may be 0-3 of these. 
- If a line does not have an instruction, they are **null**

```assembly
main:   lis $1  ; $1 = 1
        .word 1
```

## Pass 1 - Analysis
```pseudocode
PC = 0  // Program Counter
for each line of input
    scan line   // Provided

    for each LABEL
        if in symbol table
            report ERROR, exit
        add (label, PC) pair to symbol table

    if next token is an OPCODE
        if remaining tokens are not as expected
            report ERROR, exit
        create intermediate representation of instruction
        PC += 4
```

### Resolving Labels
- Labels can only be defined once
- They can be used many times as an operand
- We need to be able to add and lookup (string, number) pairs

### Implementing a Symbol Map
- Can use a map

Use
```c++
if (st.find('beep') != st.end()){
    // Not found
}
```

## Pass 2 - Synthesis
```pseudocode

```

### Building an Instruction
Each instruction is 32 bits, not 32 ascii characters.

We have to encode instructions into 4 bytes, outputted as a binary string.

Let's look at this: `bne $2, $0, loop`
- Lookup `top` in the symbol table
- It has address `0x0C`
- We need # of lines to jump, not an address
    + (`0x0C` - PC) / 4 = -5
- `bne $2, $0, -5`
- Now turn that into a 16-bit 2s complement
- Mask it so we get the lower 16 bits
- Do a bunch of bitwise ops to move things into place
    + Opcode in top 6 MSB
    + First register in next 5 MSB
    + Second register in next 5 MSB
    + and the Intermediate in the last 16 bits
- Can't output like this, it'll show as an int
- Need to split into 4 bytes and output as char. 
    + Don't add new lines
