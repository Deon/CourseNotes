Input/Output & Intro to C++
============
## Standard IO
* `#include <iostream>` - The iostream library
* `cout << stuff << endl` - Output to standard output, with a new line at the end. 
* `cin >> var` - Input from standard input to some variable `var`. 
* `cerr << errorMessage` - Use this for errors. 

To check if you've gone past the end of a line:

```C++
cin.eof() // Check the end of a file.
cin.file() // Check for eof and other errors.
```

Check these **after** reading input, but **before** using the variable.

Alternatively:
```C++
int input; 
while (cin >> input){
    // Stuff
}
```

Line-based input:
```C++
string temp;
while (getLine(cin, temp)){ // Sends input from cin (until next \n) to temp
    // Stuff
}
```
### File Input
`#include <fstream>` - Library for file IO

File names have to be `char*`s. 

To convert:
`myString.c_str()` 

**Using fstreams:**
```C++
ifstream in1("in1.txt"); 
ifstream in2;
in2.open("in2.txt");
string fileName = "in3.txt";
ifstream in3 (fileName.c_str());
ofstream out ("passes.txt");

// Exits if "in2.txt" doesn't exist.
if (!in2){
    cerr << "File not found!" << endl;
    return 0;
}

// Do stuff

// Close IO streams.
in1.close();
in2.close();
in3.close();
out.close();
```
You can open/close seperate files using the same input/output stream.

## A few things about C++
A c++ trick:
```C++
int *q, r;
// q is a pointer, r is not...
```

When pointers are created, they get garbage - set them to `NULL` to avoid this.

**Reference Parameters**

Kinda like pointers - any changes to the parameters inside the function modify the original, but you don't need to dereference them.

Generally good to save memory, as the compiler doesn't need to allocate more memory. 

```C++
void doSomething (int &x, int &y){
    // Do stuff.
}

void doSomethingElse(int &x, const int &y){
    // Do some other stuff, but you can't change y.
}

string val = "whatever";
string& val2 = val; // Abstraction of a pointer - you'll get what the pointer's pointing to back directly.

// Know the differences in the implementations / usability of each of these. 
swap (int x, int y);
swap (int* x, int* y);
swap (int& x, int& y);
```

## Basic File Structures 

### Arrays
We're not going to use these too often.
Size is static.
These are basically pointers.
Size **must** be declared at compile-time.

```C++
int A[15]; // Array of ints, size 15.
int i = 5;
A[i] == A + i == i + A == i[A] // Addition is commutative...
```

### Vectors
These are dynamic - a bit less efficient as a result.

Vectors can only store elements of one type.

To use them:
`#include <vector>`

Using vectors:
```C++
vector <string> monthName(12); // Declared with size 12.
vector <int> grades; // Size 0.

myVector.size(); // Returns # of elements inside myVector
myVector.capacity(); // Returns total space allocated for elements in myVector (always >= myVector.size())
myVector.resize(newSize); // Resizes the vector to newSize.

myVector [2]; // Unchecked access.
myVector.at(2); // checked access.
```

**Vector as a stack**
```C++
vector<string> myVector;
myVector.push_back(someString); // Push operation
myVector.pop_back(); // Pops the top element of myVector.
myVector.back(); //Returns the top element of myVector.
```
All functions for vectors are O(1) time, except `.push_back()` which is ACT, as sometimes the vector capacity needs to be doubled and the elements copied over. 

### Dynamic arrays.
* Stored on the heap
* Size can be a run-time value
* Must `delete` when done with it.
* Extent (size) is usually stored at index -1.
* Basis for `vector`

```C++
string* alloc (int n){
    assert(n > 0);
    return new string[n];
}

int main (){
    int N;
    cin >> N;
    int* a = new int[N];
    string* B = alloc(N);
    // do stuff
    delete [] A;
    delete [] B;
}
```