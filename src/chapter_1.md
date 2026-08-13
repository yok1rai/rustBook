# What is Rust?

Rust is a [low-level](./footnotes.md#low-level-programming-language), [statically typed](./footnotes.md#statically-typed-programming-language) programming language focused on performance, memory safety, and concurrency.

It is designed to provide low-level control over hardware while preventing many common programming errors at [compile time](./footnotes.md).

Rust was originally created by Graydon Hoare in 2006 and was later sponsored by Mozilla, where it grew into a larger open-source project. Rust was first publicly released in 2010 and reached its first stable release, Rust 1.0, in 2015.

## Memory Safety

Rust provides memory safety without relying on a [garbage collector](./footnotes.md#garbage-collector-gc). Its [ownership](./footnotes.md#ownership), borrowing, and lifetime systems allow the compiler to detect many memory-related errors at compile time.

This helps prevent common problems such as use-after-free, double-free, and many forms of invalid memory access.

## Ownership System

Rust uses an [ownership system](./footnotes.md#ownership) to manage memory automatically. Each value has an owner, and Rust's ownership rules determine when that value can be accessed and when its memory should be released.

Unlike garbage-collected languages, Rust does not need a garbage collector running at runtime to determine when unused memory should be freed. Instead, the compiler checks the ownership rules at compile time.

## High Performance Without Sacrificing Safety

Rust is designed to provide strong safety guarantees without requiring a garbage collector or sacrificing low-level control.

Its abstractions are designed to have minimal or no runtime overhead when they are compiled, allowing Rust programs to achieve performance comparable to languages such as C and C++.

## Concurrency

Rust provides features for writing concurrent programs while using its type system to prevent many common concurrency-related bugs, including data races.

This allows developers to take advantage of multiple CPU cores while maintaining strong compile-time safety guarantees.

## What Is Rust Used For?

Rust can be used for a wide range of software, including:

- Operating systems and system software
- Command-line applications
- Web servers and backend services
- Networking software
- Embedded systems
- Game engines
- Databases
- WebAssembly applications
- Other performance-critical software
