# Hello, World

Now that we have installed Rust, we can start writing some Rust code.

Rust source files use the `.rs` extension. Create a new directory and inside it, create a file named `main.rs`.

For executable Rust programs, there must be a function called `main`. This function is the **entry point** of the program—the first function that runs when your program starts.

To define a function, we use the `fn` keyword:

```rust
fn main() {
    // code goes here
}
```

Like C, C++, JavaScript, and many other programming languages, Rust uses **curly braces `{}`** to define blocks of code.

To print a line of text to the terminal, we use the `println!()` macro:

> **Note:** `println!()` is a macro, not a function. Macros are an advanced topic that we'll cover later.
>
> Macros have syntax similar to functions, but their names are followed by an exclamation mark (`!`), such as `println!()` and `vec![]`.

Our complete program looks like this:

```rust
fn main() {
    println!("Hello, World!");
}
```

Let's break it down:

1. We defined a function called `main`, which is the entry point of our program.
2. Inside `main`, we called the `println!()` macro and passed `"Hello, World!"` as its argument.
3. We ended the `println!()` statement with a semicolon.

> **Note:** Semicolons are used to terminate statements in Rust. However, not every Rust expression requires a semicolon. We'll learn more about this later.

## Compilation and Execution

To compile our program, we'll use `rustc`, which stands for **Rust Compiler**. It is installed alongside Rust through `rustup`.

Run:

```bash
$ rustc main.rs
$ ls
main  main.rs
```

The compiler creates an executable binary called `main`.

Now we can run it.

### Linux and macOS

```bash
$ ./main
Hello, World!
```

### Windows

```powershell
> .\main.exe
Hello, World!
```

And that's it—we've successfully written, compiled, and executed our first Rust program.
