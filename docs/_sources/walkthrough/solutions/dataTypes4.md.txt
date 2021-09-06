# Challenge 4
For this challenge, you were asked to create a new `add` function with numbers with a decimal point. This was fairly open to interpretation, so many answers are valid, some of which may include:

```rust
fn add(float a, float b) -> float => a + b;

// This one is rather fancy, as a double is the same as an f64.
fn add(double a, double b) -> f64 {
    return a + b;
}

fn add(fix12x4 a, fix12x4 b) -> fix12x4 {
    a + b
}
```