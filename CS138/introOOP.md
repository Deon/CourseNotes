Intro to OOP
============
A reminder: struct members are default **public**, class members are default **private**.
OOP focuses a lot on design - but that's saved for later classes.

**Programming Styles**

Procedural vs. Object Based vs. Object Oriented

* Procedural
    * Variables created in main and passed as parameters. 
    * We use structs and manipulate them using functions.
* Object Based Programming (Classes + instances)
    * Classes/structs have subparts and procedures (methods) that work with the subparts (fields/member variables)
    * Each object is an instance of the class/struct.
        * Objects can have subparts that are objects / pointers to objects
* Object Oriented Programming (Classes + Inheritance/Polymorphism + Generics)
    * Classes extend other classes (inheritance)
    * Some classes never have instances (abstract base classes [ABCs])
    * Treat instances of related classes uniformly (Polymorphism)
    * Parameterize some classes by type (generics)
    
    
**Access Rights**
* public - Anyone can access this stuff - don't put variables here.
* protected - instances of this class and its children can access these.
* private - only instances of this class can access these.
    * These are visible in children, they just can't be used.

```C++
class Balloon{ // Class DECLARATION (usually .h file)
    public:
        Balloon(); // Constructor, no return type.
        Balloon(string colour); // Constructor that takes a colour;
        ~Balloon(); // Destructor - notice the tilde.
        void speak() const; // Const method.
    protected:
        // Stuff here
    private:
        // Declare const member variables first.
        string colour; // Best to keep member variables private. 
}; // Like structs, don't forget this.

// Method DEFINITIONS (usually .cc file)
// Notice how methods are defined as ClassName::methodName(){}

Balloon::Balloon (){ 
    this->colour = "clear";
}

Balloon::Balloon (string colour){
    this->colour = colour;
}

// Alternatively with a initializer (preferred)
Balloon::Balloon() : colour("clear"){}
Balloon::Balloon(colour) : colour(colour){}

Balloon::~Balloon(){} // Nothing to do here!
void Balloon::speak(){
    cout << colour << endl; // or cout << this->colour << endl;
}
```
Having `this->` isn't always required - in cases where the member variable and the argument have the same name, use `this->`.

Inside methods, the compiler looks up variables in this order:

1. Is there a local variable? eg. `string colour = "blue";`
2. Is it a parameter? 
3. Is it a member variable?
4. Is it a member variable of a parent class?

Instantiating a class:
```C++
Balloon b1; // Statically 
Balloon* b2 = new Balloon("red"); // Dynamically

// Calling methods. 
b1.speak(); // clear
b2->speak(); // red
```

General OOP design says to be consistent, either use return types or reference params.
Keep constructors somewhat consistent as well. 

### Static and Const
* Declaring a method as `const` is a promise that you won't change anything using that method. It also means that you can only call other `const` methods within it.
* Declaring a variable as `const` says that after the first time it's set, it won't change.
    * `const` variables need to be declared first. 
    * Must be given its value by an initializer. 
* There only exists one instance of `static` members across all instances of that class.
    * Usually used to keep track of the number of instances.
* `static` methods can only access `static` variables. 
* `static` data is stored above the heap. 

## Constructors
* Constructors **must** have the same name as the class.
* They are called once, to create an instance of a class, and have no return type.
* If you don't define one, the compiler will make a default one (no parameters). 
* If you define 1+ constructors (overloading), the compiler **will not** define a default one for you - make it yourself. 
* STL containers give default values based on the element type... so if you want consistency, define one yourself. 
* Use initializers whenever possible. 
    * Any non-initialized parts have their default constructors called, then stuff in the constructor body is processed. 
    * Variables are initialized in their order according to the **class definition**, not the initializer. 

Example constructor with multiple parts to initialize
```C++
ClassName::ClassName(string part1, string part2): part1(part1), part2(part2){}
```

### Copy Constructors
A constructor that takes a `const` reference to an object of the same class, and creates a copy as a new object.
```C++
ClassName::ClassName(const ClassName object); // Declaration
```
One is automatically defined for you.

**Rule of Three**
If you override any of:
* Destructor
* Copy constructor
* `operator=`

you should probably override all three. 

### Destructors
* Usually there's not much to be done here - just free up dynamically allocated memory (delete).
* The parent destructor is called by default.
* Usually declared as `virtual`. 
    * Not necessary if there are no child classes, but it's best to leave it as `virtual`.
* These are called implicitly:
    * A object's scope is exited (stack frame is removed)
    * `delete` is called on the instance.
    
## Accessors and Mutators
* Generally, set member variables as private, and use accessor and mutator methods to change them / get their value. 
* Generic naming scheme:
    * Mutators: `setPartName (string newName)` 
    * Accessors: `getPartName()`
* Often, it's better to adapt the name and its functionality to the class. 
    * Eg. `giveBalloonAway()` and `acceptBalloon()`
* We can use these mutators and accessors to set restrictions, rather than letting users manipulate variables however they'd like.