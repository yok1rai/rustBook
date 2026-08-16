# Variables

Variables are named containers that store data in a program. They let you save information, use it later, and change it as your program runs.

So instead of writing `David` everywhere, you can just put it in a container and use its identifier instead.

Variables in Rust are created with the `let` keyword, and they must be inside a function.

```rust
fn main() {
    let x = 5;
    println!("{x}"); // use curly braces to insert a variable
}
```

Here we created a variable called `x` equal to 5, then printed it out.

Let's test it:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/my_project`
5
```

And it worked.

## Mutating Variables

Mutation means changing a variable's value.

To mutate a variable, you reassign it:

```rust
fn main() {
    let x = 5;
    println!("{x}"); // x is 5
    x = 6; // x is now 6
    println!("{x}"); // x is 6
}
```

Let's run it:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         - first assignment to `x`
3 |     println!("{x}");
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable
  |
help: consider making this binding mutable
  |
2 |     let mut x = 5;
  |         +++

For more information about this error, try `rustc --explain E0384`.
error: could not compile `my_project` (bin "my_project") due to 1 previous error
```

The code failed to compile because Rust variables are immutable by default, meaning you can't mutate them.

To make a variable mutable, add the `mut` keyword (as the compiler's hint suggests).

So let's rewrite it:

```rust
fn main() {
    let mut x = 5; // x is now mutable
    println!("{x}");
    x = 6;
    println!("{x}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/my_project`
5
6
```

## Declaration & Assignment

In Rust, you need to declare and assign a variable to use it.

- **Declaration**, specifying its name and type (mandatory)
- **Assignment**, giving a value to a declared variable (mandatory before you use the variable)

To declare a variable, write its name and type in this format: `let name: type`.

We'll use the `i32` type as default until we properly cover types.

`i32` is a 32-bit integer, similar to `int` in C.

```rust
fn main() {
    let x: i32; // declared
}
```

But we can't use it yet, since it has no value. If we try to:

```rust
fn main() {
    let x: i32;
    println!("{x}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error[E0381]: used binding `x` isn't initialized
 --> src/main.rs:3:16
  |
2 |     let x: i32;
  |         - binding declared here but left uninitialized
3 |     println!("{x}");
  |                ^ `x` used here but it isn't initialized
  |
help: consider assigning a value
  |
2 |     let x: i32 = 42;
  |                ++++

For more information about this error, try `rustc --explain E0381`.
error: could not compile `my_project` (bin "my_project") due to 1 previous error
```

It will fail to compile.

Now let's give it a value:

```rust
fn main() {
    let x: i32;
    x = 5;
    println!("{x}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/my_project`
5
```

You can drop the type annotation once you assign a value, it will default to `i32` for numbers.

```rust
fn main() {
    let x;
    x = 5;
    println!("{x}");
}
```

You can also declare and assign at the same time:

```rust
fn main() {
    let x = 5;
    let y: i32 = 6; // with type annotation
}
```

## Shadowing

Shadowing is declaring a new variable with the **same name** as an existing one. The new variable shadows (hides) the old one from that point onward.

Unlike making a variable `mut`, shadowing creates an entirely new binding.

```rust
fn main() {
    let x = 5;
    println!("{x}"); // 5

    let x = x + 1; // 5 + 1 = 6
    println!("{x}"); // 6

    let x = x * 2; // 6 * 2 = 12
    println!("{x}"); // 12
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/my_project`
5
6
12
```

Rust doesn't delete the old variable, it just hides it beneath the new one. Once the new variable goes out of scope, the old one reappears.

Let's see this in action:

```rust
fn main() {
    let x = 5; // x is 5 here
    {
        let x = x + 1; // x is 6 here
        {
            let x = x * 2; // x is 12 here
            println!("The value of the inner scope is {x}");
        }
        println!("The value of the middle scope is {x}");
    }
    println!("The value of the outer scope is {x}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/my_project`
The value of the inner scope is 12
The value of the middle scope is 6
The value of the outer scope is 5
```

## Declaring Constants

Like immutable variables, constants are values bound to a name that aren't allowed to change, but there are a few differences between constants and variables.

First, you're not allowed to use `mut` with constants. Constants aren't just immutable by default, they're *always* immutable. You declare them with the `const` keyword instead of `let`, and the type must be annotated (the compiler won't guess it for you).

Second, constants can be declared in any scope, while variables can only be declared inside functions.

Here's an example of a constant declaration:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Third, Rust constants are conventionally written in [SCREAMING_SNAKE_CASE](./footnotes.md#screaming-snake-case). If you write them in any other case, Rust will throw a warning.

```rust
const age: u8 = 19;

fn main() {
    println!("{age}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
warning: constant `age` should have an upper case name
 --> src/main.rs:1:7
  |
1 | const age: u8 = 19;
  |       ^^^
  |
  = note: `#[warn(non_upper_case_globals)]` (part of `#[warn(nonstandard_style)]`) on by default
help: convert the identifier to upper case
  |
1 - const age: u8 = 19;
1 + const AGE: u8 = 19;
  |

warning: `my_project` (bin "my_project") generated 1 warning (run `cargo fix --bin "my_project" -p my_project` to apply 1 suggestion)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/my_project`
19
```

It won't break your program, but Rust will nudge you to write it in upper case instead.
