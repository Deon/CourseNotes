# Modules

Stick class declarations in `.h` files, and definitions in `.cpp` files to organize things. 

`#include` components as necessary (`.cpp` files should include their respective `.h` file, which should include other `.h` files it depends on).

The preprocessor replaces `#include ...` with the contents of that header.

## Header Guard
Prevents issues with duplicate header inclusion.

```cpp
#ifndef MODULE_H
#define MODULE_H

// ...
#endif
```

## Namespaces

Don't use `using namespace X` in header files, or before `#include` tags.

We use these to package related classes / functions / types together, in the same namescope. (ie. `Scope::function`).

We can also use anonymous / unnamed spaces so functions / types within cannot be referenced out of the file. 

Anything declared in global scope belongs to the global namespace, and can be referenced by `::member`.
### Referencing Namesapce Members

Something like `using std::cout` allows for `cout` but not `endl`. 

`using namespace std;` allows for `cout` and `endl`.

## Make
Projects get big, we can use `make` and `Makefile` to make our lives easier.

```make
# For Example
program : main.o ADT1.o ADT2.o              # dependencies
    g++ main.o ADT1.o ADT2.o -o program     # build rule
```

We can also use macros.

```make
CXX = g++
program : main.o ADT1.o ADT2.o              # dependencies
    ${CXX} main.o ADT1.o ADT2.o -o program     # build rule

```

It also knows implicitly how to build `.o` files.

```make
main.o : main.cpp ADT1.h ADT2.h
```

Or, automatically derive it.

```make
CXX = g++ # variables and initialization
CXXFLAGS = -g -Wall -MMD # builds dependency lists in .d files
OBJECTS = main.o stack.o node.o
DEPENDS = ${OBJECTS:.o=.d} # substitute ".o" with ".d"
EXEC = program

${EXEC} : ${OBJECTS}
    ${CXX} ${CXXFLAGS} ${OBJECTS} -o ${EXEC}

-include ${DEPENDS} # derives dependencies from .d files
```