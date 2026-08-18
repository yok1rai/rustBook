# Control Flow

The ability to redirect a program based on conditions is called control flow.

In a simple diagram:

```mermaid
flowchart TD
    x[x = 5] --> cond{x > 5?}
    cond -->|yes| print1["print 'it's bigger than 5'"]
    cond -->|no| print2["print 'it's not bigger than 5'"]
    print1 --> _end("end")
    print2 --> _end
```

In this chapter, we will learn how to make this.

## `if-else` Expression

An `if` statement allows you to branch your code depending on a condition.

For instance:

```rust
fn main() {
    let active = true;

    if active {
        println!("user is active");
    }
}
```

If the condition is not true, you can use the `else` statement.

```rust
fn main() {
    let active = true;

    if active {
        println!("user is active");
    } else {
        println!("user is not active");
    }
}
```

Let's test it:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/my_project`
user is active
```

It will get even more dynamic as we learn how to take input.

Unlike Python, C, or JavaScript, Rust does not support truthiness.

> What is truthiness? Truthiness is when a value is treated as a boolean even though it isn't actually a boolean.
>
> For example, in C, any non-zero number is treated as `true`, while `0` is treated as `false`. In Python, empty collections such as lists and strings are treated as `false`.
>
> Rust does not have truthiness. Conditions must explicitly evaluate to a `bool` value.

For example:

```rust
fn main() {
    let x = 3;

    if x {
        println!("x is not zero");
    }
}
```

If it was a C or Python program, it would compile and print out something like "x is bigger than 0", but in Rust:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error[E0308]: mismatched types
 --> src/main.rs:4:8
  |
4 |     if x {
  |        ^ expected `bool`, found integer

For more information about this error, try `rustc --explain E0308`.
error: could not compile `my_project` (bin "my_project") due to 1 previous error
```

### Comparison Operators

To use an integer in a condition, we need an expression that produces a bool. Comparison operators are one way to do that!

There are 6 comparison operators in Rust:

- `>`: bigger than
- `>=`: bigger or equal to
- `<`: less than
- `<=`: less or equal to
- `==`: equal to (don't confuse with `=`)
- `!=`: not equal to

Let's make a program that checks if a value is non-zero:

```rust
fn main() {
    let x = 3;
    if x != 0 {
        println!("x is not equal to zero");
    } else { // same as x == 0
        println!("x is zero")
    }
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/my_project`
x is not zero
```

We can program this example too:

```mermaid
flowchart TD
    x[x = 5] --> cond{x > 5?}
    cond -->|yes| print1["print 'it's bigger than 5'"]
    cond -->|no| print2["print 'it's not bigger than 5'"]
    print1 --> _end("end")
    print2 --> _end
```

```rust
fn main() {
    let x = 5;
    if x > 5 {
        println!("it's bigger than 5");
    } else {
        println!("it's not bigger than 5");
    }
}
```

And it would print out the second branch, since `5 > 5` is false.

### `else if` Expression

`else if` is a combination of `else` and `if`. It's checked only if the previous `if` condition is false, allowing you to check another condition.

```rust
fn main() {
    let age = 67;

    if age >= 65 {
        println!("you are old");
    } else if age >= 18 {
        println!("you are an adult");
    } else {
        println!("you are a kid");
    }
}
```

If we run the program, it will print `"you are old"`. This matters because if we'd used a separate `if` statement instead of `else if`, both conditions would trigger, since `67` is greater than or equal to both `65` and `18`.

With `else if`, once a condition is found to be true, the remaining conditions are skipped.

### Logical Operators

You can chain boolean values together, like "if one side is TRUE and the other side is TRUE," and so on.

There are 3 logical operators:

- `&&`: AND
- `||`: OR
- `!`: NOT

AND checks if both values are TRUE, OR checks if at least one value is TRUE, NOT flips the boolean value.

for && (AND):

|SIDE 1|SIDE 2|RESULT|
|:-----|:-----|:-----|
|FALSE |FALSE |FALSE |
|TRUE  |FALSE |FALSE |
|FALSE |TRUE  |FALSE |
|TRUE  |TRUE  |TRUE  |

for || (OR):

|SIDE 1|SIDE 2|RESULT|
|:-----|:-----|:-----|
|FALSE |FALSE |FALSE |
|TRUE  |FALSE |TRUE  |
|FALSE |TRUE  |TRUE  |
|TRUE  |TRUE  |TRUE  |

for ! (NOT):

It only takes one value.

|VALUE|RESULT|
|:----|:-----|
|TRUE |FALSE |
|FALSE|TRUE  |

Let's use them in a real program:

```rust
fn main() {
    let money = 20000;
    let age = 18;
    let is_vip = false;
    let is_banned = false;

    if (money >= 5000 && age >= 18) || is_vip {
        if !is_banned {
            println!("welcome to the party!");
        } else {
            println!("you are banned from this party");
        }
    } else {
        println!("you do not meet requirements to enter this party");
    }
}
```

In this program, we first check if money is greater than or equal to 5000, and then if age is greater than or equal to 18. These are two separate conditions, and both need to be true because we're using the `&&` (AND) operator. If both conditions are true, the entire expression becomes true.

We then use the `||` (OR) operator with `is_vip`. This means either the previous conditions must be true, or `is_vip` must be true. So even if someone doesn't have enough money or isn't 18 yet, they can still enter if they're a VIP.

If that condition is true, we then check `is_banned` using the `!` (NOT) operator. `!is_banned` means `is_banned` must be false. If the person isn't banned, we print "welcome to the party!". Otherwise, we print "you are banned from this party".

Conceptually, it's like this:

```mermaid
flowchart TD
    start([Start]) --> money{money >= 5000?}

    money -->|Yes| age{age >= 18?}
    money -->|No| vip{is_vip?}

    age -->|Yes| banned{"is NOT banned?"}
    age -->|No| vip

    vip -->|Yes| banned
    vip -->|No| requirements["you do not meet requirements to enter this party"]

    banned -->|Yes| welcome["welcome to the party!"]
    banned -->|No| banned_msg["you are banned from this party"]
```

### Using `if` in a `let` Statement

Since `if` is an expression, you can use it in a `let` statement.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };
    println!("The value of number is: {number}"); // 5
}
```

But of course, both branches must return the same type, since Rust is statically typed.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { "hello" };
    println!("The value of number is: {number}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error[E0308]: `if` and `else` have incompatible types
 --> src/main.rs:3:44
  |
3 |     let number = if condition { 5 } else { "hello" };
  |                                 -          ^^^^^^^ expected integer, found `&str`
  |                                 |
  |                                 expected because of this

For more information about this error, try `rustc --explain E0308`.
error: could not compile `my_project` (bin "my_project") due to 1 previous error
```

## `match` Expression

`match` expressions are used for pattern matching, similar to `==`.

```rust
let number = 3;

match number {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("something else")
}
```

Match statements are made of arms.

An arm is made of two parts:

```text
pattern => action,
pattern => action
```

Or you can add curly braces for multiple actions (it will return the last one, provided it has no semicolon):

```text
pattern => {
    action(s)
}
pattern => {
    action(s)
}
```

### `match` Expressions Are Exhaustive

Match statements must cover all possible values.

For example, this will not compile:

```rust
fn main() {
    let x = 1;

    match x {
        0 => println!("it's 0"),
        1 => println!("it's 1"),
    }
}
```

Because Rust doesn't know what to do if the value is 2, 6, or 1200.

If we run it:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error[E0004]: non-exhaustive patterns: `i32::MIN..=-1_i32` and `2_i32..=i32::MAX` not covered
 --> src/main.rs:4:11
  |
4 |     match x {
  |           ^ patterns `i32::MIN..=-1_i32` and `2_i32..=i32::MAX` not covered
  |
  = note: the matched value is of type `i32`
help: ensure that all possible cases are being handled by adding a match arm with a wildcard pattern, a match arm with multiple or-patterns as shown, or multiple match arms
  |
6 ~         1 => println!("it's 1"),
7 ~         i32::MIN..=-1_i32 | 2_i32..=i32::MAX => todo!(),
  |

For more information about this error, try `rustc --explain E0004`.
error: could not compile `my_project` (bin "my_project") due to 1 previous error
```

Since manually listing every value would be absurdly long, we can use the `_` wildcard instead. It basically means "any other value."

```rust
fn main() {
    let x = 1;

    match x {
        0 => println!("it's 0"),
        1 => println!("it's 1"),
        _ => println!("any other value")
    }
}
```

And it will compile.

### `match` Is an Expression

Match statements are expressions, so they can return a value.

```rust
fn main() {
    let access = 2;
    // for reference, it is linux permission system
    let perms = match access {
        0 => (false, false, false),
        1 => (false, false, true),
        2 => (false, true, false),
        3 => (false, true, true),
        4 => (true, false, false),
        5 => (true, false, true),
        6 => (true, true, false),
        7 => (true, true, true),
        _ => {
            println!("invalid entry!");
            return;
        }
    };
    println!("your permissions are: {:?}", perms);
}
```

Since we entered 2, it will return `(false, true, false)`.

As for the `_` arm, it doesn't return `(bool, bool, bool)`, so how does that work? It's because of the never type.

### The Never Type

The never type is used when an expression never produces a value, because it never successfully finishes.

It's written as `!`.

For example:

```rust
fn forever() -> ! {
    loop {}
}
```

This is a never type because it's stuck in an unconditional loop.

Or here:

```rust
fn main() {
    let access = 2;
    // for reference, it is linux permission system
    let perms = match access {
        0 => (false, false, false),
        1 => (false, false, true),
        2 => (false, true, false),
        3 => (false, true, true),
        4 => (true, false, false),
        5 => (true, false, true),
        6 => (true, true, false),
        7 => (true, true, true),
        _ => {
            println!("invalid entry!");
            return;
        }
    };
    println!("your permissions are: {:?}", perms);
}
```

This part is a never type:

```rust
        _ => {
            println!("invalid entry!");
            return;
        }
```

Since it uses `return`, it immediately exits the function, which makes it impossible for the `perms` variable to ever receive a value from this arm.

#### The Never Type Can Become Any Type

The never type is very flexible, it's essentially treated as any type you need, because by definition it can never produce a value that would contradict that type.

That's why we didn't have to return `(bool, bool, bool)` from that arm, the never type is already compatible with that
