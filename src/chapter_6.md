# Basic Types

Every value in Rust is of a certain data type, which tells Rust what kind of data is being specified so that it knows how to work with that data. We'll look at two data type subsets: scalar and compound.

> Rust is a statically typed language, which means that it must know the types
> of all variables at compile time. In most situations, Rust can infer what type we want to use
> based on the value and how we use it. But sometimes we need to explicitly annotate the type

## Scalar Types

A scalar type represents a single value. Rust has four primary scalar types:

- integers
- floating-point numbers
- Booleans
- characters

### Integer Type

An integer is a number without a fractional component. We used an integer type in chapter 5.

There are 2 types of integers, signed and unsigned.

Signed integers can be negative, 0, or positive, but unsigned integers cannot be negative.

> Why do unsigned integers exist? Because the compiler spends 1 bit on the sign, so an 8 bit signed integer
> is actually a 7 bit number plus a sign. But for unsigned, all of the bits are used for the number.

Signed integers start with `i`, unsigned integers start with `u`.

|Length                 |Signed|Unsigned|
|:----------------------|:-----|:-------|
|8-bit                  |i8    |u8      |
|16-bit                 |i16   |u16     |
|32-bit                 |i32   |u32     |
|64-bit                 |i64   |u64     |
|128-bit                |i128  |u128    |
|Architecture-dependent |isize |usize   |

While i8 can be between $[-128, 127]$, u8 can be $[0, 255]$ because it keeps all 8 bits.

You can see all limits [here](./footnotes.md#limit-list-for-each-integers).

By default, Rust uses i32, but you can change it by casting the type.

```rust
fn main() {
let x = 5; // i32
let y: i8 = 3; // i8
let z: usize = 1; // usize
println!("{}", 3u8); // u8
}
```

You can separate large numbers using `_` to read them more easily.

```rust
fn main() {
let x: u128 = 999_999_999_999_999;
}
```

You can also change the base of the number, by default it's 10-based (decimal), but you can write hexadecimal (16-based), octal (8-based), binary (2-based), and more.

|Type of Literal|Base  |Prefix  | Example    | In Decimal |
|:--------------|:-----|:-------|:-----------|-----------:|
|Decimal        | 10   | None   | `42`       |         42 |
|Hexadecimal    | 16   | `0x`   | `0xff`     |        255 |
|Octal          | 8    | `0o`   | `0o77`     |         63 |
|Binary         | 2    | `0b`   | `0b101010` |         42 |

#### Integer Overflow

Every integer type has a range of values it can hold, based on its bit length. If you try to store a value outside that range, you get integer overflow.

For example, `u8` can only hold values from 0 to 255. Let's try to go over that:

```rust
fn main() {
    let x: u8 = 256;
    println!("{x}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error: literal out of range for `u8`
 --> src/main.rs:2:18
  |
2 |     let x: u8 = 256;
  |                  ^^^
  |
  = note: the literal `256` does not fit into the type `u8` whose range is `0..=255`
  = note: `#[deny(overflowing_literals)]` on by default

error: could not compile `my_project` (bin "my_project") due to 1 previous error
```

Since this overflow is a fixed value known at compile time, Rust catches it immediately and refuses to compile.

But overflow can also happen at runtime, for example when doing math on a variable:

```rust
fn main() {
    let mut x: u8 = 255;
    x = x + 1;
    println!("{x}");
}
```

In **debug** mode (the default when you run `cargo run`), Rust adds overflow checks and the program will panic instead of silently producing a wrong value:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/my_project`

thread 'main' panicked at src/main.rs:3:5:
attempt to add with overflow
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

In **release** mode (`cargo run --release` or `cargo build --release`), Rust does not include these checks by default, for performance. Instead, the value wraps around, so `255 + 1` becomes `0` again. This is called two's complement wrapping, and it's usually not what you want, so relying on it is considered a bug.

If you want to control overflow behavior explicitly instead of leaving it to the debug or release default, Rust gives you methods for that, such as:

- `wrapping_add`, wraps around on overflow, like release mode does
- `checked_add`, returns `None` on overflow instead of panicking
- `overflowing_add`, returns the value along with a boolean saying whether it overflowed
- `saturating_add`, clamps the result to the type's max (or min) value instead of overflowing

```rust
fn main() {
    let x: u8 = 255;

    println!("{}", x.wrapping_add(1)); // 0
    println!("{:?}", x.checked_add(1)); // None (we will learn it later)
    println!("{:?}", x.overflowing_add(1)); // (0, true)
    println!("{}", x.saturating_add(1)); // 255
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/my_project`
0
None
(0, true)
255
```

You can add overflow checks for release mode in Cargo.toml like this:

```toml
[profile.release]
overflow-checks = true
```

This way, release mode will also panic if integer overflow occurs, but it will make compile time longer.

### Floating-Point Type

Rust also has two primitive types for floating point numbers, which are numbers with a decimal point.

These are:

- f32, 32 bit floating-point number
- f64, 64 bit floating-point number (default)

Their performance is very similar, so many people skip f32.

```rust
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
    let z = 2.5f32; // f32
}
```

You can convert a floating-point number into an integer (or vice versa) through the `as` keyword.

```rust
fn main() {
    let x = 3.14; // f64
    let y = x as i32; // i32. 3.14 -> 3
    let z = x as f32; // f64. 3 -> 3.0
    println!("{x} {y} {z}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/my_project`
3.14 3 3
```

> note: the `as` keyword does not round the value, it truncates

### Numeric Operations

We can do arithmetic operations with integer and floating-point types.

All numeric operations below:

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // difference
    let difference = 21 - 3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // results in -1

    // remainder
    let remainder = 10 % 3;
}
```

You cannot perform numeric operations inside the curly braces of print macros. They only accept variable names (which means you cannot call functions or anything either). Instead, you put the curly braces empty and write the expression as the next argument.

```rust
fn main() {
    let x = 3;
    println!("{x + 2}"); // invalid
    println!("{}", x + 2); // valid
}
```

### Boolean Type

As in most other programming languages, a Boolean type in Rust has two possible values:

- true
- false

Booleans are one byte in size. The Boolean type in Rust is specified using `bool`.

```rust
fn main() {
    let t = true;
    let f: bool = false; // with explicit type annotation
}
```

> Unlike most programming languages, Rust does not have falsy/truthy values,
> so you cannot write `if 3 {}`, you must pass it a boolean value

### Character Type

Rust's `char` type is the language's most primitive alphabetic type. Here are some examples of declaring char values:

```rust
fn main() {
    let a = 'a';
    let x: char = '𝕏';
    let skull_face = '💀';
}
```

Rust's characters are UTF-8, which means it supports Chinese characters, emojis, and zero-width spaces.

> note: for characters we use '', if you use "" it becomes a String literal

## Compound Types

Compound types are types that contain multiple values. There are two in Rust:

- Tuple type
- Array type

### Tuple Type

A tuple is a general way to group a number of values with a variety of types into one place.

Tuples are created by writing a comma-separated list of values inside parentheses. Each position can be a different type.

```rust
fn main() {
    let x = (1, 3.14, true, '感');
    let y: (i32, bool, f64, i8) = (3000, false, 3241.12, 3); // with explicit type annotation
}
```

To access a tuple element directly, use a period followed by its index number.

```rust
fn main() {
    let tup = (10, 20, false);
    println!("first value of tuple is {}", tup.0); // first value of tuple is 10
}
```

You can also destructure a tuple value like this:

```rust
fn main() {
    let tup = (500, 6.4, 1);
    let (x, y, z) = tup; // x: i32 = 500, y: i32 = 6.4, z: i32 = 1.0
    println!("{x} {y} {z}"); // 500 6.4 1
}
```

The tuple without any values has a special name, unit. This value and its corresponding type are both written `()` and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don't return any other value.

Lastly, we can modify individual elements of a mutable tuple.

```rust
fn main() {
    let mut tuple: (i32, i32) = (1, 2);
    tuple.0 = 0;
    tuple.1 += 5;
    println!("{} {}", tuple.0, tuple.1); // 0 7
}
```

> you cannot add or delete a value from a tuple because tuples have a fixed size at compile time

### Array Type

Unlike a tuple, every element of an array must have the same type.

We write the values in an array as a comma-separated list inside square brackets.

```rust
fn main() {
    let arr1 = [1,2,3,4,5];
    let arr2: [i32;3] = [3.14, 4.15, 2.1];
}
```

Similar to a tuple, an array cannot shrink or grow.

But unlike a tuple, we use \[idx\] syntax to access elements.

```rust
fn main() {
    let arr = [1,2,3];
    println!("first element of the array is {}", arr[0]); // first element of the array is 1
}
```

### Array Invalid Element Access

Let's see what happens if you try to access an element of an array that is past the end of the array, Rust will panic.

```rust
fn main() {
    let arr = [1,2,3,4,5];
    let sixth = arr[5];  // it will panic
    println!("{sixth}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
error: this operation will panic at runtime
 --> src/main.rs:3:17
  |
3 |     let sixth = arr[5]; // it will panic
  |                 ^^^^^^ index out of bounds: the length is 5 but the index is 5
  |
  = note: `#[deny(unconditional_panic)]` on by default

error: could not compile `my_project` (bin "my_project") due to 1 previous error
```

It is especially a problem when you take input from the user, since the user could enter an invalid index. To solve this, we use the `.get()` function of arrays instead.

Instead of panicking for invalid element access, it returns None instead.

None and Some(T) (variants of Option&lt;T&gt;) are an advanced topic that we will cover soon. For the sake of this function, you can imagine Some(T) as value found, None as value not found.

```rust
fn main() {
    let a = [10, 20, 30];
    let x = a.get(1);
    println!("{:?}", x); // :? = debug mode. Some(20)
    let i = a.get(4);
    println!("{:?}", i); // None
}
```

We generally use them with `if let .. else` statements (we will cover them later).

```rust
fn main() {
    let a = [10, 20, 30];
    if let Some(x) = a.get(1) { // if a.get(1) returns a Some, inside of Some will be x
        println!("{x}");
    } else { // if its None
        println!("index not found");
    }
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/my_project`
20
```

If we had changed it to `a.get(3)`:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/my_project`
index not found
```
