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
* Fields:
    * `size` (memory being used by objects/variables)
    * `capacity` (allocated memory)
    * `Type* store` (pointer to the first element of the dynamic array)

### deque
* Fast random-access, but elements are not always stored contiguously.
* [] and .at() are overloaded.
* Insert is O(1), O(N/K) pointer copies when full (N elements, object array size K)

* Can be implemented as a circular buffer (uses modular arithmetic)
    * Of objects, this causes dereferencing (of pointers) when it has to be expanded - like a vector.
    * Of pointers to dynamic arrays - this doesn't dereference.

### list
* Doubly linked list
* Access is through iterators
    * No random access

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
    * Stack: Vector, deque*, list
    * Queue: vector, deque*, list
    * Priority Queue: vector*, deque
    
(*) = default implementation (you can choose the others in the constructor call)
## Associative Containers
* These are ordered - for speed.
* Implemented using a red-black tree (special BST, operations O(log n))
* Unordered versions use hash tables which are O(1)

```C++
map<keyType, valueType> m; // keyType must support "<" 
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
* **Iterators** are a design pattern for OOP, allowing us to navigate/walk through a container
* You get:
    * A pointer to the first element
    * A pointer to **just past** the last element
    * A way of advancing to the next element (`operator++`)
        * A way of reversing ot the previous element (`operator--`) in bidirectional iterators
    * When dereferencing the pointer/iterator, you'll get the element.
* Are backwards compatable with C-style arrays and pointers
### Functions with Iterator Parameters
```C++
f (iterator1, iterator2, ...){
    // iterator1 and iterator2 are pointers
    p = iterator1;
    p++; // Advances to the next element.
    (*p); // current element.
    // Stop when p== iterator2, without accessing p at that point!
}
```
### Using STL Container Iterators
* STL containers provide iterators. If `c` is a STL container, then:
* `c.begin()` - A pointer to the first element.
* `c.end()` - A pointer to one past the last element.
* `operator++` - Defined such that it works for iterators.

Using STL Iterators:
```C++
vector<string>::const_iterator vi = v.begin();
map<int, string>::iterator mi = mymap.begin();
list<Figure*>::reverse_iterator li = scene.rbegin(); // Note reverse iterators use rbegin() and rend(), but still operator++ to go from back->front. 
```

### Types of Iterators
* `iterators` - Take you through data using `++`
* `const_iterator` - Like iterators, but you can't change the data. 
* Bidirectional Iterators - Can go backwards as well (using the decrement operator)
* Random Access Iterators - Allows you to access random elements. (eg. vi[3] is 3 after vi).
    * Not all containers support random access - data may not be continguous.  
* Can be used with generics/templates. 

* vector, deque provide forwards/backwards bi-directional and random-access iterators
* list, multi[set], multi[map] provide forward/backwards bi-directional iterators

```C++
vector<string v>;
// Add stuff to v

// Const iterator
for (vector<string>::const_iterator vi = v.begin(); vi != v.end(); vi++){
    cout << (*vi) << endl;
}

//Reverse
for (vector<string>::reverse_iterator rit = v.rbegin(); rit != v.rend(); rit++){
    cout << (*rit) << endl;
}

// Random Access
for (vector<string>::iterator rai = v.begin(); rai ~= v.end(); rai++, rai++){
    cout << (*rai) << (*rai[1]) << endl;
}

```

### Iterator Methods
* All STL containers support iterator-based insert/erase.
```C++
v.insert(iterator1, iterator2, iterator3); // Insert into container v at iterator1 external elements which lie between iterator2 and iterator 3, but not what iterator3's pointing to.

v.erase(iterator1, iterator2); // Erases all elements between iterator1 and iterator2 but not what iterator2's pointing to. 
```

### Algorithms
```C++
#include <algorithm>

f(iterator1, iterator2, argument1, argument2, ...); // Typical call to a STL algorithm
find(m.begin(), m.end(), val); //Finding val in list m. 
```
* Eg. sort, random_shuffle, find, count, for_each, next_permutation. 
* There are some differences between STL algorithms and container methods - usually the methods are optimized for that container
    * STL find() is O(N). set::find() is O(log N)

## Generics and Templates
* A way to generalize functions for any type 
* **Templates** can be used to implement **generic** functions and classes.
```C++
// This T must support operator=
template <typename T> //Needed above every function.
void swap (T& a, T& b){
    T temp = a;
    a = b;
    b = temp;
}
```

* `a` and `b` have to be of the same type, and type T has to support the operations done inside the function (`operator=` in `swap`).
* Declarations get checked during "template instantiation" (compile-time)

### Generic Classes
* We can also use templates to create generic classes.

Partial example of a stack:
```C++
template <typename T>
struct Node{
    T val;
    Node* next;
};

template <typename T>
class Stack {
public :
    Stack ();
    virtual ~Stack();
    void push (T val);
    void pop ();
    T top ();
    bool isEmpty();
private :
    Node<T>* first;
}; 
template <typename T>
Stack<T>::Stack() : first(NULL){}

template <typename T>
T Stack<T>::top(){
    assert(!isEmpty());
    return first->val;
}
```
* Note that each method requires `Stack<T>::methodName()` and `template <typename T>`

Using this:

```C++
int main (void){
    Stack <string> stack1;
    Stack <int> *stack2 = new Stack <int>;
}
```
