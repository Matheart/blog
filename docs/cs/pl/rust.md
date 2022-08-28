# Rust
!!! Abstract
A brief summary of what I have learnt about this language. Many of the examples and sentences here are directly adopted from [The Rust Book](https://doc.rust-lang.org/book). 

## Compile and Run a single file
When there's only one file, we can use `rustc main.rs` to compile (just like `javac` in `Java`) and then use `./main.rs` to run it.

## Compile and Run a project
Cargo is the powerful package manager of Rust. It is easy to use, only several commands can satisfy most of the daily usage.

### Useful Commands
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
```rust title="hello world"
fn main() {
    println!("Hello, world!");
}
```
As you can see, the syntax here is quite similar to C/C++, there are some differences though. `fn` here refers to function, and also `!` is added behind `println` as `println` in Rust is a macro. (The meaning of macro would be introduced later).

### Variables and Mutability
Like in C++ there are variables and constants, in Scala there are `var` and `val`, we use `let` to define new variables, but the variables are by default immutable. Immutability has a lot of advantages (check [Wikipedia page](https://en.wikipedia.org/wiki/Immutable_object) for detailed explanations). To make the variable mutable, we should add the keyword `mut` at the beginning.
```rust
let x = 5;
let mut y = 6;
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
!!! Notice
    However, note that constants and immutable variables are different. The naming convention for naming constants is using all upper cases. Constants are valid for the entire time a program runs, within the scope they were declared in. 

