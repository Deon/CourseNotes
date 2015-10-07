# The Assembler
### Key Points
- The Assembler
- Analysis and Synthesis
- Intermediate representation and the symbol table.
- Assemblers convert assembly language to machine code.

## Analysis
Pass 1 : Analysis

The input is a text file with a sequence of assembly language instructions.

Eg. `beq $1, $2, -3 ; $1 total cost`

Purpose: To recgonize instructions.

Breakdown each line into **tokens**:
- LABEL 
- OPCODE
- REGISTER

Pass 1: Tokenization

Pass 1: Error Checking
- We also check for syntax errors (improper form/structure)
- Semantic Errors too (eg. duplicate labels)
- For this course: just recognize proper form, call everything else an error.

Pass 1: Output
- Intermediation Representation
    + Easy to work form of the input
- Symbol Table
    + Maps labels to addresses

### Intermediate Representation
- Remove Tokens
- Create Tokens
- Keep it as ASCII or Unicode   

## Synthesis
Pass 2:

**Input:** The intermediation representation + the symbol table

**Purpose:** Translate
- The intermediate representation into machine code
- Labels into addresses

**Output:** Machine language.

#### Why 2 passes?
- A label can be used before it's defined.
- Two labels can refer to each other. 
 


