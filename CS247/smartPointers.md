# Smart Pointers

Like pointers... but smart!

Basically, stack-based wrappers around pointers. Constructor allocates the heap-based pointer, destructor deallocates. Thus, if exceptions are thrown, the memory is deallocated by the destructor.

`*` and `->` are overridden so they can be used like pointers.

Syntax:
```cpp
unique_ptr(Class) myClass(new MyClass());
```

`#include <memory>` to use them

Three main types:

`unique_ptr` - Should be only one pointer to what this refers to. Cannot be copied, but can be moved. 

`shared_ptr` - Can have multiple references to it. Keeps track of the # of references, deallocates when that is 0.

`weak_ptr` - Shares ownership of a `shared_ptr`, but doesn't count towards the reference count (ie. if the only reference to something is a `weak_ptr`, it gets deallocated). If it's dangling, the object is destroyed (`weak_ptr::expired`). To get the info, use `auto shared = weak.lock()`. 


## Resource Acquisition is Initialization (RAII) Idiom

Basically, allocate resources in a constructor. Deallocate in a destructor. 