# Intro

**Accessors** - Gets info about a class member

**Mutators** - Set a class member (and check that the value is acceptable per ADT rules)

`const` - Use this keyword to specify params/functions that don't change.

**Function overloading** - Doable IFF the **params** are different.

`explicit` - The compiler implicitly type converts 1 argument constructors (or only 1 non-default param). You can prevent this by using `explicit`.

`final` - For `virtual` functions, prevents it from being overridden in derived classes. For classes, prevents it from being used as a base class.

`override` - Shows that a method overrides a virtual function.

### Default Arguments

```cpp
class MyObj {
    MyObj(int a = 0, int b = 2); 
}
```

A few rules:

* This can only be done in the function **declaration** (usually in `.h` files)
* Only with trailing parameters. This is invalid: `MyObj(int a = 0, int b);`
* Once you start using default arguments in a call, you must for the remainder

### Non-Member Functions

Usually `operator` overloads. They're defined outside the class, and often included as `friend` methods (ie. they have access to protected/private members)




