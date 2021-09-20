# First Program (Hello World)
Ah, the hello world. The first program that one makes in a new language. The sanity check that all is well. In Asylum, making such a program is relatively straightforward:

```rust
println("Hello World!");
```
Output:
```
Hello World!
```

## Understanding The Program
That's it. Now, let's understand what this syntax means:

* `println` is a function that prints what you give it, then goes to the next line. You call a function by putting its parameters between `(` and `)`, with the parameters being comma-separated. You can see here that we are telling it to print `Hello World!`. We must wrap this value in `"`s so the compiler knows that it's a value that we are sending it.
* There is a semicolon `;` to signal to the compiler that the statement is over.

Notice that statements outside a function are called *top-level statements*. These are only allowed to be in one of the files you are compiling, as the compiler wouldn't know where your program would start otherwise. Normally, we put code inside of functions to be even more organized:

```rust
fn main() {
    println("Hello World!");
}
```
Output:
```
Hello World!
```

## Understanding The Function Syntax
That's it. Now, let's understand what this syntax means:

* When you run an Asylum program, it will run the *function* called *main* (or top-level statements in one file if *main* doesn't exist).
* We declare a *function* by using the keyword `fn`.
* After that, we give it the name of our function, which in this case is *main*.
* After this, we have an empty pair of parenthesis, `()`. We will use this later to give functions *arguments*, but do not worry about this for now.
* We put any code statments we want to run inside the function within `{}`.
* If this is confusing, don't worry, we will talk about a lot of this individually later!

## A Cool Trick
Asylum also lets you do a shortcut for any function that is only one expression:
```rust
fn main() => println("Hello World!");
```
Output:
```
Hello World!
```
The same output is produced. For now, you can think of `{}` as a group of many code statements, where `=>` will just return one expression.

```{warning}
    `=>` must be used in combination with an expression. A call to `println` is allowed, as function calls are part of an expression. However, assignments to a variable such as `myVar = 2` is not an expression, it is a statement.
```

## Print Function
While `println` exists, `print` also exists too? So what's the difference? `println` will put a newline after it prints what you give it, while `print` will not. For example:

```rust
fn main() {
    println("1");
    println("2");
    print("3");
    print("4");
    println("5")
    print("6")
    println("7");
}
```
Output:
```
1
2
345
67
```

## Comments
Comments are not read by the compiler, and are also very important to documenting your code:
```rust
// This is a single line comment.
/*
    This is a mult-line comment.
    Anything within the bouncers is not read by the compiler.
*/
```

## Challenges
1. Write a program that says "Hello Asylum User!" in one line of code two different ways.
2. Write a program that prints the first 3 letters of the alphabet together, but you are only allowed to print one letter at a time.

## Challenge Solutions
* [Solution 1](solutions/helloWorld1.md)
* [Solution 2](solutions/helloWorld2.md)

## Disclaimers
```{warning}
StraitJacket currently does not have a `println` or `print` function usable at the moment due to it being part of the incomplete Embedded Asylum Standard Library (EASL). For now, just declare an `extern fn printf(string format, ... args) -> int;` at the top of the file and use that instead. This will function as the *print* function.
```