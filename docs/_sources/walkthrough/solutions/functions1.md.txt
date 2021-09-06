# Challenge 1
In this challenge, you were asked to create a function that subtracts `int b` from `int a`. There are multiple ways to define such a function, but below we will take the one line approach:

```rust
fn sub(int a, int b) -> int => a - b;
```

A common mistake is to forget the `-> int` specifying that you are returning an `int`. This is important to have!