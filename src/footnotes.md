# Introductions

## Low-Level Programming Language

A programming language that provides direct or close-to-direct control over a computer's hardware and resources, such as the CPU and memory.

## Statically Typed Programming Language

A programming language where variable and expression types are determined and checked at compile time.

## Compile Time

Compile time is the period when your source code is processed and checked by a compiler before the program runs.

## Garbage Collector (GC)

A garbage collector is a system that automatically identifies memory that a program no longer needs and frees it.

Rust does not use a garbage collector. Instead, it uses a unique system called [ownership](#ownership) to manage memory.

## Ownership

Ownership is Rust's system for managing memory. Every value in Rust has an owner, which is the variable responsible for that value.

When the owner goes out of scope, the value is automatically dropped and its memory is released.

Rust's ownership rules are checked at compile time, allowing Rust to manage memory safely without requiring a garbage collector.
