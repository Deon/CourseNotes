The C++ Standard Library
==================
## Containers
3 major container categories from the C++98 standard:

1. Sequence containers: vector, deque, list
2. Container Adapters: stack, queue, priority_queue
3. Ordered associative Containers: [multi]set, [multi]map.

* **deque** - double-ended queue
    * Fast operations on the front/end
* **list** - doubly linked list
    * Fast inserts in the middle
* **set** - a set in math
    * **multiset** - allows an element to occur more than once (usually red-black tree)
* **map** - like an array, but the index can be anything (associative array)
    * **multimap** - allows one key to map to multiple values (usually red-black tree)

C++11 standard adds:
1. array, forward-list
2/3. nothing new
4. Unordered associative containers: unordered_[multi]set, unordered_[multi]map - basically unordered versions of 3.
    * These use hash tables, so you get O(1) ops.

## Sequence Containers
* Elements are ordered 
* Usually differ on methods (lookups, insert/remove) and performance

### vector
* Dynamically allocated array (data is on heap)
* Lookups are fast - pointer offset
* Insertion is ACT
* When a vector is full, a new array is created and all objects copied over. (O(N))

### deque
* Fast random-access, but elements are not always stored contiguously.
* [] and .at() are overloaded.

* Can be implemented as a circular buffer
    * Of objects, this causes dereferencing (of pointers) when it has to be expanded - like a vector.
    * Of pointers to dynamic arrays - this doesn't dereference.

### list
* Doubly linked list
* Access is through iterators

### C++11 containers
```C++
array<type, size> name = {stuff, morestuff,...};
```
* Just as efficient as a C array, but with none of the drawbacks.
* Similar to a vector - supports `.at()`, `.size()`, and most other methods.
* Size is fixed, stored on the stack.
`forward_list` is basically a singly-linked list.

## Container Adapters
* Basically makes the interface smaller - eg. stack, queue, priority_queue
* You can choose which container to use in the constructor call - it's usually deque, except for PQ (vector)

## Associative Containers
* These are ordered - for speed.

```C++
map<keyType, valueType> m; // keyType must support < 
```

The compiler uses the following test for equality, even if overridden:
```C++
if (!(a < b) && !(b < a))
```

Sets:
```C++
set<T> s;
```
## Iterators, Algorithms, Operations


