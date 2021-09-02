# First Program (Hello World)
Ah, the hello world. The first program that one makes in a new language. The sanity check that all is well. In Asylum, making such a program is relatively straightforward:

```rust
fn main() {
    println("Hello World!");
}
```
Output:
```
Hello World!
```

## Understanding The Program
That's it. Now, let's understand what this syntax means:

* When you run an Asylum program, it will run the *function* called *main*.
* We declare a *function* by using the keyword `fn`.
* After that, we give it the name of our function, which in this case is *main*.
* After this, we have an empty pair of parenthesis, `()`. We will use this later to give functions *arguments*, but do not worry about this for now.
* We put any code statments we want to run inside the function within `{}`.
* `println` is a function that will print whatever text you feed it within `()` followed by a new line. We do a function call by giving the name of the function, and putting what arguments we want to feed it within `()`. We also tell the compiler that the code statement is over by using `;`.
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

## Disclaimers
```{warning}
StraitJacket currently does not have a `println` or `print` function usable at the moment due to it being part of the incomplete Embedded Asylum Standard Library (EASL). For now, just declare an `extern fn printf(string format, ... args) -> int;` at the top of the file and use that instead. This will function as the *print* function.
```