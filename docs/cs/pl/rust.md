# Rust
!!! Abstract
    Rust is a statically typed language so it must know the types of all variables at compile time. The language does not have a GC (Garbage Collector) but achieve the purposes by its unique functionalities called **ownership** and **lifetime**. Many of the examples and sentences here are directly adopted from [The Rust Programming Language](https://doc.rust-lang.org/book). 

## Basics
### Compile and Run
When there's only one file, we can use `rustc main.rs` to compile (just like `javac` in `Java`) and then use `./main.rs` to run it.

If things get complicated, we could make use of the powerful package manager of Rust, Cargo. It is easy to use, only several commands can satisfy most of the daily usage.

#### Useful Commands
Create a new project
```
$ cargo new hello_cargo
```
Build (compile) the project
```
$ cargo build
```
Compile and Run the project
```
$ cargo run 
```

### Hello World Program
```rust
fn main() {
    println!("Hello, world!");
}
```
As you can see, the syntax here is quite similar to C/C++, there are some differences though. `fn` here refers to function, and also `!` is added behind `println` as `println` in Rust is a macro. (The meaning of macro would be introduced later).

### Compilation / Runtime error of Rust
Rust can give very powerful static checking during compilation and it can help you figure out most of the bugs before actually running the code with useful error message. Runtime error in Rust is called `panic`.

## Variables and Mutability
we use `let` to define new variables, and we call this kind of value assignment as **variable binding**. Like in C++ there are variables and constants, in Scala there are `var` and `val`, we have mutable and immutable variables in Rust as well , but the variables are by default immutable as immutability has a lot of advantages (check [Wikipedia page](https://en.wikipedia.org/wiki/Immutable_object) for detailed explanations). To make the variable mutable, we should add the keyword `mut` at the beginning.
```rust
let x = 5;
let mut y = 6;
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
!!! Notice
    However, note that constants and immutable variables are different. 
    The naming convention for constants is using all upper cases. Constants are valid for the entire time when a program runs, within the scope they were declared in. Also, constants can only be set to a constant expression, not the result of a value that could only be computed during runtime.

**Variable scope** is a concept that is frequently used, it refers to the range within a program for which an item is valid. 

For example:
```rust
{
    let s = "hello";
}
```
Inside the curly bracket there defines a scope, and `s` is valid inside, after going out of the scope, it becomes invalid, just like the local variable.

We can declare a new variable with the same name as previous variable and we call this kind of operation as **shadowing**.
```rust
let y = 5;
let y = y + 1;

{
    let y = y * 2;
    println!("The value of y in the inner scope is: {y}");
}
```
Like this one, we first initialize the immutable variable `y` by binding 5 to it. Then we shadow `y` by `y + 1`. Inside the curly brackets, we shadow `y` again, when the scope is over, the inner shadowing ends. So the output value should be 6.

The following code can run smoothly:
```rust
let spaces = "   ";
spaces = spaces.len();
```
However, if we add `mut` to `spaces`. It would pop out errors because the compiler perceives it as mutating the type of the variable instead of shadowing, which is not allowed.

## Data Type I
!!!info
    Here I will only introduce some basic data types, more complicated ones like String, Vector, Map would be introduced later as `Data Type II`.

As shown previously, we don't always need to write out the type explicitly (`let y = 5;`) unless the compiler requires more about the type information of the variable.
If doing so, it would be like: `let y: i32 = 5;`, it is called **type annotation**.

There are two data type subsets, namely **Scalar Types** and **Compound Types**.

### Scalar Type
#### Integer Type
`u32` is one of the integer types, where `u` refers to unsigned, `32` refers to the number of bits. Similarly, there are `i32`, `u16`, `u32`, `i128`, ... By basic CS knowledge, it is trivial to calculate the range of each type.  The `isize` and `usize` types depend on the architecture of the computer your program is running on, which is denoted in the table as “arch”: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture. We can call the functions `usize::MIN`, `usize::MAX`, `isize::MIN`, `isize::MAX`.

The number literals can be represented under different basis. For example: `0xff`, `0o77`, `0b1100_0100`, `b'A'`, `345_678` etc. One of the unique features of Rust is that it can insert `_` inside the numbers to improve the readability like the binary number given above.

We still need to handle the integer overflow issue in Rust. 

Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`
Return the None value if there is overflow with the `checked_*` methods
Return the value and a boolean indicating whether there was overflow with the `overflowing_*` methods
Saturate at the value’s minimum or maximum values with `saturating_* ` methods

```rust
let of_x : u8 = 233;
let of_y : u8 = 133;
of_x.wrapping_add(of_y); // 110
of_x.checked_add(of_y); // None
of_x.overflowing_add(of_y).1; // true
of_x.saturating_add(of_y)); // 255
```

#### Floating-point Type
Only `f32` and `f64`.


The boolean type and char type are similiar to C/C++.

### Compound Type
#### Tuple Type
Tuple has fixed length but allows its elements to have different types. 
We can use pattern matching to destructure a tuple value as following:
```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```
We could also access the components of them using the following syntax:
```rust
tup.0; tup.1; tup.2;
```
#### Array Type
The array still has fixed length but all elements inside must have the same type. It is not as flexible as the vector type as it has varible length. However, the arrays are useful if you want your data to be allocated on the stack rather than the heap.
```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
let b = [3; 5]; // [3, 3, 3, 3, 3]
```
We could access the elements by using the following syntax:
```rust
a[0]; a[1]; a[2];
```

Rust can pop out runtime errors when we have invalid array element access.  
```rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!("The value of the element at index {index} is: {element}");
}
```
We neglect the very detail of this program at this moment, we only care about if we input `6` as index, the following error would pop out while C / C++ cannot:
```
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
``` 
This actually shows Rust's memory safety principles in action. It is very hard to debug if having invalid array elements access in C/C++ because it would trigger unexpected modification of data in other side of the program.

## Function
A typical example:
```rust
fn plus_two(x: i32, y: i32) -> i32 {
    x + y
}
```
We use `: i32` to give type annotation on the incoming arguments, and `->` refers to the return type. `()` (unit type) stands for nothing is returned.

You may notice no `;` is put behind `x + y`, this shows the difference between **statement** and **expression**.

**Statements** are instructions that perform a certain action, while **expressions** evaluate to a resulting value. Like the one above is an expression, where `;` should noe be added.

Another example:
```rust
let x = (let y = 6);
```
This one is incorrect, if you want to bind the value of y to x, the correct code should be:
```rust
let x = {
    let y = 6;
    y
};
```

## Comments
Comments in Rust are just similiar to C/C++.
```rust
// Hello,
// we are comments!
```
Documentation Comments would be written later here.

## Control Flows
### `if` Expressions
One Example (no parenthesis is needed):
```rust
if number < 5 {
    println!("condition was true");
} else {
    println!("condition was false");
}
```

Note that the following code cannot compiled unlike C/C++:
```rust
fn main() {
    let number = 3;

    if number {
        println!("number was three");
    }
}
```
The integer cannot be directly casted into bool. So should write `if number != 0`.

**Nested if** is the same as C/C++.

There's one useful syntactic sugar:
```rust
let number = if condition { 5 } else { 6 };
```
!!! Reminders
    The types of results of two expressions should be the same since `number` can only have one type.

### loops
#### loop
This is the endless loop which can only be stopped by the `break` command:
```rust
loop {
    bruhbruhbruh;

    if flag {
        break;
    }
}
```
If there are nested loops, we could label the loops and indicate their names during break.
```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

Besides, we could use `while` and `for` loop as well:

`While` loop:
```rust
while number != 0 {
    println!("{number}!");

    number -= 1;
}
```
`for` loop:
```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("the value is: {element}");
}
```

```rust
for number in (1..4) {
    println!("{number}!");
}
```
Here `(1..4)` refers to `[1, 4)` just like Python, to refer to `[1, 4]`, we should use `(1..=4)` instead.

## * Ownership
!!!info
    Ownership is Rust’s most unique feature and has deep implications for the rest of the language. It enables Rust to make memory safety guarantees without needing a garbage collector, so it’s important to understand how ownership works.

It governs how a Rust program manages memory.

Stack and Heap are two important data structures in memory management.

- Stack is FILO, all data stored on the stack must have a known, fixed size.
- Heap is less organized and stores data with unknown size at compile time or a size that might change. 

#### Efficency of putting new data
The memory allocator of heap would find an empty spot that is big enough if new data to be inserted, this is called **allocating on the heap**. Therefore, pushing to the stack is faster than allocating on the heap as the allocator does not need to search for a place to store new data, the new location is always at the top of stack.

#### Efficency of accessing data
It is still faster on the stack as it does not need to follow a pointer to get there.

The main purpose of ownership is keeping track of what parts of code are using what data on the heap, minimizing the amount of duplicate data on the heap, and cleaning up unused data on the heap so you don’t run out of space are all problems that ownership addresses. 

There are three ownership rules:

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

## Data Type II
### String Type
The `String` type is far more complicated than it seems.
```rust
let s1 = String::from("hello");
let s2 = "world".to_string();
```
We can convert raw string to the `String` type either by calling `String::from` or `.to_string()`.

The internal structure looks like this:

<img src="../../../assets/string_internal.svg" alt="drawing" width="250"/>

The left part is stored on stack, consisting of: the length and capacity (we can ignore capacity at this moment) of the string, and the pointer `ptr` pointing to string content on the heap. We store it on the heap because the length of string might be changed later (e.g.: extension).

We use different methods to append new raw string / char.
```rust
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
println!("s2 is {}", s2);

let mut s = String::from("lo");
s.push('l');
```

We will cover the shallow and deep copying issue.

### Vector

### Map