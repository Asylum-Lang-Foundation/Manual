# Control Flow
Being able to create and manipulate variables is useful, but without the ability to run control flow such as conditions, loops, etc. a program is fairly limited in what it can do.

## If Statements
If statements are the foundation of programming. If it is raining, you bring an umbrella. If you are hungry, you get something to eat. By using conditions in your code, you can have greater control on what your program can do:

```rust
pub fn printItem(int a) {
    if (a == 0)
        println("0");
    else if (a == 1 || a == 2) {
        println("1");
        println("2");
    } else
        println("Other");
        println("Hi!");
}
```
Output (a = 0):
```
0
Hi!
```
Output (a = 1):
```
1
Hi!
```
Output (a = 2):
```
2
Hi!
```
Output (a = 3):
```
Other
Hi!
```

As shown above, the output depends on the input value of `a`. Note that the brackets `{}` are necessary for executing multiple statements. Indentation does not matter, and the statement directly after the `if` is "inside' the `if`. Since the last `println` statement is after the statement after the `if` and is not in brackets, it is outside. Thus, it gets called no matter what the input of `a` is.

## Match
The `match` statement is useful for situations in which you have a lot of various possible values for a variable and have different code actions for each. For example:

```rust
fn magicNumberGuess(int num) -> bool {
    match (num) {
        3:
        7:
            println("I like this number!");
        2:
            println("It's a very basic number.");
        10..=100:
            println("Bigger than 9 and less than 101.");
        3:
        6:
            println("I really do not like this number.");
            return false;
        _:
            println("I have no opinions on this number.");
    }
    return true;
}
```

You are allowed to have multiple conditions for each result (`3` and `7` both lead to `I like this number!` being shown), as well as have multiple statements done as result as the condition. The `_` case will catch anything. Note that the first condition hit is matched, so `I really do not like this number.` will never be shown for `3` and the `_` case should always be done last. Note that this can work for anything, not just numbers:

```rust
fn matchStr(string s) -> int {
    match (s) {
        "Red":
            return 0;
        "Green":
            return 1;
        "Orange":
        "Purple":
            return 2;
        _:
            return -1;
    }
}
```

Also note that it is not required to match every possible value in a `match` statement. We could decide to not match the "default" case with `_`:

```rust
fn matchItem(int num) {
    match (num) {
        3:
            println("You win!");
    }
}
```

## Loops
Sometimes you want to run code multiple times. Sometimes that amount of times depends on an input given. For example, you should wash as many hands as you have. While it may be tempting to hardcode this value to `2`, some other unknown intelligent lifeform may have `4`. First, we must go over the loop statement:

```rust
fn main() {
    loop println("Hello World!");
}
```
Output:
```
Hello World!
Hello World!
Hello World!
Hello World!
Hello World!
[Continues Infinitely]
```

To stop this program, press `Ctrl+C` on your terminal. While looping like this is a cool thing that we are able to do, it does not seem particularly useful if we can not exit the loop. Luckily, `break` exists for this purpose:

```rust
fn main() {
    loop {
        println("Hello World!");
        break;
    }
}
```
Output:
```
Hello World!
```

Remember that we need to put the statements inside a `{}`, otherwise only the first statement will be part of the `loop`. The `break` statement allows us to break out of a loop we are in. The above `break;` is actually shorthand for `break 1;` This does imply you can break out of multiple loops:

```rust
fn main() {
    loop {
        loop {
            loop {
                break 2;
                println("0");
            }
            println("1");
        }
        println("2");
        break 1;
    }
    println("3");
}
```
Output:
```
2
3
```

The `break 2;` breaks out of the first 2 `loop`s, which leaves us to the `println(2);` statement. Then we break out of the final loop. While this breaking structure is neat, it does not allow us to do much, or does it?

```rust
fn count(int endNum) {
    int num = 1;
    loop {
        if (num > endNum) break;
        println(num++);
    }
}

fn main() {
    count(5);
}
```
Output:
```
1
2
3
4
5
```

Now we're getting somewhere! First, we set `num` to `1`. Inside the loop, if `num` is greater than the value of `endNum` we exit the loop. Otherwise, it first prints the value of `num`, increments the value of `num` (adds `1` to it), then jumps to the top of the loop repeating the condition check and print call until it exits.

### While Loops
Surely there must be an easier way than the above code? Luckily, `while` loops automatically check a condition at the top of the loop and `break` if it is not met. The below code accomplishes the same task as the code from earlier:

```rust
fn count(int endNum) {
    int num = 1;
    while (num <= endNum)
        println(num++);
}

fn main() {
    count(5);
}
```
Output:
```
1
2
3
4
5
```

It's important to note that the condition runs at *the top* of the loop, not for every statement in the loop:

```rust
fn count(int endNum) {
    int num = 1;
    while (num <= endNum) {
        println(num++);
        println(num++);
    }
}

fn main() {
    count(5);
}
```
Output:
```
1
2
3
4
5
6
```

Since the value of `num` is only checked at the top of the loop, even though `num` becomes `6` during the course of the loop, it is still printed anyways.

### Do While Loops
What if you wanted to run a condition at the end of a loop rather than the beginning? For this purpose, you can use a do while loop:

```rust
fn count(int endNum) {
    int num = 1;
    do {
        println(num++);
    } while (num <= 5);
}

fn main() {
    count(5);
    count(0);
}
```
Output:
```
1
2
3
4
5
6
1
```

Note that a do while loop guarantees the code in the loop will be executed at least once.

### For Loops
It's annoying that we have to increment `num` ourselves and declare it outside the loop. With `for` loops, this problem is taken care for us:

```rust
fn count(int endNum) {
    for (int i = 1; i <= endNum; i++)
        println(i);
}

fn main() {
    count(5);
}
```
Output:
```
1
2
3
4
5
```

The first part of the `for` loop has an expression that is done once before the loop. The next expression is the condition to check before each loop. The loop only continues if this condition succeeds. Finally, there is an expression to run at the end of the loop. Here, the variable `i` is incremented. It is common practice for `for` loop iterators to be `i`, `j`, or `k`.

#### Iterating
You may also use a `for` loop for iterating over an iterable list. You may iterate over anything that implements `Iterable`:

```rust
fn printListItems(List<int> items) {
    for (int item : items)
        println(item);
}
```

The above code will print all of the items in the given list. You can also iterate using the `..` (`Range`) or `..=` (`RangeEq`) operator:

```rust
fn main() {
    for (int i : 1..5)
        println(i);
    for (int i : 1..=5)
        println(i);
}
```
Output:
```
1
2
3
4
1
2
3
4
5
```

### Do For Loops
Like the do while loops, do for loops run at least once:

```rust
fn count(int endNum) {
    do
        println(i);
    for (int i = 1; i <= endNum; i++)
}

fn main() {
    count(5);
    count(0);
}
```
Output:
```
1
2
3
4
5
6
1
```

It's important to note that the condition is checked *before* the final expression is ran. In the example above, since `i` is `5` it passes the condition of being less than or equal to `endNum` (`endNum` is `5`). This means the loop goes for another iteration, allowing `6` to be printed. Note that you may not loop over iterators as done in `for` loops.

### Continue
Continues allow you to jump to the end of a loop:

```rust
fn main() {
    for (int a = 0; a < 6; a++) {
        if (a == 4)
            continue;
        println(a);
    }
}
```
Output:
```
0
1
2
3
5
```

When `a` is `4`, we jump to the end of the `for` loop. At this point we increment `a`, then jump to the top of the loop where we check the condition `a < 6`. You are also allowed to continue out of multiple loops like with `break`:

```rust
fn main() {
    for (int a = 0; a < 2; a++) {
        for (int b = 0; b < 3; b++) {
            if (b == 2)
                continue 2;
            println(a + ", " + b);
        }
    }
}
```
Output:
```
0, 0
0, 1
1, 0
1, 1
```

## Recursion Recursion Recursion
One method of control flow that uses function calls rather than loops is called recursion, in which a function calls itself. For example:

```rust
fn fib(int n) {
    if (n <= 0)
        return 0;
    else if (n == 1)
        return 1;
    else
        return fib(n - 1) + fib(n - 2);
}
```

The above calculates the Fibonacci number for number `n`. Note that for large values of `n`, this will grow the call stack a large amount. In order to avoid this, tail recursion can be used. Tail recursion is when the last thing done by the function is calling itself. This means we can replace the call stack with the new one, therefore avoiding growing the stack:

```rust
fn fib(int n, int prev = 0, int curr = 1) {
    if (n == 0)
        return prev;
    else if (n == 1)
        return curr;
    else
        return fib(n - 1, curr, prev + curr);
}
```

In the above example, either a value is returned or the current call stack is replaced with a new one. This is different from the first recursive example, as we don't need to wait for the result of the function call to do calculations. The result *is* the new function call.