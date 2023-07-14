```{eval-rst}
:orphan:
```

# Challenge 2
Here, you were asked to initialize the `Color` struct in both ways. This referred to either setting the variables manually, or using an initializer:

```rust
fn main() {
    Color color1;
    color1.red = 255;
    color1.green = color1.blue = 255;
    Color color2 = Color {
        red: 255,
        green: 255,
        blue: 255
    };
}