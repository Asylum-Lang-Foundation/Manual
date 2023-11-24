# Data Types
On the last page, you vaguely heard about the `int` data type. But what is a data type, what are they, and how do you use them?

## Purpose Of Data Types
In Asylum, you can think of everything in an "is a" way. For example, 5 "is a" number, the vehicle you drive "is a" car, and `main` "is a" function. **A data type lets the computer know what kind of data you are working with.** Obviously, your computer needs to do different things to add numbers together compared to concatting text. And of course, if you write a function that adds numbers together, you wouldn't want anyone to give it text, a function, or even a car.

## Primitive Data Types
A primitive data type is a type that can not be recreated using other types available in the language. These types are built into the language and can be used anywhere.

### Void
The `void` type can be thought of to mean nothing or be void of anything. Functions return `void `from default as the two functions below are the exact same:

```rust
fn printItem(str item)
{
    println(item)
}
```

```rust
fn printItem(str item) -> void
{
    println(item)
}
```

````warning
It is illegal to declare a parameter or variable of type `void`! You can't have a variable that is nothing!
````

### Bool
Any item that is either true or false can be represented as `bool`. A `bool` is `true` if it is `true` and `false` if it is `false`. By default, a `bool` is `false`. Converting a `bool` to an integer will return a `1` if `true` and `0` if false.

### Integer Types
Perhaps the most common group of types in programming are integer types. Integers are used to represent whole numbers. Signed integers can be negative, while unsigned integers can not be negative. Below are the available integer types:

| Type | Minimum Value | Maximum Value | Description | Overflow/Underflow |
| ---- | ------------- | ------------- | ----------- | ------------------ |
| s`X` | -(2^`X` / 2) | 2^`X` / 2 - 1 | A standard signed integer where `X` is the bitwidth of the number. Ex: `s16` is a signed 16-bit integer that ranges from -32768 to 32767 inclusively. | Undefined |
| u`X` | 0 | 2^`X` - 1 | A standard unsigned integer where `X` is the bitwidth of the number. Ex: `u16` is an unsigned 16-bit integer that ranges from 0 to 65535 inclusively. | Undefined |
| so`X` | -(2^`X` / 2) | 2^`X` / 2 - 1 | Like `sX` but can be overflowed/underflowed. | Wraps |
| uo`X` | 0 | 2^`X` - 1 | Like `uX` but can be overflowed/underflowed. | Wraps |
| int | - | - | A signed integer that is the bitwidth of the system's registers. It is at least 16 bits. It is impossible to have an array index bigger than its range. Pointer math is also done with this. | Undefined |
| uint | - | - | An unsigned integer that is the bitwidth of the system's registers. It is at least 16 bits. It is impossible to have a type that has a size bigger than this. | Undefined |

Any integer has a default value of `0`.

It is also important to note that division by 0 will abort the program and dump a stack trace in debug mode and cause undefined behavior in release mode. In debug mode, an overflow or underflow will abort the program and dump a stack trace if it is undefined.

### Floating Point Types
Another common primitive are numbers that have decimal points in them. Below are the available floating-point types:

| Type | Description |
| ---- | ----------- |
| f`X` | Floating-point number with a bitwidth of `X`. Only `X` values of `16`, `32`, `64`, `80`, and `128` are allowed. |
| float | Floating-point number the size of floating-point registers on the system. It is at least 16-bits. |

Any floating point number has a default value of `0.0`.

Note that any overflow, underflow, division by 0, or invalid operation will abort the program and dump a stack trace in debug mode and cause undefined behavior in release mode.

### Other Numbers
Asylum also supports other types of numbers that are neither integer or floating point types.

| Type | Description |
| ---- | ----------- |
| fix`X`x`Y` | A fixed point number with `X` whole bits and `Y` fractional bits. Fixed-point numbers are signed. Both `X` and `Y` must be positive integers. |
| varlen | A number with infinite range. It can represent any number imaginable, though it comes with the cost of being very slow. |

Both number types have a default value of `0.0`.

Note that any overflow, underflow, division by 0, or invalid operation will abort the program and dump a stack trace in debug mode and cause undefined behavior in release mode.

### Text Based
Strings of text are composed of characters. Below are the types allowed:

| Type | Description |
| ---- | ----------- |
| char | A text character. Characters are in the UTF8 format. You can think of this as a single item in text like 'A'. |
| wchar | A wide text character. Characters are in the UTF16 format. |
| str | A "string" of `char`s. Every string is null-terminated for compatibility with C APIs, though also contains its own length to make operations faster. |
| wstr | A "wide string" of `wchar`s. Every wide string is null-terminated for compatibility with C APIs, though also contains its own length to make operations faster. |

By default, characters are set to `h`. The default value of a string is the empty string.

### Function Type
The function type is in the form `func<T(...)>` It is a type that is a function that has a return type of `T` and parameter types for the function in the parenthesis. The add function from earlier is type `func<int(int, int)>`. It is mostly used for storing functions into variables.

Functions do not have a default value and must be initialized immediately.

### Non-Types
These types are not really types and are context sensitive.

| Type | Description |
| ---- | ----------- |
| var | This is not a type, it turns into another type that is automatically determined at compile time. It has its uses, but it is generally not recommended, this is a matter of opinion though. |
| This | The current type being implemented in an implementation block. |

## Special Types
Like most other languages, Asylum has *special types* that are types that involve other types. This statement is confusing, but it'll make sense after looking at the special types below (where `T` is any non-void data type). It is not expected for everything listed here to make sense, as they are discussed in more detail later:

### Tuple Types
These are used for grouping types so you can return multiple types at the same time, or work with variables that contain multiple types.

| Type | Name | Description |
| -----| ---- | ----------- |
| `T`, `T`, `...`, `T` | Tuple | This allows you to store a bunch of other data types in one variable. Ex: `int, float, str`. |
| (`T`, `T`, `...`, `T`) | Tuple | This is the exact same as tuple, except with parenthesis around it. This is useful for removing ambiguity.

A tuple has the default value where all its elements are at its default value if possible.

### Array Types
Arrays are used for storing blocks of data that do not grow or shrink in size.

| Type | Name | Description |
| ---- | ---- | ----------- |
| `T`[] | Dynamic array. This is an array of `T` that is allowed to be any length. |
| `T`[C] | Constant array. This is an array of `T` that is constant with size C. |
| `T`[A, B, ...] | A dimensional array where `A`, `B`, etc. are constants, but constants are not required to be specified. For example, `int[2, 3]` can be thought of as a 2x3 grid of `int`s. `char[3, , 7]` can be thought of as a 3d array of chars where one dimension is 3 long, the last is 7 long, and the other can be any length. |

Arrays will have their elements be their default values if possible. Note that is not possible to store a dynamic array in a `struct` since they do not have a definite size.

### Other Types
There are some other types that have special uses as well.

| Type | Name | Description |
| ---- | ---- | ----------- |
| `T`& | Reference | A not-null reference to a data type. For now, think of it as a shortcut to already existing data of the same type that always references data. (L-value reference for those familiar with C++). |
| `T`% | Move | Data being transferred that can not be written to (R-value reference for those familiar with C++). |
| `T`* | Pointer | A pointer that can only be used in unsafe contexts. For now, think of it as a shortcut to existing data without any safety mechanisms. This also allows you to do math operations with memory positions (it's for more low-level uses so you most likely don't need it). |

References and moved data do not have a default value and must be initialized immediately. Pointers are `null` by default.

## EASL Types
EASL (Embedded Asylum Standard Library) is a thin layer over Asylum that helps implement the language itself. It is responsible for providing the types below. Note that all EASL types start with upper-case letters as only the primitive types start with lowercase.

### C Compatibility
C uses different types from Asylum, so the below are for C compatibility. EASL *typedef*s, or creates new types that really just shortcut back to the original type:

| Type | C Type | True Type | Range |
| ---- | ------ | --------- | ----------- |
| C_schar_t | signed char | s8 | -128 to 127 |
| C_uchar_t | unsigned char | u8 | 0 to 255 |
| C_char_t | char | - | - | 
| C_short_t | short | s16 | -32,768 to 32,767 |
| C_ushort_t | unsigned short | u16 | 0 to 65,535 |
| C_int_t | int | s32 | -2,147,483,648 to 2,147,483,647 |
| C_uint_t | unsigned int | u32 | 0 to 4,294,967,295 |
| C_long_t | long | s32 | -2,147,483,648 to 2,147,483,647 |
| C_ulong_t | unsigned long | u32 | 0 to 4,294,967,295 |
| C_longlong_t | long long | s64 | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| C_ulonglong_t | unsigned long long | u64 | 0 to 18,446,744,073,709,551,615 |
| C_float_t | float | f32 | 3.4E +/- 38 (7 digits) |
| C_double_t | double | f64 | 1.7E +/- 308 (15 digits) |
| C_ssize_t | ssize_t | int | - |
| C_size_t | size_t | uint | - |
| C_intptr_t | intptr_t | int | - |
| C_uintptr_t | uintptr_t | uint | - |

Despite the fact that this table is final, it is highly recommended to continue using types such as `u16` when the type expected *must* be a 16-bit unsigned number. This helps better communicate the fact that the operation depends on there being 16-bits.

### Smart References
Smart references are used to access data stored somewhere else without having to copy it. With the exception of nullable references, references will always have valid data.

| Type | Name | Description |
| ---- | ---- | ----------- |
| `T`@ | Nullable Reference | Smart reference to a data that may be null. |
| `T`^ | Owning Reference | Smart reference that owns data. When it goes out of scope, the value it owns dies with it. Owning references may only moved to other owning references, never copied. It is not possible for this reference to be null. |
| `T`# | Counted Reference | Smart reference that counts references to data. Copying it to another `T#` will increase the reference count, where moving it leaves the reference count the same. References that are not `T#` do not effect the reference count. It is not possible for this reference to be null. |

All references must be initialized while declared as they have no default value with the exception of the nullable reference, which is `null` by default.

### Vectors
Vectors are shortcuts to tuples of multiple of the same type. For example, a `Vector<float, 4>` will produce a tuple of `4` `float` elements. EASL defines typedefs for commonly used vector types:

| Type | True Type |
| ---- | --------- |
| Vec2 | Vector<float, 2> |
| Vec3 | Vector<float, 3> |
| Vec4 | Vector<float, 4> |
| UVec2 | Vector<uint, 2> |
| UVec3 | Vector<uint, 3> |
| UVec4 | Vector<uint, 4> |
| SVec2 | Vector<int, 2> |
| SVec3 | Vector<int, 3> |
| SVec4 | Vector<int, 4> |

### Matrices
EASL also allows you to declare a matrix with `Matrix<T, U, V>` where `T` is the element type, `U` is the number of rows, and `V` is the number of columns. This means a `Matrix<float, 3, 4>` is a matrix of `float` elements with `3` rows and `4` columns. Note that `T` must have addition, subtraction, multiplication, and division defined with itself. Like vectors, EASL defines some typedefs for commonly used matrix types:

| Type | True Type |
| ---- | --------- |
| Matrix2x2 | Matrix<float, 2, 2> |
| Matrix2x3 | Matrix<float, 2, 3> |
| Matrix3x3 | Matrix<float, 3, 3> |
| Matrix3x4 | Matrix<float, 3, 4> |
| Matrix4x4 | Matrix<float, 4, 4> |
| UMatrix2x2 | Matrix<uint, 2, 2> |
| UMatrix2x3 | Matrix<uint, 2, 3> |
| UMatrix3x3 | Matrix<uint, 3, 3> |
| UMatrix3x4 | Matrix<uint, 3, 4> |
| UMatrix4x4 | Matrix<uint, 4, 4> |
| SMatrix2x2 | Matrix<int, 2, 2> |
| SMatrix2x3 | Matrix<int, 2, 3> |
| SMatrix3x3 | Matrix<int, 3, 3> |
| SMatrix3x4 | Matrix<int, 3, 4> |
| SMatrix4x4 | Matrix<int, 4, 4> |

Unlike `Vector<T, U>`, `Matrix<T, U, V>` is a struct rather than a tuple. Its composure looks like:

```rust
struct Matrix<type T: Add<T> & Sub<T> & Mul<T> & Div<T>, uint U where U > 1, uint V where V > 1> {
    Vector<T, U>[V] rows;
}
```

### Other Types
Some other types that EASL defines:

| Type | Name | Description |
| ---- | ---- | ----------- |
| `T`? | Optional | An optional value. Either contains `T` or `null`. |
| Error<`T`, `U`> | When a success occurs, the value of type `T` will be returned. When an error occurs, the value of type `U` will be used. Note that both are allowed to be `void`. |

Optional is `null` by default. Errors by default are set to an error of the default type of `U` if possible.

## Storage Qualifiers
You're allowed to tag on storage qualifiers to add rules on how data is allowed to be accessed.

| Type | Name | Description |
| ---- | ---- | ----------- |
| readonly/ro `T` | A type `T`, but you can only assign to it once. This is optimal for data you know will definitely not change, and is known at compile time. All parameters of functions are always `ro` (discussed later). |
| writeonly/wo `T` | A type `T`, but you can only write to it and not read from it. This is useful for hardware registers. |
| static `T` | A type `T`, but there is only one instance of it across everything. For example, say you make a `static int` inside a function, and inside this function, this `int` is incremented. Instead of creating a new `int` every time you would call the function and having its value increment to one each time, this will instead act like a counter for every time you call the function as that variable is shared across every time that single function is called. The constructor for the type is only called once initially though. |
| volatile `T` | If you need to make sure `T` has changed, for example you are using it as a condition in a loop, you can mark it as volatile. |
| atomic `T` | Declaring a type as atomic will make sure only one thread is allowed to use it at a time. |

Again, do not be overwhelmed if these do not make sense. They will all be explored in more detail later.

## Typedefs
Asylum creates those above "shortcuts" but doing what is called a typedef statement. These are to be placed outside of functions, structures, and implementations.

```rust
typedef medIntArr = s24[];
```

This example creates a new type called, `medIntArr`, which is really just a dynamic array of 24-bit signed integers in disguise. You can use `medIntArr` and `s24[]` interchangibly, they are the exact same thing.

## Structs
Of course, making other names for existing types is cool and all, but it can only go so far. Structs allow you to group together a bunch of data types like so:

```rust
struct Color {
    byte red;
    byte green;
    byte blue;
}

struct Car {
    str licensePlate;
    Color rgbColor;
    int year;
    str model;
}
```
Now you can pass around more meaningful data!

### Infinite Structs
As you can see above, you can put structs inside of other structs. But one thing that is not allowed is putting a struct inside itself. Why is this a problem? Well let's think about it:

````{warning}
```rust
struct IllegalStruct {
    str name;
    int number;
    IllegalStruct illegalStruct;
}
```
````

Nothing too evil looking, right? But the compiler needs to determine the size of a struct at compile time. So let's try and think of how that happens. We know that Asylum takes care of strings, and trying to figure out its size is a complex topic. But strings do have a fixed length, even if it is a black box to us. Ok, `int`, that's easy, 32 bits or 4 bytes. Ok, and it contains an `IllegalStruct`, so we just give it the length of the `IllegalStruct`. Ok, now we need to go inside that `IllegalStruct` and figure out the length of its `IllegalStruct`... This is the problem. We'll keep trying to figure out the length in an infinite loop. So how do we fix it? If you remember from earlier, we have references which allow us to have a "shortcut" to data:

```rust
struct LegalStruct {
    str name;
    int number;
    LegalStruct^? legalStruct;
}
```
This is legal, since we don't need to know the length of `LegalStruct` to calculate the length of `LegalStruct`. A reference/shortcut always has a fixed length, no matter what type of data it shortcuts/points to. We need to also make the owning reference optional so that we can end the chain of structs when there is no more (or else it'd have to go on forever). Using a reference instead does add some things we will have to do when using it, but this will be discussed later. Also note the following illegal code:

````{warning}
```rust
struct IllegalA {
    str name;
    int number;
    IllegalB illegalB;
}

struct IllegalB {
    str name;
    int number;
    IllegalA illegalA;
}
```
````

Similarly, like a struct containing itself, this will also produce an infinite loop for the compiler that won't be able to determine the length of the structure. This can again be fixed by making either the `illegalA` or `illegalB` members a reference. Note that just having one reference solves the infinite struct error.

## Challenges
1. Explain the difference between unsigned and signed integers. What is a common signed type?
2. Explain the difference between a floating-point and a fixed-point number. What is a common floating-point type?
3. What type of number is `c_long_t`? Is it unsigned, signed, floating-point, or fixed-point?
4. Remember the `add` function from earlier? Remake it so that `a` and `b` can have a decimal point.
5. Create a `struct` that represents a circle. Each circle has a radius, a line width, color, and can have another circle inside of it. Create any other necessary `struct`s.

## Challenge Solutions
* [Solution 1](solutions/dataTypes1.md)
* [Solution 2](solutions/dataTypes2.md)
* [Solution 3](solutions/dataTypes3.md)
* [Solution 4](solutions/dataTypes4.md)
* [Solution 5](solutions/dataTypes5.md)