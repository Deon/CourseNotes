# Finite Automata
- Parts of a finite Automata
- Deterministic Finite Automata (DFA)
- Non-Deterministic Finite Automata (NFA)

## DFA 
- Start state - curvy arrow in.
- Transitions to several states
- One or many places to end.
- Final State(s) - Double Circle. 
- Kinda like a FSM
- Finite set of states, input symbols, and transitions.
- Can determine if input is accepted or rejected. 
- Deterministic - all transitions are labelled. 
- There is no error state - if there is no transition, ERROR. 

## NFA
- May have transitions on the same input to different states (not determined)
- Can include an epsilon transition (move to new state w/o reading input)
- Easier to design than DFA but more complex to evaluate input
- Can always create a DFA from a NFA (Algorithms exist to handle the conversion)
- You can be in multiple states at once. 
- Usually have fewer states than a DFA (easier to implement) and are slower. 

## Usages
### DFAs
- Lexers/Scanners/Tranducers
- Processors are usually highly complex DFAs

#### Transducers
- For each transition, provide the ability to output a character.

##epsilon-Non-Dterministic Finite Automata (e-NFA)
### e-Transitions
- Allow transitions from one state to another without input
- Makes it easy to join different FAs together
- Easy to convert an eNFA to an NFA (shift stuff left)

