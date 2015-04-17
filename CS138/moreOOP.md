More on OOP
=================
## Inheritance
* **abstract** classes (Abstract Base Classes)
    * Define the instance variables / methods that are common to concrete classes
    * We don't create instances of these.
* **concrete** (child) classes
    * Inherit from ABCs (parent)
    * We can override method definitions
    * These **extend** functioanlity of the ABCs.
    * Sometimes the ABCs provide a "shell" of a method, each concrete class must define their own version (pure virtual)
        * Pure virtual: `virtual method() = 0;` as declared in the class declaration

**Basic Summary**
* Instance Variables & Methods are inherited
* Pure Virtual methods must be overridden
* Child classes can define their own methods and instance variables
* Child constructor **should** call the parent constructor in an initializer
* Child destructor automatically calls parent destructor

```C++
class Child : public Parent{
    // Define constructors, destructors, methods, and fields.
};
```

### Virtual or not?

* Virtual
    * Probably going to be overridden
    * Safer to use this for now
    * A bit slower than non-virtual, but that's okay.
    * You should use this for destructors
    
* non-virtual
    * I do not expect this to be overridden
    * More efficient
    * If overridden, we might have some funky logic errors.
    
### Constructors and Inheritance
* ABCs usually have protected constructors that are called by its children
* Call these constructors using the initializer (do this first, before initializing other stuff)
* If you don't call the parent constuctors, a call to the constructor with no arguments is made.
* You **cannot** initialize subparts, so use a parent constructor. 

### Design
* Avoid repeating yourself.
* Common parts get moved up to the ABC.
* **Template Method**
    * Parent has a high-level recipe that's the same for all children
    * Some sub-parts will vary by children
    * Parent methods are usually public, sub-parts private and abstract in the parent.
    * Children implement the sub-parts, but they can't use it (private).
* If Parent defines fields, it's best if Parent works with that data. If Child is a child and uses that data, have Child instantiate it using an initializer. 

## Polymorphism
* Treating objects of similar kind in the same way.
    * Polymorphic methods should have different behaviour, not just return different info (`area()` vs `getKind()`).
* Static type: Type it was declared to be.
* Dynamic type: Type it's pointing to
    * Dynamic types **must** be children of / the same as the static type.
* You can only use methods of the **static** type.

* `object` cannot call Child methods, even though its dynamic type is Child. 
```C++
Parent *object = new Child; // Parent is the static type, Child is the dynamic type.
Parent *object2 = new Parent; // Parent is the static and dynamic type.
```

* If you're using a vector with polymorphic objects, it has to be a vector of pointers.
    * Say Circle and Rectangle inherit from Figure:
```C++
vector <Figure*> myFigures;
Circle *circle1 = new Circle ("red");
Rectangle *rectangle1 = new Rectangle;
myFigures.push_back(circle1);
myFigures.push_back(rectangle1);

myFigures.back()->draw(); // "I am a clear rectangle!"
```

### So... How does this work?
* Virtual Function Tables (V-Tables)
    * Each **class** has their own, not object.
* When an instance method is called, a `this` pointer to the object is made in the stack frame.
* Objects point to v-tables using a `vptr`
* V-tables point to the static, executable code
* Runtime follows the `vptr` to the class definition / v-table to find the right method to call. If it's not found, it looks up the inheritance chain to find the most "recently" defined implementation. 

### Static/Dynamic Stuff.
* Static vs Dynamic dispatch
* Say you have this, and Child overrides the `speak` method of Parent
```C++
Parent *p = new Child;
```
If you call `p->speak`, then if `speak()` is defined as `virtual`, then the Child's implemtnation is called. If it's not declared as `virtual`, then the Parent's implementation is hard-coded in at compile-time and that is called. 

