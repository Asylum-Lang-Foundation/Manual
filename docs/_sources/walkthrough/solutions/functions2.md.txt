```{eval-rst}
:orphan:
```

# Challenge 2
Now, you were asked to create 2 additional functions to subtract more parameters, each in a unique way. In the first one you can use a standard return statement, in the second one you can use the `=>` (lambda) operator.

```rust
fn sub(int a, int b, int c) -> int {
    return a - b - c;
}

fn sub(int a, int b, int c, int d) -> int => a - b - c - d
```