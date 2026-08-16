# Hello, World

Now that we have installed Rust, we can start writing some Rust code.

Rust source files use the `.rs` extension. Create a new directory and inside it, create a file named `main.rs`.

For executable Rust programs, there must be a function called `main`. This function is the **entry point** of the program, the first function that runs when the program starts.

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

> **Note:** Semicolons are commonly used to terminate statements in Rust. However, not every Rust expression requires a semicolon. We'll learn more about this later.

## Compilation and Execution

To compile our program, we'll use `rustc`, which stands for **Rust Compiler**. It is included with the Rust toolchain installed through `rustup`.

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

And that's it, we've successfully written, compiled, and executed our first Rust program.

However, manually invoking `rustc` is not how we normally manage Rust projects. For that, Rust provides **Cargo**.

# Cargo

Cargo is Rust's official **package manager and build system**. It is also used for managing dependencies, running tests, generating documentation, and performing many other common tasks.

For most Rust projects, you'll use Cargo instead of invoking `rustc` directly.

To create a new Cargo project, use the `new` command followed by the project's name:

```bash
$ cargo new my_project
    Creating binary (application) `my_project` package
note: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
```

Here, we created a new project called `my_project`.

If we inspect its directory:

```bash
$ tree my_project
my_project/
├── Cargo.toml
└── src
    └── main.rs
2 directories, 2 files
```

A newly created Cargo binary project contains:

* `Cargo.toml` -> the project's manifest file
* `src/` -> the directory containing the project's source code
* `src/main.rs` -> the default binary crate root

## Cargo.toml

`Cargo.toml` is the **manifest file** for your Rust project.

It contains information about your package, such as its name, version, Rust edition, dependencies, and other configuration.

Let's look inside our `Cargo.toml`:

```bash
$ cd my_project
$ ls
src/
Cargo.toml

$ cat Cargo.toml
[package]
name = "my_project"
version = "0.1.0"
edition = "2024"

[dependencies]
```

By default, Cargo creates two sections:

### `[package]`

The `[package]` section contains metadata about your package.

Some commonly used fields are:

* `name` -> the name of the package
* `version` -> the current package version
* `edition` -> the Rust edition used by the package
* `rust-version` -> the minimum supported Rust version (MSRV)
* `description` -> a short description of the package
* `license` -> the package's license, such as `MIT` or `Apache-2.0`
* `keywords` -> keywords used to describe the package

Cargo supports many more package fields, but these are some of the ones you'll encounter most often.

### `[dependencies]`

The `[dependencies]` section is used to declare the **external crates** that your project depends on.

Crates are Rust's compilation and distribution unit and are roughly comparable to libraries or packages in other programming languages.

For example:

```toml
[dependencies]
rand = "0.10.2"
clap = { version = "4.5", features = ["derive"] }
```

The `rand` dependency specifies a version requirement, while `clap` additionally enables the `derive` feature.

> **Note:** A dependency version such as `"0.10.2"` is generally a **version requirement**, not necessarily an instruction to use exactly version `0.10.2`. Cargo uses the requirement to determine which compatible version can be selected.

Cargo has many other sections, such as `[features]`, `[workspace]`, `[profile]`, and `[patch]`. We'll cover those later.

A more complete `Cargo.toml` could look like this:

```toml
[package]
name = "my_package"
version = "0.1.0"
authors = ["David", "Alex"]
edition = "2024"
license = "MIT"
description = "A project"

[dependencies]
rand = "0.10.2"
clap = { version = "4.5", features = ["derive"] }
```

## Source Files

The `src` directory contains the source code of a typical Cargo project.

When you create a binary project with `cargo new`, Cargo creates a `src/main.rs` file for you:

```bash
$ cat src/main.rs
fn main() {
    println!("Hello, world!");
}
```

Cargo gives us this simple program as a starting point.

Let's compile and run it.

# Building and Executing

To build our Rust project, we use Cargo's `build` command:

```bash
$ cargo build
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
```

Cargo creates the build output inside the `target` directory.

For a normal binary project on Linux or macOS, the executable will be located at:

```text
target/debug/my_project
```

We can run it directly:

```bash
$ ./target/debug/my_project
Hello, world!
```

After building, our project directory now looks like this:

```bash
$ ls
src/
target/
Cargo.lock
Cargo.toml
```

Two new things appeared: the `target` directory and `Cargo.lock`.

## The `target` Directory

`target` is Cargo's **build output directory**.

It contains compiled binaries, intermediate build artifacts, incremental compilation data, and other files generated during the build process.

For example:

```text
target/
├── debug/
│   ├── build/
│   ├── deps/
│   ├── examples/
│   └── my_project
└── ...
```

The exact contents depend on the project and the commands you've run.

Because `target` contains generated build artifacts rather than source code, you generally **do not commit it to Git**. It is normally added to `.gitignore`.

## Cargo.lock

`Cargo.lock` records the **exact versions of dependencies** that Cargo resolved for the project, along with their dependency relationships.

In `Cargo.toml`, you specify version requirements:

```toml
[dependencies]
rand = "0.10.2"
```

Cargo then resolves those requirements into specific versions and records the result in `Cargo.lock`.

This is important because your dependencies can have dependencies of their own.

For example:

```text
my_project
└── rand
    ├── dependency_a
    └── dependency_b
```

Cargo resolves the entire dependency graph and records the resolved versions in `Cargo.lock`.

> **Note:** `Cargo.lock` is automatically managed by Cargo. You generally should not edit it manually.

If our project has no external dependencies yet, the file can be very small:

```bash
$ cat Cargo.lock
# This file is automatically @generated by Cargo.
# It is not intended for manual editing.
version = 4

[[package]]
name = "my_project"
version = "0.1.0"
```

Once we add external dependencies, `Cargo.lock` will contain entries for them and their transitive dependencies.

> **Note:** For applications and other executable packages, you generally commit `Cargo.lock` to version control. Libraries have different conventions because their dependency resolution is ultimately handled by the package that uses the library.

# `cargo run`

If you want Cargo to build your project and then immediately execute it, you can use the `run` command:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.18s
     Running `target/debug/my_project`
Hello, world!
```

`cargo run` is essentially a convenient way to build and execute your binary.

If the project is already up to date, Cargo may skip compilation and simply run the existing executable.

# `cargo check`

If you only want to check whether your project compiles, you can use:

```bash
$ cargo check
    Checking my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
```

`cargo check` checks your code for compilation errors without producing the final executable.

It is usually much faster than `cargo build`, which makes it useful while developing.

For example, while writing code, you can repeatedly run:

```bash
$ cargo check
```

to quickly catch errors.

# `cargo clean`

If you want to remove Cargo's generated build artifacts, use:

```bash
$ cargo clean
     Removed 14 files, 27.2KiB total
```

Before:

```bash
$ ls
src/
target/
Cargo.lock
Cargo.toml
```

After:

```bash
$ ls
src/
Cargo.lock
Cargo.toml
```

`cargo clean` removes the `target` directory and its contents.

It does **not** remove your source code, `Cargo.toml`, or `Cargo.lock`.

The most important Cargo commands we've covered are:

* `cargo new` -> create a new Cargo project
* `cargo build` -> compile the project
* `cargo run` -> compile and run the project
* `cargo check` -> check whether the project compiles without producing the final executable
* `cargo clean` -> remove generated build artifacts

And the three most important files/directories introduced so far are:

```text
Cargo.toml   → Project manifest and configuration
Cargo.lock   → Resolved dependency versions
src/         → Source code
target/      → Generated build artifacts
```
