# Challenge 2
In this question, you were asked to write a program that prints the first 3 letters of the alphabet together, but you are only allowed to print one letter at a time. If you remember, the `print` function will print what you feed it without writing a new line. But remember, it is important to end your program on a new line, otherwise your OS's terminal will be out of place when you go to enter in a new command. To counter this, the `C` is written with a new line:

```rust
fn main() {
    print("A");
    print("B");
    println("C");
}
```
Output:
```
ABC
```