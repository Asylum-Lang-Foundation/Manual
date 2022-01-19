# Functions
In just about any programming language, functions are heavily utilized. The job of a function is to execute code depending on input parameters, and produce any amount of results from it. Functions can also call other functions and execute the code within them. You saw this earlier by calling `println`, which told the computer to execute printing code to whatever you gave it. We can also have values be returned like so:

```rust
fn main() {
    println(add(3, 7));
    println(add(2, add(3, 2)));
    println(add(2, 1));
    add(1, 4);          // Notice that this last one is not printed!
}

fn add(int a, int b) -> int => a + b;
```
Output:
```
10
7
3
```
```{note}
`add` was able to be called before it was defined. This is something you can do in Asylum that is not possible in some other low-level languages.
```
```{note}
As you can see, the result of calling `add` can just be directly put into a `println` call. As long as the input you feed into a function is the same type of what it wants, you can do this with any function.
```
```{note}
Remember that `=>` can be used for functions with only one code statement!
```

## Return Types
In Asylum, you can denote what a function returns by the `->` operator, which can be thought of as "returns". So in this case, `main` doesn't return anything (as it doesn't have `->`), or returns *void*. `add` returns an `int`, as it is right after the `->`. But what is an `int`? It will be talked about later in the types section, but for now think of it as a whole number (no decimal places).

## Function Parameters
A function can be given parameters by having a comma-separated list of data types with names. The `add` function above has two parameters, an `int` called `a`, and another `int` called `b`. Of course, the data types of these parameters do not have to be the same. You call a function by separating the input parameters to it by commas. You must call the function with the same number of parameters. For example, `add(5);` is invalid.

## Different Ways To Return
Asylum allows you to return a value in 3 different ways:
* In a one line statement for the function using `=>`:
```rust
fn add(int a, int b) -> int => a + b;
```
* Using the return statement:
```rust
fn add(int a, int b) {
    return a + b;
}
```
* A statement without a semi-colon:
```rust
fn add(int a, int b) {
    a + b
}
```
These all produce the same code!
````{warning}
The following examples are invalid:
```rust
/*  
    "=>" Can only be used with an expression.
    "a + b" is an expression that evaluates to "int", but "return a + b" is a code statement.
*/
fn add(int a, int b) -> int => return a + b;
```
```rust
fn add(int a, int b) -> int {
    // Like above, "return a + b" is a code statement, not an expression, so it can not be returned.
    return a + b
}
```
````

## Multiple Function Definitions
You can actually define the same function multiple times, as long as its signature is not the same as another function with the same name. The signature of two functions are considered equivalent if all of the following conditions are met:
* Have the same number of parameters.
* Have the same return type.
* Parameter 1 in function 1 has the same data type as parameter 1 in function 2, parameter 2 in function 1 has the same data type as parameter 2 in function 1, etc.
The following is valid Asylum code:
```rust
fn add(int a, int b) -> int => a + b;
fn add(int a, int b, int c) -> int => a + b + c;
fn add(int a, int b) {}
```
The compiler will know which function to call depending on the context on which it is used.

## Challenges
1. Adding is cool and all, but subtraction is where it is at! Make a function called `sub` that subtracts `int b` from `int a`, this returns an `int`.
2. The more numbers the merrier! Make another function that computes `a - b - c` and another that computes `a - b - c - d`. Create them in such a way that all your functions return in a unique way.
3. Let's see some results! Make a program to print the results of calling these new functions. Comment on the top of each `println` statement what you expect the value to be.

## Challenge Solutions
* [Solution 1](solutions/functions1.md)
* [Solution 2](solutions/functions2.md)
* [Solution 3](solutions/functions3.md)

## Disclaimers
```{warning}
StraitJacket currently is not smart enough to understand function overloading. The code to do this is not implemented.
```
```{warning}
StraitJacket can't use the `+` operator to add numbers at the moment. You have to use the LLVM ASM call `(int)llvm("add", a, b)` instead.
```