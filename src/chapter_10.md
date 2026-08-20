# I/O

Input/Output (I/O) is how a computer **receives and sends data**.

* **Input:** receiving data
* **Output:** sending or displaying data

In Rust, many I/O operations are handled through the `std::io` module.

> `::` is used to separate parts of a module path.

Displaying text in a terminal isn't the only form of output, and reading input from a keyboard isn't the only form of input.

There are many different kinds of I/O operations. For example, **writing to a file is output**, while **reading from a file is input**. File operations commonly involve both `std::fs` and `std::io`.

You can think of I/O as a **stream of data** flowing into or out of a program.

## Standard I/O Streams

On Unix-like systems and Windows, programs generally have three standard I/O streams:

| Stream   | Full Name       | Purpose                                              | Default Source / Destination |
| :------- | :-------------- | :--------------------------------------------------- | :--------------------------- |
| `stdin`  | standard input  | Where a program reads input from                     | Keyboard                     |
| `stdout` | standard output | Where a program writes normal output                 | Terminal                     |
| `stderr` | standard error  | Where a program writes error and diagnostic messages | Terminal                     |

On Unix-like systems, each stream is identified by a small integer called a **file descriptor**:

* `stdin` → `0`
* `stdout` → `1`
* `stderr` → `2`

You might wonder why there are two output streams. The reason is that it's useful to keep **normal program output** separate from **errors and diagnostic messages**.

This becomes especially useful when working with shells such as Bash, since you can redirect these streams independently.

```bash
# Redirect stdout to results.txt
$ my_program > results.txt

# Redirect stderr to errors.log
$ my_program 2> errors.log

# Redirect stdout to results.txt and stderr to errors.log
$ my_program > results.txt 2> errors.log

# Redirect stdout to everything.txt, then redirect stderr to stdout
$ my_program > everything.txt 2>&1

# Redirect the contents of name.txt into the program's stdin
$ my_program < name.txt
```

Notice that `>` redirects **stdout**, while `2>` redirects **stderr**. The `2` comes from stderr's file descriptor.

Similarly, `<` redirects data **into stdin**.

## I/O in Rust

We've actually already used `stdout` without explicitly interacting with it:

```rust
println!("Hello!");
```

The `println!()` macro writes formatted text followed by a newline to **standard output**.

For writing to `stderr`, Rust provides the `eprintln!()` macro:

```rust
eprintln!("Something went wrong!");
```

`eprintln!()` is generally useful for **errors, diagnostics, debugging information, and other messages that shouldn't be considered normal program output**.

This distinction becomes especially important when your program's output is being redirected or piped to another program.

### `print!()` vs `println!()`

Rust also has a `print!()` macro, which behaves similarly to `println!()` except it does **not** add a trailing newline.

```rust
fn main() {
    print!("Hello, ");
    print!("world!");
    println!(); // just prints a newline
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/my_project`
Hello, world!
```

This is useful when you want to build up a line of output piece by piece, or when printing a prompt so the user can type on the same line:

```rust
use std::io;

fn main() {
    print!("Enter your name: ");

    let mut name = String::new();
    io::stdin()
        .read_line(&mut name)
        .expect("failed to read line");

    println!("Hello, {}!", name.trim());
}
```

### Flushing Output

Here's a subtlety worth knowing: when `stdout` is connected to a terminal, its output is normally **line-buffered**. This means output may remain in a buffer until a newline is written or the buffer is explicitly flushed.

Since `print!()` doesn't add a newline, its output may not appear immediately.

This matters when displaying a prompt before waiting for input. The prompt:

```text
Enter your name:
```

might still be sitting in the output buffer when `read_line()` starts waiting for input.

To force Rust to immediately write anything waiting in the buffer, we use `.flush()`, which comes from the `std::io::Write` trait.

```rust
use std::io::{self, Write};

fn main() {
    print!("Enter your name: ");
    io::stdout().flush().unwrap(); // forces the prompt to be written

    let mut name = String::new();
    io::stdin()
        .read_line(&mut name)
        .expect("failed to read line");

    println!("Hello, {}!", name.trim());
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/my_project`
Enter your name: Yokirai
Hello, Yokirai!
```

Now the prompt is explicitly flushed before the program waits for input.

> **Note:** `println!()` writes a newline, so when stdout is line-buffered and connected to a terminal, that newline normally causes the buffered output to be flushed. You mainly need `.flush()` when using `print!()` for interactive prompts.

### `write!()` and `writeln!()`

Rust also provides the more general `write!()` and `writeln!()` macros.

Unlike `print!()` and `println!()`, these macros let you specify **where** the formatted output should be written.

`write!()` does not add a newline, while `writeln!()` does.

For example, we can write directly to `stdout`:

```rust
use std::io::{self, Write};

fn main() {
    let stdout = io::stdout();
    let mut stdout = stdout.lock();

    write!(stdout, "hello world\n").unwrap();
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/my_project`
hello world
```

> **Note:** `std::io::Write` is a trait that provides methods for writing to I/O destinations such as files, `stdout`, and `stderr`.

You can also use `write!()` with a `String`. However, a `String` isn't an I/O stream, so it uses the `std::fmt::Write` trait instead.

```rust
use std::fmt::Write;

fn main() {
    let mut output = String::new(); // creates an empty String
    write!(output, "Hello world").unwrap();
    println!("{output}");
}
```

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/my_project`
Hello world
```

Notice that both examples use the same `write!()` macro, but different `Write` traits:

* `std::io::Write` → writing to I/O destinations
* `std::fmt::Write` → writing formatted text into things such as `String`

## Reading From `stdin`

Now, to read from `stdin`, we can read one line into a `String`.

```rust
use std::io;

fn main() {
    let mut input = String::new();

    io::stdin()
        .read_line(&mut input)
        .expect("failed to read line");

    println!("you typed: {}", input.trim());
}
```

Once you press Enter, your input gets stored in the `input` variable.

We use the `trim()` method here because `read_line()` keeps the line ending in the `String`. `trim()` removes whitespace from the beginning and end of the string, including the newline.

`.expect("failed to read line")` is there because `read_line()` can technically fail. If that happens, `.expect()` will panic and print the message you gave it.

We'll cover proper error handling later. For now, `.expect()` is a simple way to say, "this should work, and if it doesn't, crash with this message."

Let's run it. Since Rust doesn't print a prompt before `read_line()` waits, the typed line appears directly in the terminal:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.18s
     Running `target/debug/my_project`
I love programming
you typed: I love programming
```

You can also pass input from a file instead of typing it:

```bash
$ cat input.txt
I love programming

$ cargo build
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s

$ ./target/debug/my_program < input.txt
you typed: I love programming
```

Notice that the program doesn't actually know whether the data came from a keyboard or a file. It simply reads from `stdin`. The shell decides where `stdin` gets its data from.

## Building a Python-Style `input()` Function

We can combine `print!()`, `.flush()`, and `.read_line()` into our own function that behaves similarly to Python's `input()` function:

```rust
use std::io::{self, Write};

fn input(prompt: &str) -> String {
    let mut buffer = String::new();

    print!("{prompt}"); // no newline
    io::stdout()
        .flush()
        .expect("cannot flush stdout");

    io::stdin()
        .read_line(&mut buffer)
        .expect("cannot read stdin");

    buffer.trim().to_string()
}

fn main() {
    let name = input("What is your name? ");
    println!("You are {name}");
}
```

The call to `trim()` returns a string slice (`&str`), so we use `to_string()` to create a new `String` from it.

Let's test it out:

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/my_project`
What is your name? David
You are David
```

## Practical Example: Interactive Calculator

Now that we know how to read from `stdin`, write to `stdout`, and write errors to `stderr`, we can put everything together into a basic calculator.

```rust
use std::io::{self, Write};

fn input(prompt: &str) -> String {
    let mut buffer = String::new();

    print!("{prompt}"); // no newline
    io::stdout()
        .flush()
        .expect("cannot flush stdout");

    io::stdin()
        .read_line(&mut buffer)
        .expect("cannot read stdin");

    buffer.trim().to_string()
}

fn main() {
    loop {
        let num1 = input("Enter number 1: ");

        let num1: i32 = match num1.parse() {
            Ok(num) => num,
            Err(_) => {
                eprintln!("You must enter a number!");
                continue;
            }
        };

        let num2 = input("Enter number 2: ");

        let num2: i32 = match num2.parse() {
            Ok(num) => num,
            Err(_) => {
                eprintln!("You must enter a number!");
                continue;
            }
        };

        let op = input("Enter the operator (+-*/): ");

        match op.as_str() {
            "+" => println!("Result: {}", num1 + num2),
            "-" => println!("Result: {}", num1 - num2),
            "*" => println!("Result: {}", num1 * num2),
            "/" => {
                if num2 == 0 {
                    eprintln!("You cannot divide by 0!");
                    continue;
                }

                println!("Result: {}", num1 / num2);
            }
            _ => {
                eprintln!("Invalid operator!");
                continue;
            }
        }
    }
}
```

Here, `parse()` attempts to convert the input `String` into another type. Because we explicitly tell Rust that `num1` and `num2` are `i32`, Rust knows that we want `parse()` to produce an `i32`.

The `match` handles the two possible outcomes:

* `Ok(num)` means the conversion succeeded.
* `Err(_)` means the conversion failed.

If the conversion fails, we print an error to `stderr` and use `continue` to start the next iteration of the loop.

We also check for division by zero before performing the division.

```bash
$ cargo run
   Compiling my_project v0.1.0 (/home/yok1rai/my_project)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/my_project`
Enter number 1: 20
Enter number 2: 30
Enter the operator (+-*/): +
Result: 50
Enter number 1: 10
Enter number 2: 0
Enter the operator (+-*/): /
You cannot divide by 0!
Enter number 1: 40
Enter number 2: 20
Enter the operator (+-*/): *
Result: 800
Enter number 1: a
You must enter a number!
Enter number 1: a
You must enter a number!
Enter number 1: 3
Enter number 2: 2
Enter the operator (+-*/): %
Invalid operator!
Enter number 1: ^C
```

The calculator demonstrates several I/O concepts working together:

* `input()` reads from `stdin`.
* `print!()` writes prompts to `stdout`.
* `.flush()` makes sure prompts appear immediately.
* `println!()` writes normal program output to `stdout`.
* `eprintln!()` writes errors to `stderr`.
* Shell redirection can send `stdout` and `stderr` to different destinations.
* `parse()` converts the input text into an `i32`.
* `match` handles successful and failed conversions.

This is the basic foundation of terminal I/O in Rust. From here, the same ideas can be extended to files, buffered I/O, command-line arguments, and other kinds of input and output
