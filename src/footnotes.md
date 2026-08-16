
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

## Keywords

Keywords are special words that are reserved by the programming language for specific purposes. You cannot normally use them as names for variables, functions, or other identifiers

## List of Keywords in Rust

there are 3 type of keywords in rust

1. Strict keywords -> They have definied meaning in Rust and you cannot use them
2. Reserved keywords -> These are reserved for possible future use, so you generally can't use them as identifiers
3. Weak keywords -> These are treated as keywords only in specific contexts, so you can use them as identifiers in other contexts.

### Strict Keywords

|Name|Description|
|:--|:-----------|
|as|Performs type casting or renames imports|
|async|Defines asynchronous functions, blocks, and closures|
|await|Waits for an asynchronous operation|
|break|exists a loop|
|const|Defines constants or constant contexts|
|continue|skips to the next loop iteration|
|crate|Refers to the current crate|
|dyn|Enables dynamic dispatch through trait objects|
|else|Provides an alternative branch for `if`|
|enum|Defines an enumeration|
|extern|Declares external functions or ABI boundaries|
|false|Boolean literal representing false|
|fn|Defines a function|
|for|defines a `for` loop|
|if|Creates a conditional expression|
|impl|Defines implementations for types or traits|
|in|Specifies the iterator in a for loop|
|let|declares a variable|
|loop|creates an unconditional loop|
|match|performs a pattern matching|
|mod|defines a module|
|move|Transfers ownership into a closure|
|mut|Makes a binding or reference mutable|
|pub|Makes an item publicly accessible|
|ref|Creates a reference binding in a pattern|
|return|Returns a value from a function|
|self|Refers to the current value, module, or context|
|Self|Refers to the implementing type|
|static|Defines a static item or refers to the `'static` lifetime|
|struct|defines a structure|
|super|refers to the parent module|
|trait|defines a trait|
|true|Boolean literal representing true|
|type|Defines a type alias or associated type|
|unsafe|Marks unsafe code or operations|
|use|Imports names into scope|
|where|Specifies generic constraints|
|while|Creates a conditional loop|

### Reserved Keywords

- abstract
- become
- box
- do
- final
- gen
- macro
- override
- priv
- try
- typeof
- unsized
- virtual
- yield

### Weak Keywords

|Name|Description|
|:---|:----------|
|union|Used when defining a union type|
|raw|Used in certain raw identifier/pointer syntax|

## Cases

Rust code uses a few different naming conventions depending on what you're naming. Here they are and where each one is used.

### Snake Case

Snake case means all lowercase words separated by underscores, like `my_variable_name`.

Used for:
- variable names, like `let user_age = 19;`
- function names, like `fn calculate_total() {}`
- module names, like `mod my_module {}`
- file names, like `main.rs` or `user_input.rs`

### Screaming Snake Case

Screaming snake case means all uppercase words separated by underscores, like `MY_CONSTANT_NAME`.

Used for:
- constants, like `const MAX_SPEED: u32 = 120;`
- static variables, like `static APP_NAME: &str = "MyApp";`

### Upper Camel Case (Pascal Case)

Upper camel case means every word starts with a capital letter, with no separators, like `MyStructName`.

Used for:
- struct names, like `struct UserAccount {}`
- enum names, like `enum Direction {}`
- enum variant names, like `Direction::North`
- trait names, like `trait Drawable {}`
- type aliases, like `type UserId = u32;`

## Number Limits

### Integer Limits

formula for signed and unsigned integers' limit is:

Unsigned:

$$
n = \text{bit} \\
\text{min} = 0 \\
\text{max} = 2^{n} - 1 \\
\text{number} \in [\text{min}, \text{max}]
$$

signed:

$$
n = \text{bit} \\
\text{min} = -2^{n -1} \\
\text{max} = 2^{n - 1} - 1 \\
\text{number} \in [\text{min}, \text{max}]
$$

#### Limit list for each integers

| Length                 | Signed                                                    | Unsigned                          |
| :--------------------- | :-------------------------------------------------------- | :-------------------------------- |
| 8-bit                  | $[-128, 127]$                                             | $[0, 255]$                        |
| 16-bit                 | $[-32,768, 32,767]$                                       | $[0, 65,535]$                     |
| 32-bit                 | $[-2,147,483,648, 2,147,483,647]$                         | $[0, 4,294,967,295]$              |
| 64-bit                 | $[-9,223,372,036,854,775,808, 9,223,372,036,854,775,807]$ | $[0, 18,446,744,073,709,551,615]$ |
| 128-bit                | $[-2^{127}, 2^{127}-1]$                                   | $[0, 2^{128}-1]$                  |
