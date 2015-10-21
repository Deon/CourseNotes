# Linkers

## Assemblers, Loaders, Linkers
- Assemblers
    + Need 2 passes to translate labels
    + So labels can be used before they're defined
- Loaders
    + Need to track and adjust labels used in `.word`
    + Allows a program to be loaded anywhere in RAM
- Linkers
    + Use multiple files for code
    +

## Linking
Why breakup large programs?
- Subroutines that can be reused 
- Errors are easier to find
- Different people can be responsible for different modules
- Keeping code DRY

#### How to Link - Attempt 1
Goal: Use multiple files for code.

- Combine/concat all small files into one big one and assemble.
- One change in a file means reassembling everything.
- May want to distribute the object code - not the assembly language code.
- We want to do it at the object code level.  

**Requirement 1** - We need a tool that works with multiple MERL files.

#### How to Link - Attempt 2
Assemble all the MERL files and then concat.

- When assembling, we start at `0x00` - so they all start at the same location.
- If you concat 2 MERL files, the result is not MERL.

**Requirement 2** - We need a tool that outputs MERL. 
**Requirement 3** - We need to support labels defined in one file and used in another. 

### How to Link - The External Symbol Reference (ESR)
- Create a directive, `.import` - it tells the assembler that the symbol occurs in another file.
- This isn't translated into an instruction - it tells the assembler.
- Eg. `.import notify_nsa` means that `notify_nsa` is defined elsewhere.
- When assembling... Assign 0 to the symbol for now, but note it in the MERL file.
- If it's never found, throw an error.

#### The ESR Format
- Made in the 3rd section of MERL files. 
- We use an ASCII char to represent the chars in the symbol
- The first one is `0x11` to signify that the following is an ESR
```
word 1:     0x11
word 2:     address ; Where it's used
word 3:     length  ; of the symbol in bytes (n)
word 4:     1st char in symbol in ASCII
word 5:     2nd char in symbol in ASCII
...
word n+3:   last char in symbol in ASCII
```

What if multiple files use the same symbol - it's implemented twice?

### The External Symbol Definition (ESD)
**Requirement** - A way to hide information.

We want to differentiate between symbols for local & global usage.

Use the `.export` directive - indicates that other files can use this symbol.

A symbol can only be defined once, but used several times.

#### The ESD Format
- Using `.export` is like declaring a variable globally.
- `.import .export` links definitions to usages.
- Created in the 3rd section of MERL files.
- Use entry type `0x05`
```
word 1:     0x05
word 2:     address ; Where the symbol refers to. 
word 3:     length  ; of the symbol in bytes (n)
word 4:     1st char in symbol in ASCII
word 5:     2nd char in symbol in ASCII
...
word n+3:   last char in symbol in ASCII
```

### Making a MERL Assembler
#### Pass 1
- Record the size of the file.
- When you encounter `.word <label>`, record the location.
- When you encounter `.import <symbol>`, record all symbols that need importing.
- When you encounter `.export <symbol>`, record all symbols that need exporting.
#### Pass 2
- Create an ESR entry for all imported symbols
- Create an ESD entry for all exported symbols 
- Output header
- Output MIPS Code
- Output Relocation Table

#### Linker Pseudocode
- Handle multiple files/symbols

1. Concatenate the programs
2. Combine and adjust ESDs
3. Use new ESDs to update ESRs
4. Reallocate Addresses

Addresses in files need to be adjusted for their place in the concatenated file.

**1. Concatenate Files**
- One header, then Program 1, 2, 3, Reallocation Entries, ESRs, ESDs

Adjust Addresses:
- Add2 = Add2_old + |Prog 1|
- Add3 = Add3_old + |Prog 1| + |Prog 2|

**2. Combine and Adjust ESDs**
- Combine all the ESDs
    + Program 1 ESDs have no change
    + Program 2s ESDs have to be shifted by size of Prog 1 (ESD2 = ESD2_old + |Prog 1|)
    + Program 2s ESDs have to be shifted by size of Prog 1 + Prog 2 (ESD3 = ESD3_old + |Prog 2| + |Prog 1|)
- You can get the size of each program from their old headers

**3. Use New ESDs to update Old ESRs**
```
for each old ESR
    lookup the new ESD
    if found
        update the value at the location + offset
    else
        adjust the new ESRs with the new offset
        ESR2_new = ESR2_old + |Prog 1|
        ESR3_new = ESR3_old + |Prog 1| + |Prog 2|
```

**4. Relocate Addresses**
- Any relocatable addresses in programs 2,3 need to be relocated
- For each relocation entry
    + Add the proper offset in the code
    + Add the proper offset in the relocation entry
    + Add2_new = Add2_old + |Prog 1|
    + Add3_new = Add3_old + |Prog 1| + |Prog 2

## Dynamic Linking
- That was just static linking - all files are linked before loading.
- Among commonly shared libraries, we use dynamic linking.

Shared Libraries
- Many programs use I/O or Math
- Many programs may use it at once
- Idea: Use only one copy in library
- Reserve memory for relocatable object code

Dynamic Libraries
- Do not add the object code to the executable
- Combine object code at load time

Dynamic Linking
- Relocate and resolve at load time
- A program may halt as it is missing a 'dll'

A What?
- Dynamic Link Library (DLL) in Windows
- Shared Object File (so file) in Linux
- dylibs (Mac OS)
