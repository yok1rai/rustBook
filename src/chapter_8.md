# Comments

All programmers strive to make their code easy to understand, but sometimes extra explanation is warranted. In these cases, programmers leave comments in their source code that the compiler will ignore but that people reading the source code may find useful.

here's a simple comment

```rust
// hello world
```

In Rust, the idiomatic comment style starts a comment with two slashes, and the comment continues until the end of the line. For comments that extend beyond a single line, you’ll need to include // on each line, like this:

```rust
// So we're doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what's going on.
```

Or you can use the multiline comment syntax with `/*` and `*/`:

```rust
/* So we’re doing something complicated here, long enough that we need
   multiple lines of comments to do it! Whew! Hopefully, this comment will
   explain what’s going on. */
```

