# Finite Automata

##Key Ideas
- Scanners
- Regular Expressions
- Finite Automata

##Overview
- How can we turn High level languages to MIPS assembly
- How does C++ or Python get converted to machine language?

##Classical Tool Chain
- Compiler turns HLL into an Assembly program (use `-S` in g++)
- Assembler translates assembly code into machine language

## The Compiler
- Source language to a target language (translator)
- Typically HLL to LLL
    + Complex language to a simple one
- Typically followed by an assembler to generate machine code
- Also try to identify errors in the program and tell the user. 

Basic Steps:
- Scanning - creating a token sequence
- Syntax Analysis - Create a parse tree
- Semantic Analysis - Symbol table + Type Checking
- Code Generation - Analysis/Optimization
#### Chomsky Hierarch
- Finite Languages
- Regular Languages (recgonize with finite amounts of memory)
- Contex-Free Languages (recgonize with 1 stack)
- Context-Sensitive Language (recgonize with 2 stacks)

**Stack** - Dealing with functions and ifs, which are usually 'pushed' on a stack for nested calls to match brackets. 

**Compilation Steps**
1. Regular Languages - Lexical Analysis - Find each lexical element.
2. Contex-Free Languages - Check syntax.
3. Context-Sensitive Languages - Check semantics.
4. Code generation.

### The Scanner
What's a scanner?
- Answers: What are the keywords, names, constants, etc in the code?

#### Tokens
- AKA Lexical Analysis
- Convert code into a stream of (token types, token value) pairs
Ex.
- Keywords
- Operators
- Constants
- Delimiters
- Names

Input
`def columnSums (a_matrix, n_rows, n_cols):`
Output
- DEF: def
- name: columnSums
- delimiter: (
- name: a_matrix
- delimiter: , 
- etc.

#### Scanning
- Keywords
    + Easy to recgonize
    + There's a fixed number of them
    + No ambiguity. 
- Delimiter and Operator
    + Easy to recgonize
    + Fixed number of them
    + Some ambiguity - [] are used as delimiters or sequences

Eg. 
- List: `children = ['bob', 'mary']`
- Sequence: `favourite = children[1]`
- Need some context to tell the difference. 

- Constants and Names
    + Harder to tell - variable length.
    + Need pattern matching
    + When does the token end and the next begin
    + the first 3 characters are 230 <- is this an int or a float. 
    + Infinite number of names and constants. 
    + *Challenge 1* - How to specify all the elements in the infinite set of valid tokens for WLP4 - CS241's Waterloo Language + Pointers + Procedures
**Scanning Background**

*Challenge 2* - Clearly and unambigiously recgonize all the tokens in a computer language (WLP4). 

**Complications:**
- Names and constants have variable length
- Some tokens (eg. `(`) meam different things in different context.
- Many types of names - function names, arguments, global/local variables
**Approach**
- Use Regular Expressions (Language) to describe our language
- Then use a lexer generator
    + Converts our description into a program for recgonizing tokens
    + eg. lex, flex, ANTLR
- Lexers are an example of programs called **finite automata** 
- What is RegEx?
    + A precise way of describing a set of strings.

**Goal: Give a precise specification of a language**
- Describe/specify a language (eg. C++)
- In a way that it is possible to tell if some input meets the spec, in an automated way.

We need this formal method because...
- A good way of communication
- To determine the expressive power/limitations of the language
- To guide how to make software

### Regular Expressions
- Define a set of strings over a finite alphabet (usually ASCII)

#### Constants
- We now have the empty string (epsilon), with no characters.
- Literal character a
    + All the individual characters in the alphabet
    + The alphabet is finite, but the language can be infinite

#### 3 Ways of Building Languages
1. Union 
- R | S is the union of R and S.
- If T1 and T2 are RegEx, then so is T1 | T2
2. Concatenation
- RS = {ab: a in R and B in S} - Combining one word from one language and one from another language. 
- Concatenation with the empty string does nothing. (Identity element of concatenation). 
- If T1 = {dog, cat} and T2 = {fish, epsilon}, then T1T2 = {dog, cat, dogfish, catfish}
- If T1 and T2 are RegEx, then so is T1T2 
3. Repetition
- R* = smallest superset of R containing epsilon and closed under concatenation. 
- All combos of the elements in R
- if R = {a} then R* = {epsilon, a, aa, aaa, ...}
- If R = {0, 1} then R* = {epsilon, 0, 1, 00, 01, 10, ...}
- If R is a RegEx, then so is R*
- Can specify # of repetitions, eg. R^2

#### a Finite Language (Example)
- Alphabet = {a, b}
- Words are finite sequences of characters. 
    + eg. a, b, ba, abba
- Language is a set of words over some alphabet
    + eg. L= {a, b, ba, abba}
- Languages can be infinite or infinite. 
    + |L| = 4
- (h | c)at = {hat, cat}
#### Infinite Languages
- a* = {epsilon, a, aa, aaa, ...}
- {a, b}*
- b | a* = {epsilon, b, a, aa, aaa, ...}
- (0|1)*

**Unix Tools**
- egrep - Search RegEx 
- sed - Stream Editor to transform files
- awk - Pattern scanning/processing language
- make - Software Building Utility

#### RegEx in our language
- Keywords
    + int | float | for | while | return | if | else
- Operators
    + + | - | * | / | = | ...
- Names
    + [a-zA-Z_][a-zA-Z0-9_]*

#### Issues
- Would also have to specify the format of integers, floats, strings.
- Conflicting rules - need precedence
    + a | ab* = (a|(ab))* or a| (a(b*))
    + Use precendence rules
- Usually greedy (produce longest possible match)
    + If i stop at 3, I get an int.
    + If i stop at 5, I get a float (greedy).

#### Recgonizing a RegEx
- Clearly and unambigiously recgonize all the tokens in a language
- Once we've specified our language with RegEx, we need to recgonize it with Non-Deterministic Finite Automata
