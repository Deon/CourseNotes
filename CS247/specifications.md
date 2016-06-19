# Specifications

We break this down into roughly 3 types.

## Interface Specification
A 'contract' for our ADT - the preconditions and postconditions.

Includes the specification fields, which are the class data members and their descriptions, and defines the following attributes for functions:

* requires: preconditions
* modifies: class members that are changed
* ensures: what has changed after the call
* returns: return value
* throws: Things that are thrown, and why

the info listed in requires should never be listed as a reason in throws.


## Representation Invariant
The conditions that always hold true about our ADT.

## Abstraction Function
Something to give a 'real-life' abstraction of our classes. 