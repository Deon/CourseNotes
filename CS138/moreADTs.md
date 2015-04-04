More on ADTs
=============

## Doubly Linked List
Similar to a linked list, but also has a `prev` pointer.
```C++
struct Node{
    string val;
    Node* next;
    Node* prev;
};
```

`insert` and `remove` have changed.

### Sorted List
Essentially a linked list that is sorted (eg. alphabetically).
Remove is more efficient usually, as inserts go directly in the right spot.

## Graphs, BSTs

### Binary Trees
```C++
struct Node{
    string val;
    Node* left;
    Node* right;
};
```
* Instead of a `next` pointer, each node has a `left` and `right` pointer.
* Can be sorted - heap, BSTs.