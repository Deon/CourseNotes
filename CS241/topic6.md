# Loaders

## What's Loading?
- Assembly Language Program -> Machine Language Program (assembler)

But how do we actually run the program?
- A program that copies the program from secondary storage (HDD/SSD) to primary storage (RAM) and execute (**loader**)

### Version 1.0
```
repeat:
        P <- next program to run
load:   copy P to memory starting at 0x00
        jalr $0
        beq $0, $0, repeat
```
**Problem:** Programs are not often loaded into memory at 0x00 

### Version 2.0
```
repeat:
        P <- next program to run
load:   $3 <- loader(P)
        jalr $3
        beq $0, $0, repeat
```

The loader takes a program P and...
- Finds a location in RAM for P (at starting address a)
- Copies P into RAM starting at a
- returns a - the start address
- Executes

## Relocation
If a program gets shifted around in memory (starting address a in reality; starting address 0 in the code), it affects the value of certain labels.

Initially... the label `ft` referred to `0x10`, but when the code gets reallocated, it should refer to `a + 0x10`. 

### Which locations get changed?
- When `.word` refers to a location, you must add a to it.
- When `.word` refers to a constant, do nothing.
- For `beq`, `bne`, do nothing. They jump forwards or backwards `i` instructions, not to an address.
- All other instructions, do nothing.

#### Finding those Values
Problem: Machine code is a lot of bits.
Question: How do we know which words are constants, and which are addresses?
Answer: We don't know.
Approach: We must augment the machine code with info about which words need adjusting if the code is allocated - **object code**

### MERL
MERL is the format for programs in machine language that contain information about what words need to be adjusted if the program is located into a location other than `0x00`.

- MERL = MIPS Executable Relocatable Linkable file
- Linux uses `ELF`; there are tools like `readelf` that understand this format.
- MERL has 3 parts - a header, the MIPS machine code, and the reallocation info

#### MERL Header
Consists of 3 words - 12 bytes. 

1. Cookie
    - `0x10000002`
    - Identifies the type of file
    - Can be interpreted as the MIPS instruction `beq $0, $0, 2` which would skip the header
2. FileLength - the length of the .merl file in bytes
3. CodeLength - the length of the header + the MIPS machine code

#### The MIPS Program
- The program in MIPS machine language
- It works correctly if the program is loaded into RAM `0x0C` - the location following the header

#### Relocation and External Symbol Table
- The relocation information
- The word 0x01 followed by the location of a word the .merl file that needs to be adjusted if the file is reallocated
- It also contains the external symbol definition and reference. 
```
endcode:                    ;3 –Relocation Table
.word 0x1       0x00000001  ; format code
.word reloc1    0x00000018  ; relocation address (of the code)
.word 0x1       0x00000001  ; format code
.word reloc2    0x00000028  ; relocation address
```

#### Pseudocode
```
read MERL header
a = findFreeRAM(codeLength)

for each instructon
    MEM[a + i] = instruction
for each relocation entry
    MEM[a + location] += a
$29 <- a
jalr $29
```

#### A MERL Assembler
Adapting our assembler to create a MERL assembler.

Pass 1
- Record the size of the file
- When encountering a `.word <label>`, record the location

Pass 2
- Output header
- Then MIPS machine code
- Then relocation table


#### Notes
Notice how `mips.twoints` works
`java mips.twoints <filename> [load_address]`

Description of MERL is available on the CS241 site y