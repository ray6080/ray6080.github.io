---
layout: post
title: The Simplified Guide to Rust
comments: true
categories:
- blog
tags:
- system
---

Rust is a programming language foucsing on safety, speed and concurrency. Rust can be treated as a safer and easier alternative to C/C++. It provides powerful high-level abstractions and automatic safe check and memory management without the overhead of GC. As a programming language designed for system development, it has some wonderful features, and we are going to discover them one by one.

## Mutable and Immutable
Every variable in Rust is immutable by default like  in most functional programming languages.

```Rust
let x = 10;
```
The variable `x` is immutable, and its value cannot be changed in the later execution. If mutable is necessary, `x` can be declared as following:

```Rust
let mut x = 10;
x = 5;
```
Immutability in Rust encourages writing code in a way that takes advantage of the safety and easy concurrency. Reassignment to immutable variable leads to compiler errors.

`Constants` is a concept often mixed up with `immutable`. Like immutable variables, constants are also values that are bound to a name and are not allowed to change, but there are a few differences between constants and variables.

+ First, constants are immutable by born, and we are not allowed to `mut` a constant.
+ Secondly, constants are declared by `const` keyword instead of `let`.
+ Thirdly, constants can be declared in any scope, including the global scope, which makes them useful for values that many parts of code need to know about.
+ Lastly, constants may only be set to a constant expression, not the result of a function call or any other value that could only be computed at runtime.

There is one interesting feature called **Shadowing** in Rust. Shadowing allows us to declare new variables with the same name as a previous ones, and the new variable `shadows` the previous one. For example:

```Rust
fn main() {
    let x = 10;
    let x = x * 2;
    println!("The value of x is {}", x);
}
```
`x` is declared as immutable, we cannot change the value of `x` to `x*2`, however, we can declare a new variable named `x` as `x*2` to shadow the previous one.

This feature is especially useful during the type conversion. In Java

```Java
public static void main(String[] args) {
    String numStr = "120";
    int num = Integer.parseInt(numStr);
}
```
We need to declare a `numStr` and a `num`. In Rust

```Rust
fn main() {
    let num = "120";
    let num: u32 = num.parse().expect("Not a Number!");
}
```

Except for variables, there is a special concept `reference` annotated by `&` in Rust. Reference can allow us to refer to some value without taking [ownership](##Ownership) of it, and it can be passed between functions as parameters. As for one variable, at any given time, we can have only one mutable reference or any number of immutable references. Mutable references allow us to change the value of the variable referred to, which means we get the write permission. As for immutable references, only read permission is granted.

```Rust
fn main() {
    let mut s = String::from("Rust");
    
    change(&mut s);
}

fn change(s: &mut String) {
    s.push_str(" Mutable!");
} 
```
This is an example of mutable reference. The string `s` is changed in `change` function, and `s`'s reference is passed into `change` function as a parameter.

If we change 

```Rust
let mut s = String::from("Rust");
```
 to 
 
 ```Rust
 let s = String::from("Rust");
 ```
 , then compiler error will be thrown out, because `s`'s reference is **immutable**, thus `change` function cannot change the value of `s`.
 
 To get more detail of `reference` and `mutable`, we are going to discuss the memory model of Rust -- the *stack* and the *heap*.

## Stack and Heap
Stack and heap are common concepts in programming. 

During the runtime, stack often takes up a known, fixed size, and it has a good feature referred as *last in, first out (LIFO)*, thus, the values in stack are removed in the opposite order of insertion. The stack is often small and fast, because it never has to search for a place to put new data or a place to get data from, which is always on the top. Thus, stack is always for local and temp variables, which are pre-known and fixed size.

For data with a unknown size at compile time or a changing size during runtime, they are stored in heap. The heap is not well organized, so it has no in and out rules. When we put data on the heap, firstly ask for a certain amount of space, and the OS finds a satisfied empty slot, marks it as in use, and return us the pointer to the memory location. This is called `allocating`. In Java, all class instances are stored on heap, if we `new` a class, we are allocating a certain amount of new space on heap. And almost the same, if we call `malloc` in C, we are asking for allocating a new space on heap. What makes the story different is that how we deallocate the unused memory. Java is famous (both in positive and negative ways) for its GC (garbage collector), it can collect unused heap memory automatically based on GC algorithms, and heap in Java is further partitioned into several regions to better support its GC mechanism. However, in C there is no GC mechanism supported, we can do what we want on the heap, and meanwhile we are supposed to do everything on our own.

Rust takes another way, in which it makes use of **ownership** to clean unused heap space once the variable is out of scope.

```Rust
use std::io;

fn main() {
    count_len();
}

fn count_len() {
    let mut line = String::new();
    io::stdin().read_line(&mut line)
        .expect("Failed to read line!");
    let line_len = line.trim().len();
    println!("Line is {}, and length is {}", line.trim(), line_len);
}
```
`line` is allocated on heap, and `line_len` is pushed into stack at runtime. After the call of `count_len` function, `line` will be out of scope, and its heap space is reclaimed.

## Ownership
Ownership is the most unique feature in Rust, and it enables Rust to guarantee safe memory without a GC. Rust has three rules:

+ Each value in Rust has a variable that's called its *owner*.
+ There can only be one owner at a time.
+ When the owner goes out of scope, the value will be dropped.



## Data Types and Structures
### Primitive Data Types
### Structs
### Collections

## Error Hnadling

## Cargo
Cargo is a build tool similar to `Maven`. It can be used to create a project, manage dependencies, build a project into binaries and release a project.

## Bad Parts of Rust
+ Package management system
+ Mod system is too complex, though flexible, compared with Java

## Awesome List
+ [TiKV](https://github.com/pingcap/tikv): *TiKV* is a distributed transactional key value database based on the design of **Spanner** and **HBase**. It has implementation of the Raft consensus algorithm in Rust and consensus states stored in RocksDB to guarantee data consistency.
+ [Rust Clippy](https://github.com/Manishearth/rust-clippy): *Rust Clippy* is a tool for helping Rust developers to write better code. It has a bunch of lints to catch common mistakes in Rust code.
+ [MIO](https://github.com/carllerche/mio): *MIO* is a lightweight I/O library for Rust with a focus on adding as little overhead as possible over the OS abstractions.

