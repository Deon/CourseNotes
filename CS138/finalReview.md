Final Review
==========
* Unix and Programming Languages (a few marks)
    * Not too important
* Linked structures, memory, ADTs
    * Pointers and reference parameters
    * Know the difference between * and & 
    
```C++
string val = "whatever";
string& val2 = val; // Abstraction of a pointer - you'll get what the pointer's pointing to back directly.

swap (int x, int y);
swap (int* x, int* y);
swap (int& x, int& y);
```
* Recursion on the runtime stack
    * Activation record on the stack, nothing magical.
    * 3 Parts:
        * Base case - where do I stop
        * Reduction - how are you breaking up the problem
        * Composition - compose smaller parts into full answer. 
* Basic ADTs (set 4)
    * Sequence / vector
    * Stack
    * Queue
    * Priority Queue
    * Sorted List
* What you need to know about these ADTs
    * Runtimes for all operations
    * Understanding of the implementation
    * Know when to use each ADT for different scenarios
    
**Set 5**
* Trees
    * Internal nodes
    * Leaf Nodes
    * Binary Tree
        * BST
        * Heap - Must be a *complete* binary tree (left->right insertion)
            * Best implementation for a priority queue.
* BST
    * Know the BST property (left->key < key < right->key and all children have the BST property)
        * All keys to the left < current key, all keys to the right > current key
    * Operations
        * lookup()
        * insert()
        * delete()
        * print()
    * Recursion to build clean solutions
    * Time complexity O(log n)
* Reversing a linked list
    * Can be done in place - no extra memory required. 

```C++
void reverse (Node*& first){
    Node* prev = NULL;
    while (first != NULL){
        Node* temp = first->next;
        first->next = prev;
        prev = first;
        first = temp;
    }
    first = prev;
}
```
* Dictionary ADT
    * Associative container - (Key:Value)
    * Implemented using many data structures
        * Sorted/Unsorted
            * Vector
            * Linked List
        * BST
        * HEAP
* Hash Tables
    * O(1) time.
    * Hash function has to be consistent. 
        * Cheap to compute
        * Good distribution
        * Supports a variable range. 
    * Memory is allocated once at the beginning. 
    * Collision strategies
        * Probing
        * Chaining 

**Set 6**
* IDEAS of programming - know the differences
    * Procedural 
    * Object-based
    * OBject-oriented 
* Know the required special methods
    * Constructors
        * Know initializers
        * How to call base constructors
    * Destructors
        * Needs a special implementation if you have heap-based memory
        * virtual or not - if ABC, then yes. 
    * Copy constructors
* Access specifiers
    * Public - it can be called/used everywhere
    * Protected - called/used by objects of this class, or its children
    * Private - called/used only by objects of this class.
* Keywords
    * static
    * virtual
    * instance
    * field
    * method
    * object
* Polymorphism 
    * Treat objects of similar kind in an uniform way
* Static/Dynamic pointers
    * Static is the API access
    * Dynamic is what i'm referring to.
    
```C++
Parent* ptr = new Child; // Static type Parent, dynamic type Child.
```
* Generics and templates
    * Know what they are and why they're useful
    * Know great OO design practice
```C++
template <typename T>
class...
```
* Know good OO design principles

**The STL**
1. Sequence containers
    * vector, deque, list
2. Container adapters
    * Stack, queue, priority queue
3. Associative Containers
    * unordered/ordered set/multiset
    * map/multimap

* List
    * Doubly Linked List
    * Supports bidirectional iterators
    * No random access

* Vector
    * Iterators
        * Forwards/reverse
        * Random access
    * Dynamic heap-based array
    * Special fields
        * Size
        * Capacity
        * Type* Store (points to the first element of the array)

```C++
push_back(){
    if (size == capacity){
        // copy over data
        // delete old array
    }
}
```

* Deque
    * Double ended queue
    * Insert/delete in the front and back is efficient
    * Bidirectional iterators, with random access.
    * Insert O(1), O(N/K) when full. 
        * Only pointers are copied
        * This is for object array size K
        * N total elements.
        * We make N/K copies of pointers. 
        
* Adapter Containers
    * Stack: Vector, deque*, list
    * Queue: vector, deque*, list
    * Priority Queue: vector*, deque
    
(*) = default implementation

* Associative containers
    * Set
    * Multiset
    * Map
    * Multimap
* Ordered Map/Set
    * Implemented as a special BST - Red/Black Tree (O(log n))
    * Ordering is based on key value
* Unordered map/set
    * Hash Table, O(1)
* Circular Buffers
    * Use modular arithmetic to wrap around the end back to the front.