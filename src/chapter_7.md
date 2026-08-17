# Functions

Functions are very important in Rust, you use them frequently.

We already used the `fn` keyword to declare a function (specifically the main function).

In Rust, we write function names in [snake case](./footnotes.md#snake-case).

```rust
fn main() {
    println!("hello world!");
    another_function();
}

fn another_function() {
    println!("another function!");
}
```

We define functions with the keyword `fn`, followed by a name, its parameters, then curly braces for its body.

Let's run it:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/my_project`
hello world!
another function!
```

The lines execute in the order in which they appear in the main function. First the "hello world!" message prints, and then another_function is called and its message is printed.

## Parameters

Parameters are special variables that we give to a function while calling it, written between parentheses.

The real, actual data given is called an argument. So parameters are placeholders, and an argument is what we actually give.

In Rust, you must annotate the type of each argument, since it's a statically typed language.

```rust
fn another_function(x: i32) {
    println!("The value of x is: {x}");
}

fn main() {
    another_function(3);
    another_function(7);
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/my_project`
The value of x is: 3
The value of x is: 7
```

When defining multiple parameters, separate each one with a comma.

```rust
fn print_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}

fn main() {
    print_measurement(5, 'h');
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/my_project`
The measurement is: 5h
```

## Statements and Expressions

Statements are instructions that perform some action but do not return a value, while expressions evaluate to a value.

For instance, `let` is a statement and cannot return anything.

```rust
fn main() {
    let x = (let y = 1);
}
```

It will fail because `let` statements do not return any value.

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error: expected expression, found `let` statement
 --> src/main.rs:2:14
  |
2 |     let x = (let y = 1);
  |              ^^^
  |
  = note: only supported directly in conditions of `if` and `while` expressions

warning: unnecessary parentheses around assigned value
 --> src/main.rs:2:13
  |
2 |     let x = (let y = 1);
  |             ^         ^
  |
  = note: `#[warn(unused_parens)]` (part of `#[warn(unused)]`) on by default
help: remove these parentheses
  |
2 -     let x = (let y = 1);
2 +     let x = let y = 1;
  |

warning: `my_project` (bin "my_project") generated 1 warning
error: could not compile `my_project` (bin "my_project") due to 1 previous error; 1 warning emitted
```

On the other hand, a math operation does return a value, so it's an expression.

```rust
fn main() {
    let x = 3 + 2;
}
```

Code blocks are also expressions in Rust. A block returns its last line (the one without a semicolon) as its value. If the last line has a semicolon, the block returns unit (`()`) instead.

```rust
fn main() {
    let x = {
        let y = 1;
        y + 3
    };
    println!("{x}"); // 4
}
```

## Functions with Return Values

The same rule applies to functions as well.

```rust
fn add(x: i32, y: i32) {
    x + y
}

fn main() {
    let x = add(3,4); // 7
    println!("{x}"); // 7
}
```

Let's try it:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error[E0308]: mismatched types
 --> src/main.rs:2:5
  |
1 | fn add(x: i32, y: i32) {
  |                       - help: try adding a return type: `-> i32`
2 |     x + y
  |     ^^^^^ expected `()`, found `i32`

error[E0277]: `()` doesn't implement `std::fmt::Display`
 --> src/main.rs:7:15
  |
7 |     println!("{x}"); // 7
  |               ^^^ `()` cannot be formatted with the default formatter
  |
  = help: the trait `std::fmt::Display` is not implemented for `()`
  = note: in format strings you may be able to use `{:?}` (or {:#?} for pretty-print) instead

Some errors have detailed explanations: E0277, E0308.
For more information about an error, try `rustc --explain E0277`.
error: could not compile `my_project` (bin "my_project") due to 2 previous errors
```

It failed because you must annotate the return type in Rust. You do this by adding an arrow (`->`).

```rust
fn add(x: i32, y: i32) -> i32 {
    x + y
}

fn main() {
    let x = add(3,4);
    println!("{x}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/my_project`
7
```

For functions, you can also use the `return` keyword, but it's generally not recommended. Leaving off the semicolon on the last line already returns from the block, whereas the `return` keyword directly exits the function.

So where do we actually use it? Mostly when we want to return early, or when we're several levels deep.

```rust
fn add(x: i32, y: i32) -> i32 {
    return x + y; // not recommended
}

fn divide(x: i32, y: i32) -> i32 {
    if y == 0 {
        return -1; // legitimate, an early exit
    }
    x / y
}
```

`return` exits the *entire function* it's in, not just the current block. That's why you should avoid using it to "return" a value out of a block into a variable.

```rust
fn main() {
    let n = 0;
    let is_even = {
        if n % 2 == 0 {
            return true;
        } else {
            return false;
        }
    };
    println!("{is_even}");
}
```

This would fail to compile, because `return` here tries to exit the `main` function itself (which returns `()`), not just the block, so `true`/`false` can't be returned from `main` like that.

Instead, you should drop `return` and just use the block's final expression:

```rust
fn main() {
    let n = 0;
    let is_even = {
        if n % 2 == 0 {
            true
        } else {
            false
        }
    };
    println!("{is_even}");
}
```

Or better yet, skip the block entirely since the comparison is already an expression:

```rust
fn main() {
    let n = 0;
    let is_even = n % 2 == 0;
    println!("{is_even}");
}
```

