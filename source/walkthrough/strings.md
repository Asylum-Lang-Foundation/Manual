# Strings
In any programming language, you typically need a way to pass text around. This is done with strings. As seen from the beginning of the guide, you use quotations to mark a string:

```rust
println("Hello World!");
```
Output:
```
Hello World!
```

## Formatting
The main way of combining strings together is through using formatting. This is done by inputting a value between `{}` (curly brackets) in the string:

```rust
fn displayItem(int val)
{
    println("You're number is... {val}!");
}

fn main()
{
    displayItem(3);
}
```
Output:
```
You're number is... 3!
```

### Format Specifiers
TODO!!! 

## Escape Characters
Escape characters are done using `\` (backlash). For example, to do a newline you would type `\n`. Below is a table of available escape characters:

| Symbol | Name |
| ------ | ---- |
| ' | Single Quote |
| " | Double Quote |
| `{` | Left Curly Bracket |
| `}` | Right Curly Bracket |
| \ | Backslash |
| 0 | Null Character |
| a | Alert |
| b | Backspace |
| f | Form Feed |
| n | New Line |
| r | Carriage Return |
| t | Horizontal Tab |
| uX | Unicode, where `X` is the unicode byte sequence. |
| v | Vertical Tab |
| xFF | Byte, where `FF` is the byte value. |

## Splitting
You may split a string by using a division operator. This will produce a dynamic array:

```rust
fn main()
{
    str[] items = "This.Is.A.Test" / ".";
    for (item : items)
        println(item);
}
```
Output:
```
This
Is
A
Test
```

## Repeating
Multiplying a string by an integer will repeat the string the given number of times.

```rust
fn main()
{
    println("abacab_" * 3);
}
```
Output:
```
abacab_abacab_abacab_
```

## Contains
You may use the `&` operator to see if the first string contains the second:

```rust
fn main()
{
    str item = "Hello World!";
    println(item & "ello");
    println(item & "World.");
}
```
Output:
```
true
false
```

## Replacing
TODO!!!

## Substrings
TODO!!!