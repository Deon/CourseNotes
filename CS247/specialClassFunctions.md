# Special Class Functions

Special functions are declared by the compiler if we don't.

Our special functions are:

- Default constructor
- Destructor
- Copy constructor
- assignment (`operator==`)
- Move constructor (`std::move`)
- Move assignment (`std::move`)

Here's what the compiler does by default:

## Default Constructor 
Defined if we don't define any constructors.

* Simple data members: uninitialized
* Pointers: uninitialized
* Member objects: initialized via their constructor
* Inherited members: initialized via base class constructor

## Destructor

* Simple data members: deallocated
* Pointers: pointer deallocated (not the same as `delete`)
* Member objects: deallocated via member destructor
* Inherited members: deallocated via base class destructor

## Copy Constructor

* Simple data members: bitwise copy
* Pointers: bitwise copy
* Member objects: copy via object copy constructor
* Inherited members: copy via base class copy constructor

## Assignment Operator
Very similar to the copy constructor, but the destination exists

* Simple data members: bitwise copy
* Pointers: bitwise copy
* Member objects: member assignment operator
* Inherited members: base class assignment operator

## Move Constructor
This and move assignment require usage of `std::move` as a `rvalue`. 

The existing object is removed. 

Example declaration:
```cpp
ADT& ADT::ADT(ADT&& a) {
    // stuff
}
```

Example usage:
```cpp
ADT a;
// stuff
ADT b = std::move(a); // This is construction / initialization
```


* Simple data members: bitwise copy
* Pointers: bitwise copy
* Member objects: member move constructor
* Inherited members: base class move constructor

## Move Assignment 
Example declaration:
```cpp
ADT& ADT::operator= (ADT&& a) {
    // stuff
}
```
Example usage:
```cpp
ADT a;
// stuff
ADT b;
b = std::move(a); // This is assignment, as b already exists 
```


* Simple data members: bitwise copy
* Pointers: bitwise copy
* Member objects: member move assignment
* Inherited members: base class move assignment

## Copies (Shallow/Deep)

### Shallow Copy
Copies all members, and the **addresses** of pointers - both objects point to the same object in memory.

### Deep Copy
Copies all members, and what the pointers **point to**. Objects refer to different objects. 

## Equality (Shallow/Deep)

### Shallow Equality
Are stack members equal, and **addresses** of pointers equal (ie. same object/reference in memory)?

### Deep Equality
Are stack members equal, and the values of the pointers equal?