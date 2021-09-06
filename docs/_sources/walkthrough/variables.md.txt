# Variables
On the previous page, you learned about data types. They are an essential first half to learning one of the fundamental concepts in any programming language: variables.

## Why Use Variables?
When programming, you typically need to store information/data in places. A variable gives you a way to pass and edit this information around, whether it is numbers, text, etc.

## Using A Variable
How you use a variable depends on what it is of course. But let's look at an example that utilizes variables:

```rust
fn main() {
    string myText;
    myText = "Hello World!";
    string myOtherText = "This is more text!";
    println(myText);
    println(myOtherText);
}
```
Output:
```
Hello World!
This is more text!
```

### Understanding The Code
* We declare a variable (named "myText") by specifying its type and then the name of what we will call the variable. Here, we are saying that there is a variable called "myText" and it "is a" `string`.
* Using an assignment operator such as `=`, we give the variable a value.
* It is also possible to declare and assign a variable at the same time.
* After this, we could simply print out those variables.

And that is how you declare and use basic variables. You should not think of `=` as the math equals, but rather you are assigning a value to this variable at this point in time.

## Operators
What's cool about variables is the fact that you can perform operations on them to either manipulate current variables, or form new values entirely. For example, you can add two number variables together to produce a new number, which you could store in a new variable, store it to an existing one, or even not store it at all and throw the result out of the window.

### General Operators
These operators can be applied to any variable(s)/values to produce a new value. These operators are "static", as they do not modify the values of the variables they interact with.

| Operator | Name | Usage | Description |
| -------- | ---- | ----- | ----------- |
| + | Add | a + b | Returns the sum of two items |
| - | Sub | a - b | Returns the difference of two items |
| * | Mul | a * b | Returns the product of two items |
| / | Div | a / b | Returns the result of division of two items |
| % | Mod | a % b | Returns the remainder of the result of the division of two items |
| .. | Range | a .. b | Returns an interable item that is defined by the range between `a` and `b` |
| & | BitAnd | a & b | Returns the bitwise and operation of two items |
| \| | Or | a \| b | Returns the bitwise or operation of two items |
| ^ | BitXor | a ^ b | Returns the bitwise exclusive or operation of two items |
| ~ | BitNot | ~a | Returns the bitwise not of an item |
| << | Lshift | a << b | Shifts `a` to the left by `b` bits |
| >> | Rshift | a >> b | Shifts `a` to the right by `b` bits |
| + | Pos | +a | Modifies a value with positive (does nothing) |
| - | Neg | -a | Modifies a value to be negative |
| ++ | Inc | ++a; a++ | Increment the value of a |
| -- | Dec | --a; a-- | Decrement the value of a |
| ^ | Member | ^a | When indexing an element, will access the member that is the count minus `a` |
| * | Dereference | *a | Given that `a` is a double-pointer, this will return the value the double-pointer points to |
| & | Address Of | &a | Returns a raw, unsafe pointer that points to `a` |
| @ | As Address | @a | Given that `a` is a pointer, `@a` will return the value of the pointer instead of returning what `a` points to |
| && | And | a && b | Returns `true` if, and only if, both `a` and `b` are `true` |
| \|\| | Or | a \|\| b | Returns `true` if, and only if, either `a` and `b` are `true` |
| !& | Nand | !& | a !& b | Returns `true` if, and only if, both `a` and `b` are `false` |
| !\| | Nor | !\| | a !\| b | Returns `true` if, and only if, neither `a` or `b` are `true` |
| ! | Not | !a | Returns `true` if, and only if, `a` is `false` |
| == | Eq | a == b | Returns `true` if, and only if, `a` is equal to `b` |
| != | Neq | a != b | Returns `true` if, and only if, `a` is not equal to `b` |
| > | Gt | a > b | Returns `true` if, and only if, `a` is greater than `b` |
| < | Lt | a < b | Returns `true` if, and only if, `a` is less than `b` |
| >= | Ge | a >= b | Returns `true` if, and only if, `a` is greater than or equal to `b` |
| <= | Le | a <= b | Returns `true` if, and only if, `a` is less than or equal to `b` |
| ?: | Cond | a ? b : c | If `a` is `true` return `b`, else, return `c` |
| ?? | Null | a ?? b | If `a` is a null pointer, return `b`, else, return `a` |
| , | Comma | a, b, ... | Form a tuple with `a` in the first element, `b` in the second, etc. Expressions that result in `void` are NOT included |

### Assignment Operators
These operators are "not static", as in they modify the value of the left-hand variable. All operators are in the form `a {OP} b`.
| Operator | Equivalent |
| -------- | ---------- |
| += | a = a + b |
| -= | a = a - b |
| *= | a = a * b |
| **= | Raises `a` to the `b`th power |
| /= | a = a / b |
| %= | a = a % b |
| &= | a = a & b |
| \|= | a = a \| b |
| ^= | a = a ^ b |
| ~= | Flips the bits in `a` specified by `b` |
| <<= | a = a << b |
| >>= | a = a >> b |
| &&= | a = a && b |
| \|\|= | a = a \|\| b |
| !&= | a = a !& b |
| ~\|= | a = a !\| b |
| ??= | a = a ?? b |

### Operator Precedence
You may have remembered in elementary school learning PEMDAS, or the order of operations (parenthesis, exponents, multiply/divide, addition/subtraction). Asylum also obeys an order of operations, the higher on the table, the higher the precedence (same row is on the same level):

| Operators |
| --------- |
| x++, x-- |
| +x, -x, !x, ~x, ++x, --x, ^x, &x, *x, @x |
| x..y |
| x * y, x / y, x % y |
| x + y, x - y |
| x >> y, x << y |
| x > y, x < y, x >= y, x <= y |
| x == y, x != y |
| x & y |
| x ^ y |
| x \| y |
| x && y, x !& y, x !\| y |
| x \|\| y |
| x ?? y |
| x ? y : z |
| x, y |
| Assignment Operators |

Remember to use parenthesis when in doubt! It makes code easier to read.

### Preserving Data With Operators
What happens when you use these different operators on differing on similar types? For example, let's look at the following code:

```rust
fn main() {
    println(5 / 2);
    println(5.0 / 2);
}
```
Output:
```
2
2.5
```

What just happened? Well it turns out that since `5` and `2` are both integers, the result must be an integer. This led to the decimal portion being completely ignored! But `5.0` is a floating-point number. Even though `2` is a decimal number, since an operation is being done with an integer and a floating-poing number, the computer will calculate the result that involves losing less information. So for example, an `s32` (`int`) added with a `s64` (`long`) will produce an `s64` (`long`) as it loses less information. Despite the technicalities of floating-point numbers, a floating-point or fixed-point number is always said to be "bigger".

## A More Complex Example
As you can tell, there are quite a lot of different operators. Each will be discussed in more detail later, but for now, let's see an example that utilizes some:

```rust
// Here we have a function that solves for x = (-b +/- sqrt(b^2 - 4ac)) / 2a, which is better known as the standard quadratic formula.
// It is also worth noting that we are allowed to return multiple values at once in a tuple as shown:
fn quadraticFormula(double a, double b, double c) -> double, double {
    double innerPart = b * b - 4 * a * c;           // Calculate the interior part of the square root.
    innerPart **= 0.5;                              // Apply the square root by raising to the .5th power.
    -b + innerPart, -b - innerPart                  // Return the final results (the comma creates a tuple that is returned).
}
```

## Struct Variables
Remember structs from earlier? What if we actually wanted to use them? We can define a variable that "is" our struct like we define any other variable. We could then access the members of our `struct` by using a `.`.

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

fn main() {
    Car car;
    car.licensePlate = "TOY1234";
    car.rgbColor.red = car.rgbColor.green = 70; // You can set multiple variables to the same value too!
    car.year = 2017;
}
```

But what happens to the variables we did not set? In Asylum, the rule of thumb is that everything is inialized to either be `0` or empty:

```rust
println(car.model);
println(car.rgbColor.blue);
```
Output:
```

0
```

The first line is an empty line if that is not clear.

### Initialization
Of course, setting the initial variables the above way is a little messy. Luckily, there is a shorthand using curly brackets for such:

```rust
fn main() {
    Car car = Car {
        licensePlate: "TOY1234",
        rgbColor: Color { red: 70, green: 70 },
        year: 2017
    };
}
```

This above code is equivalent, and much cleaner.

## Challenges
1. Make a program that prints the sum of two variables to produce "Hello World!".
2. Initialize the `Color` struct in both ways to both produce white (white is 255).
3. Create a function to solve for `c` in the Pythagorean Theorem (`a^2 + b^2 = c^2`).
4. What is the result of `8 / 3`?
5. What is the result of `4 * (2 + 1) % 3`?

## Challenge Solutions
* [Solution 1](solutions/variables1.md)
* [Solution 2](solutions/variables2.md)
* [Solution 3](solutions/variables3.md)
* [Solution 4](solutions/variables4.md)
* [Solution 5](solutions/variables5.md)