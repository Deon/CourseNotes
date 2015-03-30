Basic ADTs
===================
Heap - dynamically allocated objects
Stack - local variables and parameters to functions, stored in stack frames.

## Structs
``` C++
struct Coord {
    int x, y;
}

Coord c1; //Static allocation
Coord* c2 = new Coord; // Dynamic

c1.x; // . because static.
c2->x; // As c2 is a pointer.
```

Pointers to stuff on the stack is legal, but it's weird.
Use `free` or `delete` to deallocate memory. 


Generally, classes and structs are pretty similar, except struct members are default public, class members are default private. 

## The stack
A **LIFO** data structure.
Stuff we need:
```C++
initStack(Stack &s);
bool isEmpty(Stack &s);
push(Stack &s, string element);
pop(Stack &s);
string peek(Stack &s);
```
## The Queue
A **FIFO** data structure. 
Stuff we need:
```C++
initQueue(Queue &q);
bool isEmpty(Queue &q);
enter (Queue &q, string element);
leave (Queue &q);
first (Queue &q);
```