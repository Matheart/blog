!!! Notice
    This is COMP2012H Revision Note. Please it doesn't record all the knowledge taught, instead it only covers the part that I am not so clear about.

# Basics
## type
both **char** and **bool** occupie one byte, for ascii it only has 128 characters, raw strings are of `const char *` type, all floating literals is treated as double by default unless there is a suffix 'f'. A narrowing conversion will result in a warning, can use `static-cast` to make it disappear.

## const vs #define
`const` can be type-checked during compilation, and memory is not allocated for constant definition with only a few exceptions. Simply `const int x = 10` still stores x in memory, but not in O3 optimization. If we print &x at the end, then both unoptimised and O3 optimised program stores x in memory.

for `#define`, it simply substitutes value inside the program.

## Variable naming
Allowed Characters: 0-9, a-z, A-Z, _
The first character cannot be a digit
cannot use reserved word
identifiers start with '_' are always used for system variables

## Reference
```cpp
int a;
double &m = a;
```
This results in error: non-const lvalue reference to type 'double' cannot bind to a value of unrelated type 'int'.
The casting from int to double result in creating a temporary variable, and we cannot create non-const lvalue reference to a remporary variable.

++x is lvalue, x++ is rvalue

## Operators
For the modulo operator %: 
```
5 % 2 = 1
5 % -2 = 1
-5 % 2 = -1
-5 % -2 = -1
```

## Control Flow
Remember to add break / return for `switch`, default is for not catching anything, not any cases can have the same value, the expression must be evaluated to an integral value ie (integer, char, bool)

## Function
Return type is not part of the signature, if included, hard to infer which type to return.
https://chortle.ccsu.edu/java5/Notes/chap34A/ch34A_14.html

Overloading
```cpp
int f(int a, double b) {return 1989;}
int f(double a, int b) {return 2022;}
int main() {
    cout << f(89.64,19.89);

    return 0;
}
```
will pop out an error: call of overloaded 'f(double, double)' is ambiguous

## Array
The array elements cannot be reference variables, because references are not objects and doesn't occupy the memory so doesn't have the address. 

Reference to an array, `int *b = a` would make it lose information about the underlying array (eg: iterators), should better use `int (&b)[4] = a`, for functions: `int (&func())[5]`
```cpp
// int *b = a;
int (&b)[4] = a;
for (int v: a) cout << v << endl;
cout << "=========\n";
for (int v: b) cout << v << endl;
```

```cpp
int a[10];

int main() {
    cout << typeid(a).name() << " " << typeid(&a).name() << " " << typeid(&a[0]).name() << endl;
    cout << a << "  " << &a << " " << &a[0] << endl;
}
// A10_i PA10_i Pi
// 0x558fe4de8160  0x558fe4de8160 0x558fe4de8160
```

char [] vs char *
```cpp
#include <iostream>
using namespace std;

int main() {
    char s[6] = "hello";
    cout << reinterpret_cast<void *>(s) << " " << &s << endl;
    const char *ss = "hello";
    cout << reinterpret_cast<const void *>(ss) << " " << &ss << endl;
}
// 0x7ffcad02ac0a 0x7ffcad02ac0a
// 0x402006 0x7ffcad02ac00
```
ss is a pointer
the address of a pointer is not equal to the address it's holding.
s is an array, and &arr == arr, but not the same for pointers

## Recursion vs Non-recursive Approach
Recursion needs more memory and more computational name
- has to memorize its current state, and passes control from the caller to
the callee.
- sets up a new data structure (you may think of it as a scratch paper for
rough work) called activation record which contains information such as
* where the caller stops
* what actual parameters are passed to the callee
* create new local variables required by the callee function
* the return value of the function at the end
- removes the activation record of the callee when it finishes.
- passes control back to the caller.

## Struct
struct-struct assignment by default is memberwise copy (bit by bit), even array can be copied by deep copy, for pointer it is shallow copy only.
```cpp
#include <iostream>

using namespace std;

struct example {
    int hi;
    int arr[4];
    int *ptr;
};

int main()
{
    example bruh = {5, {1,2,3,4}, new int (5)};
    
    example copy_bruh = bruh;
    cout << bruh.arr << endl;
    cout << copy_bruh.arr << endl;
    cout << endl;
    
    cout << bruh.ptr << endl;
    cout << copy_bruh.ptr << endl;
    
    return 0;
}
/*
0x7fffc8c90c44
0x7fffc8c90c64

0x56498b106eb0
0x56498b106eb0
*/
```

## g++ compilation command & makefile
`myclass.h`, `libmyclass.a` (library file of the object code of implementation of all class member functions, this enforces information hiding)

Build library command: `g++ -c xxx.cpp` first, then `ar rsuv libxxx.a xxx.o xxx.o xxx.o`.
Compilation Command: `g++ -o main main.cpp -L -lmyclass`

# OOP
## C++ Class
Be careful for deleting `char*`, if it is a raw string, it should be `delete []`.
`delete []` will call the class destructor on each array element in reverse order.

```cpp
#include <iostream>
using namespace std;

int times = 0;
struct A {
    int v;
    A() {v = times++;}
    ~A() {cout << v << endl;}
};

int main() {
    A *arr = new A [5];
    delete [] arr;
}
```

Forward definition (same applies to function)
```cpp
class Cell;

class List {
    int size;
    Cell *data;
    Cell x; // need Cell
}

class Cell {
    int info;
    Cell *next;
}
```

Non-static data members that are not having like `float real = 1.3;`, `float imag {0.5}` will have the values of their default member initializer, and undefined if there's no.

```cpp
class A {
  private:
    int v;
  public:
    A() { cout << v << endl;}
};

int main() {
    A *arr = new A [5];
    delete [] arr;
}
```

`inline` is used for inside-header implementation, if implementation is inside class, the keyword is optional, otherwise it is compulsory. Another usage of inline is that by declaring a very short function `inline`, we serve a hint to the compiler to make it inline as function calling is expensive, but the compiler still has the freedom to choose it or not.

`protected`: accessible to
- member functions and friends of the class, as well as
- member functions and friends of its derived classes (subclasses)

We can use `Word movie = {1, "Titanic"}` only if all data members are public

Default behavior:
copy constructor: perform memberwise assignment by calling the assignment operator

`A a();` does not actually mean initialization

MIL: The order of the members in the list doesn’t matter; the actual
execution order is their order in the class declaration.
Advantage: avoid using assignment operator if it is not appropriately defined (error-prone)

`reinterpret_cast<void *>(")`, output the address even though it is char *

## Construction and Destruction
has-a-relationship vs own-a-relationship
Has-a-relationship:
```cpp
class A {};
class B {
    A a;
    B() = default; // B(): a() {}
};
```
Own-a-relationship
```cpp
class A {};
class B {
    A* a;
    B() = default; 
};
```
The Clock object is constructed in the Postoffice constructor, but
it is never destructed, since we have not implemented that, we need to manually delete it in the destructor.

## Inheritance & Polymorphism
Remember to include the father into MIL:
```cpp
struct A {
    int val;
    A(int val): val(val+10) {}
    virtual void print_label() { cout << "A " << val << "\n"; }
};

struct B: A {
    int val;
    B(int val) : A(val), val(val) {}
    void play() {
        cout << "play\n";
    }
    virtual void print_label() override { cout << "B " << val << "\n"; }
};
```

Construction Order:
1. its parent
2. its data members (in order of their appearance)
3. itself

```cpp
struct A {
    virtual void print() const {
        cout << "A\n";
    }
};

struct B : A {
    virtual void print() const override {
        cout << "B\n";
    }
};

void plv(A a) {
    a.print();
}

void plr(const A& a) {
    a.print();
}

void plp(const A* a) {
    a->print();
}

int main() {
    B b;
    plv(b);
    plr(b);
    plp(&b);
}
```
Using virtual function only helps with pointer + reference to avoid slicing, can't help with PBV. (this uses the dynamic binding technique, determine the called function at run time).

A virtual function is declared using the keyword virtual in the class
definition, and not in the member function implementation, if it is
defined outside the class.
Once a member function is declared virtual in the base class, it is
automatically virtual in all directly or indirectly derived classes.
Even though it is not necessary to use the virtual keyword in the
derived classes, it is a good style to do so because it improves the
readability of header files.

static_cast vs dynamic_cast vs reinterpret_cast
There's no any checking of static_cast and it does not consult RTTI so it runs faster than dynamic_cast. Dynamic_cast only works on pointer and reference of polymorphic class (with virtual functions), it checks and returns a null pointer if conversion of pointer fails (a valid complete object
of the target type), for reference it will trigger runtime error.

It's fine to use static_cast for upper casting ie inherited=>base, but for base=>inherited better use dynamic_cast.

Do not rely on the virtual function mechanism during the execution of
a constructor.
```cpp
class Base {
  public:
    Base() { cout << "Base's constructor\n"; f(); }
    virtual void f() { cout << "Base::f()" << endl; }
};

class Derived : public Base {
  public:
    Derived() { cout << "Derived's constructor\n"; }
    virtual void f() override { cout << "Derived::f()" << endl; }
};

int main() {
    Base* p = new Derived;
    cout << "Derived-class object created" << endl;
    p->f();
}
```
This is not a bug, but necessary — how can the derived object
provide services if it has not been constructed yet?
Similarly, if a virtual function is called inside the base class destructor,
it represents base class’ virtual function: when a derived class is being
deleted, the derived-specific portion has already been deleted before
the base class destructor is called!

pointers and references to an ABC can be declared, but use it as PBV is prohibited.

public / protected / private inheritance
1. Public inheritance preserves the original accessibility of inherited
members:
public ⇒ public
protected ⇒ protected
private ⇒ private
2. Protected inheritance affects only public members and renders them
protected.
public ⇒ protected
protected ⇒ protected
private ⇒ private
3. Private inheritance renders all inherited members private.
public ⇒ private
protected ⇒ private
private ⇒ private

Private and protected inheritance do not allow casting of objects of
derived classes back to the base class.

## Generic Programming
```cpp
/* General case */
template <typename T>
const T& larger(const T& a, const T& b)
{ cout << "general case: "; return (a < b) ? b : a; }

/* Exceptional case */
template <>
const char* const & larger(const char* const & a, const char* const & b)
{ cout << "special case: "; return (strcmp(a, b) < 0) ? b : a; }
```
const pointers match `const T&`, if delete the second const it would result in type mismatch

```cpp
// Here, x is a reference to an array of N objects of type T
template <typename T, int N>
int f(T (&x) [N]) { return N; }
```
Return size of an array

## Operator Overloading
= copy assignment operator vs copy constructor
We can implement copy assignment operator by copy constructor (like copy-swap) or vice versa.
```cpp
Word(const Word& w): str{nullptr} { cout << "Copy: "; *this = w; }
```

copy assignment need to delete old data (rmb to avoid self-assignment)
actually we can use copy and swap
```cpp
dumb_array& operator=(const dumb_array& other) {
    dumb_array temp(other); // call the copy constructor
    swap(*this, temp); // this content would be deleted after the function call

    return *this;
}
```

+=, = returns const reference
avoid situation like a = b = c, this implicitly means (a.equal(b)).equal(c), which is not intended (it makes a = b, then a = c)

+: member function vs global function
```cpp
Vector Vector::operator+(const Vector &a);
Vector operator+(const Vector& a, const Vector &b);
```

# Data Structure

## rvalue Reference and Move Semantics
Revisit that a variable has dual rules: **lvalue** (read-write) or **rvalue** (read-only). More kinds of values: **xvalue**, **glvalue**, **prvalue**, this would be introduced later.

**rvalue Reference Definition**
```cpp
T && <variable> = <temporary object>
```