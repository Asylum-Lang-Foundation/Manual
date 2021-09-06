# Challenge 2
Now, you were asked to create 2 additional functions to subtract more parameters, each in a unique way. In the first solution, I used `=>`, so for these solutions I must use a `return` statement, and an expression without a semicolon to note a return.

```rust
fn sub(int a, int b, int c) -> int {
    return a - b - c;
}

fn sub(int a, int b, int c, int d) -> int {
    a - b - c - d
}
```