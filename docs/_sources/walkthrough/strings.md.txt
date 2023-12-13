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
Some items can be formatted using format specifiers. Note that they are case insensitive. Below is a table that shows available specifiers for different types:

#### Integers

| Specifier | Description | Example |
| --------- | ----------- | ------- |
| `N` | `N` is a positive whole number, which specifies how many digits. If there are not enough, padding 0s are added. | `2` for `123` is `23`. `4` for `23` is `0023`. |
| b | Binary number. | `7` is `111`. |
| d | Decimal number (unneccessary). | `7` is `7`. |
| e | Exponential notation. | `17000` would be `1.7e+004`. |
| h | Hex number. | `32` is `20`. |
| o | Octal number. | `8` would be `10`. |
| x | Hex number. | `32` is `20`. |

#### Floats
| `N`.`M` | `N` is a positive whole number, which specifies how many digits in the whole number's place, and `M` is for the decimal's place. If there are not enough, padding 0s are added. | `2.3` for `123.34` is `23.340`. |
| b | Binary number. | `7.25` is `111.01`. |
| d | Decimal number (unneccessary). | `7.3` is `7.3`. |
| e | Exponential notation. | `17000.54` would be `1.700054e+004`. |
| h | Hex number. | `32.25` is `20.4`. |
| o | Octal number. | `8.25` would be `10.4`. |
| x | Hex number. | `32.25` is `20.4`. |

#### Fixed
| `N`.`M` | `N` is a positive whole number, which specifies how many digits in the whole number's place, and `M` is for the decimal's place. If there are not enough, padding 0s are added. | `2.3` for `123.34` is `23.340`. |
| b | Binary number. | `7.25` is `111.01`. |
| d | Decimal number (unneccessary). | `7.3` is `7.3`. |
| e | Exponential notation. | `17000.54` would be `1.700054e+004`. |
| h | Hex number. | `32.25` is `20.4`. |
| o | Octal number. | `8.25` would be `10.4`. |
| x | Hex number. | `32.25` is `20.4`. |

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
Replacing parts of strings with another string is done with a function rather than an operator. There are many string functions available to be utilized other than this:

```rust
fn main()
{
    println("Hello World!".replace("o", "i"));
}
```
Output:
```
Helli Wirld!
```

## Substrings
Strings are arrays of characters. Thus, you can get a substring by simply taking a slice of the character array:

```rust
fn main()
{
    str text = "Hello World!";
    char[]& byRef -> text[2:4];
    byRef[0] = "7";
    str subStr = text[:-2];
    println(byRef);
    println(subStr);
}
```
Output:
```
[ 7, l, o ]
Hello World
```

## Challenges
TODO!!!

## Challenge Solutions
TODO!!!