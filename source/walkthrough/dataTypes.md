# Data Types
On the last page, you vaguely heard about the `int` data type. But what is a data type, what are they, and how do you use them?

## Purpose Of Data Types
In Asylum, you can think of everything in an "is a" way. For example, 5 "is a" number, the vehicle you drive "is a" car, and `main` "is a" function. **A data type let's the computer know what kind of data you are working with.** Obviously, your computer needs to do different things to add numbers together compared to text. And of course, if you write a function that adds numbers together, you wouldn't want anyone to give it text, a function, or even a car.

## Primitive Data Types
A primitive data type is one given by Asylum. They are definined by the language and compiler itself. Here is a table of primitives:
| Primitive | Description |
| --------- | ----------- |
| object | The "any" type. Every single type, even the ones below "are" an object. It is not recommended to use often though, as conversion from an object to anything else is not guaranteed to be valid and defeats the point of types |
| void | A type that means no type. A function that does not have `->` to signify return a type can be read as `-> void`. It would just be annoying to write that for every function that does not return anything |
| bool | A boolean value that is either `true` or `false` |
| char | A character. You can think of this as single text character like 'h', '7', or ';' |
| wchar | A wide character. It is like a `char`, except it can fit wide characters such as Chinese characters |
| string | A "string" of characters that forms standard text. This is your typical "Hello World!" |
| wstring | A "string" of wide characters that forms extended text. It is like a `string`, but can include wide characters like Chinese characters |
| uX | An unsigned number. X is allowed to be any whole number from 1 to 16777215. An unsigned number is a positive integer (whole number). Its range can be thought of as 2^X. For example, a `u3` data type has a range of `2^3=8`, so can be any value from 0 to 7 inclusively |
| sX | A signed number. X is allowed to be any whole number from 1 to 16777215. Unlike unsigned numbers, signed numbers can be negative, at the expense of half its range being negative. Its range can be thought of as 2^X. For example, an `s4` data type has a range of `2^4=16`, so it can be any value from -8 to 7 inclusively |
| varlen | A number that can be any integer with no range. It is only recommended to be used with really large numbers that need high precision, as it takes more space compared to fixed range unsigned or signed numbers |
| fX | A floating-point number. This can be thought of as a number with decimals, such as `1.234`. X is allowed to be 16, 32, 64, 80, or 128 |
| fixAxB | A fixed-point number with A whole bits, and B decimal bits. This is a signed number that has decimal points. What makes it different from floating-point is that math can be done on it like a signed/unsigned number, which is great for systems that don't have floating-point processor support. The whole bits work like signed numbers, and the decimal bits allow precision of `1/(2^n)`. For example, a `fix3x4` can have the whole part be from `-4` to `3`, and the fractional part be any of `1/2`, `1/4`, `1/8`, and `1/16` added together |
| func<A, ...> | A function that has a return type of A. Any other types after it are the parameter types The add function from earlier is type `func<int, int, int>` |
| event<A, ...> | An event handler that can hold functions to execute. Think of this like a list of fuctions that you can add and remove from, and when executed will call of the functions in the list |
| var | This is not a type, it turns into another type that is automatically determined at compile time. It has its uses, but it is generally not recommended, this is a matter of opinion though |
| unsigned | Represents a generic, unsigned integer with bitwidth determined at compile time |
| signed | Represents a generic, signed integer with bitwidth determined at compile time |
| floating | Represents a floating-point number with bitwidth determined at compile time |
| fixed | Represents a fixed-point number with bitwidth determined at compile time |

## Common Primitives
Wait, most languages use primitives such as `int`, `float`, `ulong`, etc. Where are they? If you look above, Asylum already provides a way to declare unsigned, signed, and floating point numbers. So to get these *normal* types used commonly in programming, Asylum *typedef*s, or creates new types that really just shortcut back to the original type. The below chart shows the typedefs defined by EASL (Embedded Asylum Standard Library):

| Type | True Type | Range |
| ---- | --------- | ----------- |
| sbyte | s8 | -128 to 127 |
| byte | u8 | 0 to 255 |
| short | s16 | -32,768 to 32,767 |
| ushort | u16 | 0 to 65,535 |
| int | s32 | -2,147,483,648 to 2,147,483,647 |
| uint | u32 | 0 to 4,294,967,295 |
| long | s64 | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| ulong | u64 | 0 to 18,446,744,073,709,551,615 |
| longlong | s128 | −170,141,183,460,469,231,731,687,303,715,884,105,728 to 170,141,183,460,469,231,731,687,303,715,884,105,727 |
| ulonglong | u128 | 0 to 340,282,366,920,938,463,463,374,607,431,768,211,455 |
| half | f16 | ? |
| float | f32 | 3.4E +/- 38 (7 digits) |
| double | f64 | 1.7E +/- 308 (15 digits) |
| size_t | uX | X is the bitwidth of the system, which for 32-bit systems is `u32`, `u64` for 64-bit, etc. |

## Special Types
Like most other languages, Asylum has *special types* that are types that involve other types. This statement is confusing, but it'll make sense after looking at them below (where T is any data type). It is not expected for everything listed here to make sense, as they are discussed in more detail later:

| Special Type | Description |
| ------------ | ----------- |
| T, T, ..., T | Tuple - this allows you to store a bunch of other data types in one variable. Ex: `int, floatm string` |
| (T, T, ..., T) | This is the exact same as tuple, except with parenthesis around it. This is useful for removing ambiguity |
| T& | A reference to a data type. For now, think of it as a shortcut to already existing data of the same type |
| T* | A pointer that can only be used in unsafe contexts. For now, think of it as a shortcut to existing data without any safety mechanisms. This also allows you to do math operations with memory positions (it's for more low-level uses so you most likely don't need it) |
| T[] | Dynamic array. This is an array of T that is allowed to be any length |
| T[C] | Constant array. This is an array of T that is constant with size C |
| T[A, B, ...] | A dimensional array where A, B, etc. are constants, but constants are not required to be specified. For example, `int[2, 3]` can be thought of as a 2x3 grid of `int`s. `char[3, , 7]` can be thought of as a 3d array of chars where one dimension is 3 long, another is 7 long, and the other can be any length |
| const T | A type T, but you can only assign to it once. This is optimal for data you know will definitely not change |
| static T | A type T, but there is only one instance of it across everything. For example, say you make a `static int` inside a function, and say inside this function, this `int` is incremented. Instead of creating a new `int` every time you would call the function and having its value increment to one each time, this will instead act like a counter for every time you call the function as that variable is shared across every time that function is called |
| volatile T | If you need to make sure T has changed for say you are using it as a condition in a loop, you can mark it as volatile |
| atomic T | Declaring a variable as atomic will make sure only one thread is allowed to use it at a time |

## Typedefs
Asylum creates those above "shortcuts" but doing what is called a typedef statement. These are to be placed outside of functions, structures, and implementations.

```rust
typedef s24[] medIntArr;
```

This example creates a new type called, `medIntARr`, which is really just a dynamic array of 24-bit signed integers in disguise. You can use `medInt` and `s24[]` interchangibly, they are the exact same thing.

## Structs
Of course, making other names for existing types is cool and all, but it can only go so far. Structs allow you to group together a bunch of data types like so:

```rust
struct Color {
    byte red;
    byte green;
    byte blue;
}

struct Car {
    string licensePlate;
    Color rgbColor;
    int year;
    string model;
}
```
Now you can pass around more meaningful data!

### Infinite Structs
As you can see above, you can put structs inside of other structs. But one thing that is not allowed is putting a struct inside itself. Why is this a problem? Well let's think about it:

````{warning}
```rust
struct IllegalStruct {
    string name;
    int number;
    IllegalStruct illegalStruct;
}
```
````

Nothing too evil looking, right? But the compiler needs to determine the size of a struct at compile time. So let's try and think of how that happens. We know that Asylum takes care of strings, and trying to figure out its size is a complex topic. But strings do have a fixed length, even if it is a black box to us. Ok, `int`, that's easy, 32 bits or 4 bytes. Ok, and it contains an `IllegalStruct`, so we just give it the length of the `IllegalStruct`. Ok, now we need to go inside that `IllegalStruct` and figure out the length of its `IllegalStruct`... This is the problem. We'll keep trying to figure out the length in an infinite loop. So how do we fix it? If you remember from earlier, we have diet-pointers which allow us to have a "shortcut" to data:

```rust
struct LegalStruct {
    string name;
    int number;
    LegalStruct* legalStruct;
}
```
This is legal, since we don't need to know the length of `LegalStruct` to calculate the length of `LegalStruct`. A diet-pointer/shortcut always has a fixed length, no matter what type of data it shortcuts/points to. Using a diet-pointer instead does add some things we will have to do when using it, but this will be discussed later. Also note the following illegal code:

````{warning}
```rust
struct IllegalA {
    string name;
    int number;
    IllegalB illegalB;
}

struct IllegalB {
    string name;
    int number;
    IllegalA illegalA;
}
```
````

Similarly like a struct containing itself, this will also produce an infinite loop for the compiler that won't be able to determine the length of the structure. This can again be fixed by making either the `illegalA` or `illegalB` members a diet-pointer. Note that just having one diet-pointer solves the infinite struct error.

## Challenges
1. Explain the difference between unsigned and signed integers. What is a common signed type?
2. Explain the difference between a floating-point and a fixed-point number. What is a common floating-point type?
3. What type of number is `long`? Unsigned, signed, floating-point, or fixed-point?
4. Remember that `add` function from earlier? Remake it so that `a` and `b` can have a decimal point.
5. Create a `struct` that represents a circle. Each circle has a radius, a line width, color, and can have another circle inside of it. Create any other necessary `struct`s.

## Challenge Solutions
* [Solution 1](solutions/dataTypes1.md)
* [Solution 2](solutions/dataTypes2.md)
* [Solution 3](solutions/dataTypes3.md)
* [Solution 4](solutions/dataTypes4.md)
* [Solution 5](solutions/dataTypes5.md)