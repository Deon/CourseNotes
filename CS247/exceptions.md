# Exceptions

This is a way of halting the execution chain and sending errors up the call stack. Here's how it looks:

```cpp
void someMethod() {
    // stuff
    throw "An error."
}

try {
    // stuff
    someMethod();
    // stuff
}
catch (char* e) {
    // do something with e
}
```

We can also throw exception classes, eg:

```cpp

class SomeObject {
    // Stuff
    void someMethod();
    class SomeErrorException {
    private:
        std::string msg;
    public:
        SomeErrorException(std::string msg) : msg(msg) {};
        std::string msg() {return msg;}
    }
}

void SomeObject::SomeMethod() {
    // Do something
    throw SomeErrorException("An error");
}

// elsewhere
myObj = new SomeObject();
try {
    myObj.someMethod();
} catch (SomeObject::SomeErrorException e) {
    cout << e.msg() << endl;
}

```



Exception classes can also inherit from `<exception>`

When exceptions are thrown from methods, anything on the stack is automatically cleared. Objects on the heap are **not** deallocated, you must deallocate these yourselves, or use smart pointers. 

Exceptions are caught by the 'closest' handler (the one closest on the call stack). If there is none, the program aborts.

Example from slides:

```cpp
class X {
public:
    class Trouble {};
    class Small : public Trouble {};
    class Big : public Trouble {};
    void f() { throw Big(); }
};

int main() {
 X x;
try {
    x.f();
 }
catch(X::Small&) {
    cout << "Small Trouble"; throw Trouble(); }
catch(X::Trouble&) {
    cout << "Trouble"; }
catch(X::Big&) { cout << "Big Trouble"; } // This is never reached, as the above handler handles Trouble and any derived classes. 
catch(...) { cout << "catches any type of exception"; throw; } // Rethrows the exception
} 
```

## Exception Safety

Operations are exception safe if the op leaves the program in a valid state after throwing an exception.

**basic guarantee** - All invariants are maintained, nothing leaks.

**strong guarantee** - Basic guarantee + program reverts to the state it was in pre-call

**nothrow guarantee** - Basic guarantee, also will not throw an exception.


Functions can declare that they won't throw exceptions with `noexcept`. This benefits the programmer and compiler (optimizations can be made).

Eg:

```cpp
class SomeObj {
public:
    SomeObj();
    doSomething() noexcept;
}


Do not use exceptions for programming errors - it will remove elements from the call stack. Use assertions to preserve program state for debugging.