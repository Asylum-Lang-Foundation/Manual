```{eval-rst}
:orphan:
```

# Challenge 3
Here, you were asked to make a function to solve for `c` in `a^2 + b^2 = c^2`. After solving for `c`, you get `c = sqrt(a^2 + b^2)` or:

```rust
// The data type used doesn't matter too much, but it makes sense for it to be a floating-point number of sorts.
fn pythagoreanTheorem(float a, float b) {
    float c = a * a + b * b;
    c **= 0.5;
    c
}