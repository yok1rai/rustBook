# Installation

The first step to install Rust is installing the command line tool called `rustup` which is used to manage Rust versions and associated tools


> ### Command Line Notation
> In this chapter and throughout the book, we'll show some commands used in terminal
> Lines that you should enter in terminal will start with `$`
> and powershell specific examples will use `>` rather than `$`

## Installing rustup on Linux-MacOS

If you’re using Linux or macOS, open a terminal and enter the following command:

```bash
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

<details>
    <summary>Breakdown of the command</summary>

This command downloads and runs the official Rust installation script using curl and sh.
- `curl` downloads data from a URL.
- `--proto '=https'` tells curl to only use HTTPS for the request.
- `--tlsv1.2` requires TLS 1.2 or newer for the HTTPS connection.
- `https://sh.rustup.rs` the URL of the Rust installation script.
- `-s` runs curl in silent mode, reducing unnecessary output.
- `-S` shows errors even when silent mode is enabled.
- `-f` makes curl fail if the server returns an HTTP error.
- `|` pipes the downloaded script into the next command.
- `sh` executes the script using the POSIX shell.

</details>

The command downloads a script and starts the installation of the rustup tool, which installs the latest stable version of Rust. You might be prompted for your password. If the install is successful, the following line will appear

```bash
Rust is installed now. Great!
```

You will also need a linker, which is a program that Rust uses to join its compiled outputs into one file. It is likely you already have one. If you get linker errors, you should install a C compiler, which will typically include a linker. A C compiler is also useful because some common Rust packages depend on C code and will need a C compiler.

On macOS, you can get a C compiler by running:

```bash
$ xcode-select --install
```

Linux users should generally install GCC or Clang, according to their distribution’s documentation. For example, if you use Ubuntu, you can install the `build-essential` package.

## Installing rustup on Windows

On Windows, go to `https://www.rust-lang.org/tools/install` and follow the instructions for installing Rust. At some point in the installation, you’ll be prompted to install Visual Studio. This provides a linker and the native libraries needed to compile programs. If you need more help with this step, see `https://rust-lang.github.io/rustup/installation/windows-msvc.html`.

## Troubleshooting

To check whether you have Rust installed correctly, open a shell and enter this line

```bash
$ rustc --version
```

You should see the version number, commit hash, and commit date for the latest stable version that has been released, in the following format

```bash
rustc x.y.z (abcabcabc yyyy-mm-dd)
```

If you see this information, you have installed Rust successfully! If you don’t see this information, check that Rust is in your PATH system variable as follows.

in Windows CMD, use

```batch
> echo %PATH%
```

in Powershell, use

```powershell
> echo $env:Path
```

in Linux and MacOS, use

```bash
$ echo $PATH
```

If that’s all correct and Rust still isn’t working, there are a number of places you can get help. Find out how to get in touch with other Rustaceans (a silly nickname we call ourselves) on the community page.

## Using rustup

Rustup is a toolchain to develop and compile rust programs

### Toolchains

A toolchain is a collection of tools used to develop and compile Rust programs. A Rust toolchain includes the Rust compiler, standard library, Cargo, and other associated tools.

Rustup allows you to install multiple toolchains and switch between them when needed.

You can see the toolchains installed on your system with:

```bash
$ rustup toolchain list
```

for example, you might see

```bash
stable-x86_64-unknown-linux-gnu (active, default)
nightly-x86_64-unknown-linux-gnu
```

The first part, `stable`, is the Rust release channel. The remaining part, `x86_64-unknown-linux-gnu`, describes the platform the toolchain is intended for.

### Rust Release Channels

- Stable -> the recommended channel for most Rust programs. It contains officially released and thoroughly tested features
- Beta -> contains changes that are planned for the next stable release
- Nightly -> contains experimental features and changes that have not yet been released as stable

You can install a toolchain using

```bash
$ rustup toolchain install stable
# or
$ rustup toolchain install nightly
```

### Updating Rust

Rust is updated regularly. To update your installed toolchains, use:

```bash
$ rustup update
```

Rustup will check for newer versions and update the installed toolchains.

### Uninstalling Rust

To uninstall Rust and `rustup`, run the following uninstall script from your shell:

```bash
$ rustup self uninstall
```


