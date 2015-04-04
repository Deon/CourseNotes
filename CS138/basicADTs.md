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

## Linked-List Basis
Linked-list based Queues and Stacks can use:
```C++
struct Node{
    string val;
    Node* next;
};
```

Common methods:
```C++
void initList (Node* head){
    head = NULL;
}

bool isEmpty (Node* head){
    return NULL == head;
}

bool lookup (Node* head, string val){
    assert (!isEmpty(head));
    Node* curr = head;
    
    while (curr != NULL && curr->val != val){
        curr = curr->next;
    }
    
    if (curr == NULL){
        return false;
    }
    return true;
}

void insert (Node* head, string val){
    //Setup the new node.
    Node* newNode = new Node;
    newNode->val = val;
    newNode->next = NULL;
    
    // Go to the end of the list and assign.
    Node* curr = head;
    while (curr->next != NULL){
        curr = curr->next;
    }
    curr->next = newNode;
}

void remove (Node* head, string val){
    assert(lookup(head, val));
    
    if (val == head->val){
        Node* temp = head;
        head = head->next;
        delete temp;
        return;
    }
    
    Node* curr = head;
    
    while(curr->next != NULL && curr->next->val != val){
        curr = curr->next;
    }
    if (curr->next == NULL){
        return;
    }
    Node* temp = curr->next;
    curr->next = curr->next->next; // or temp->next
    delete temp;
}
```
## The stack
A **LIFO** data structure.
Stuff we need:
```C++
typedef Node* Stack;
void initStack(Stack &s); // similar to initList
bool isEmpty(Stack &s); // same as before

void push(Stack &s, string element){
    Node* newNode = new Node;
    newNode->val = element;
    newNode->next = s;
    s = newNode;
}

void pop(Stack &s){
    assert(!(isEmpty(s)));
    Node* temp = s;
    s = s->next;
    delete temp;
}

string peek(Stack &s){
    assert(!isEmpty(s));
    return s->val;
}
```

## The Queue
A **FIFO** data structure. 
Stuff we need:
```C++
struct Queue{
    Node* first;
    Node* last;
};

void initQueue(Queue &q){
    q.first = NULL;
    q.last = NULL;
}

bool isEmpty(Queue &q){
    return NULL == q.first;
}

enter (Queue &q, string element){
    Node* newNode;
    newNode->val = element;
    newNode->next = NULL;
    
    if (isEmpty(q)){
        q.first = q.last = newNode;
    }
    else{
        q.last->next = newNode;
        q.last = newNode;
    }
}

void leave (Queue &q){
    assert(!isEmpty(q));
    
    Node* temp = q.first;
    q.first = q.first->next;
    
    if (isEmpty(q)){
        q.last = NULL;
    }
    delete temp;
}

string first (Queue &q){
    assert(!isEmpty(q));
    return q.first->val;
}
```