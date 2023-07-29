# Pointers
Now that you understand how references work, it is time to discuss a topic more in-line with what low-level C programmers expect: pointers. Do note that pointer-related functions are unecessary in the vast majority of Asylum applications. These below operations should only be used where appropriate. Note that any function that utilizes pointers need to be marked as `unsafe` to signify you know what you are doing and let the compiler continue compiling the code.

```rust
unsafe fn main() {
    int x = 5;
    int* y = &x;
    int* z -> x;
    println(y == z);
    *y = 7;
    *z = 3;
    println(x);
}
```
Output:
```
true
3
```

The above code is an example that showcases the use of pointers. Let's walk through it line by line:
1. An `int` named `x` is instantiated with the value `5`.
2. A pointer to an `int` named `y` is set to be set to the "address of" `x`.
3. A pointer to an `int` named `z` is set to point to `x`.
4. Tests if `y` and `z` point to the same thing.
5. The value `y` points to is set to `7`.
6. The value `z` points to is set to `3`.
7. Print the value of `x`.

Since `y` and `z` point to the same thing (the variable `x`), setting the value `y` and `z` point to set the value of `x` as well.

## Variables Overview
For the sake of simplicity, let there be known two types of values: L-values and R-values. The names of the values come from the fact that L-values usually are on the left side of the `=` and R-values are on the right side of the `=`. This is an incorrect way of thinking about these. Think of L-values as values you can "load from/store to" and R-values are values you can "read from". Let us look at an example:

```rust
int x = 3;
x = x + 2;
int y = x;
println(y + 1);
```

1. The variable `x` is an L-value because we can store to it. You can not give a value to the number `3`, so it must be an R-value.
2. We know that all variables are L-values, so `x` on the left hand side must be an L-value. Though note when we add `x` with `2` we need to get the value `x` contains somehow. This is done by converting the `x` on the right to an R-value. It does this by loading the value contained in `x` (in this case `3`), adds it to `2`, then stores the result back into `x`.
3. The variable `y` is an L-value because all variables are L-values. When we say `y = x`, we really mean that the value of `y` should be set to the value of `x`. This is done by converting `x` into an R-value by loading from it, and then storing it into `y`.
4. All function calls expect R-values. Luckily, `y + 1` is an R-value as first the L-value `y` is converted to an R-value and then 1 is added to it.

### Graphic
The above may be confusing, and that is a perfectly natural feeling. L-values and R-values can be hard to get an exact feel for. Consider the below tables:

`int x = 3;`:
| Code | Location | Value |
| ---- | -------- | ----- |
| 3 | - | 3 |
| x | 0x5af5b570 | 3 |
1. Number `3` is an R-value since it has no location in memory.
2. Variable `x` is an L-value since we can store to it, and store the `3` to it.

`x = x + 2;`
| Code | Location | Value |
| ---- | -------- | ----- |
| x | 0x5af5b570 | 3 |
| 2 | - | 2 |
| 3 + 2 | - | 5 |
| x | 0x5af5b570 | 5 |
1. We need to get the value from `x`, and since it as an L-value (has a location in memory) we can extract the value `3` from it.
2. The constant `2` is an R-value since it has no location in memory.
3. Add the two numbers together, the R-value loaded from L-value `x` and the `2` previously.
4. The new value of `5` is stored back into L-value `x`.

`int y = x;`:
| Code | Location | Value |
| ---- | -------- | ----- |
| x | 0x5af5b570 | 5 |
| 5 | - | 5 |
| y | 0x5af5b578 | 5 |
1. R-value `5` is loaded from L-value `x`.
2. The `5` loaded has no operation done on it.
3. The `5` is stored into L-value `y`.

`println(y + 1);`
| Code | Location | Value |
| ---- | -------- | ----- |
| y | 0x5af5b578 | 5 |
| 1 | - | 1 |
| 5 + 1 | - | 6 |
| println | 0x10230fc8 | code |
| println(6) | - | 2 |
1. R-value `5` is loaded from L-value `y`.
2. R-value `1` is summoned into existence.
3. R-values `5` and `1` are added.
4. L-value `println` is resolved. Functions are still variables that live in memory, though usually in static memory. When they are converted to R-values, they return "code". Of course there is no type to store this data, nor can you change the value of a function.
5. Code from above is ran with `6` as a parameter. It returns R-value `2`, the number of characters printed.

As you may have noticed, L-values have a location in memory (either static, stack, or heap) and have values, where as R-values are pure values that do not live anywhere in memory. All arithmetic operations are done with R-values, where as you can only store to L-values.

The memory addresses given above are possible addresses that may occur, though your results will most likely vary heavily.

## Address-Of Operator
Now that you understand L-values and R-values, we can talk about the address-of operator `&`. Variables, as discussed, are L-values and so are locations in memory that hold values. But what if we wanted to get the location of a value rather than the value? That is what the address of operator is for:

```rust
int x = 3;
int* y = &x;
println(y);
```
Output:
```
0x5af5b574
```

| Code | Location | Value |
| ---- | -------- | ----- |
| x | 0x5af5b570 | 3 |
| y | 0x5af5b578 | 0x5af5b570 |

For demonstration purposes, this address of `x` matches the location in memory assigned to it earlier. Remember, we normally convert an L-value to an R-value by loading the value at an address. But all the address-of operator does is treat an L-value as an R-value. This allows us to get the memory address of the variable!

````{warning}
The following ways of using the `&` operator are invalid:
```rust
int x = 3;
int* y = &3;
int** z = &&x;
```
1. It is not possible to get the address of an R-value. Note that since functions are technically L-values, you can get the address of them.
2. While it may be tempting that `&&` would give an address of an address, it's important to realize that `&` outputs an R-value. This means it is not possible to get the address of it.
````

## Dereference Operator
Consider the opposite of getting the address of a variable. What if we wanted to treat a variable's value as a location to set an item at. We can do this to R-values using the dereference operator:

```rust
int x = 3;
int* y = &x;
*y = 5;
println(x);
println(*y);
```
Output:
```
5
5
```

| Code | Location | Value |
| ---- | -------- | ----- |
| x | 0x5af5b570 | 3 |
| y | 0x5af5b578 | 0x5af5b570 |
| x | 0x5af5b570 | 5 |

1. We make a variable of type `int` with a value of `3`.
2. Variable `y` is set to be the address of `x` in memory.
3. The value of y is treated as a location, and the value at that (variable `x`) is set to `5`.

A more technical explanation is that `y` is an L-value, but `*` only operates on R-values. This means `y` is converted to an R-value first by loading it, which gives the address of `x` (`0x5af5b570`). Note though that we can not store to an R-value, so we have to treat it as an L-value. This is done by using `*`. Now that `0x5af5b570` is treated as an L-value, we are allowed to store `5` to it (as if we were storing to `x`). Note that if we print the `0x5af5b570` L-value instead, it must be converted to an R-value first (as all function arguments must be). This means the value gets loaded from it to get an R-value, which is the value at `x` or `5`!

## Points-To Operator
A shorthand for making pointers is the points-to operator `->`. This is used for creating and setting pointers:

```rust
int x = 3;
int* y -> x; // Equivalent to int* y = &x;
int z = 5;
y -> z; // It is possible to re-assign pointers to point to other variables.
y = &x;
```

## Structures
You can make pointers to structures and access members with the `.` operator like usual. C programmers will note that they typically need to do `(*myStruct).myMember` or `myStruct->myMember`, but the `->` operator has a different meaning in Asylum. Thus, the `.` operator will dereference automatically:

```rust
struct MyStruct {
pub:
    int myMember;
}

unsafe fn main() {
    MyStruct data1;
    MyStruct* data2 = new MyStruct();
    MyStruct** data3 -> data2;
    data1.myMember = 3;
    data2.myMember = 5; // Same as line above!
    data3.myMember = 7; // Also same as line above.
    println(data1.myMember);
    println(data2.myMember);
    delete data2;
}
```
Output:
```
3
7
```

Since a pointer can not hold any data other than an address, there is no hard in having the `.` act for acessing members for both values and pointers. The `.` operator will dereference until it reaches the member desired.

## Pointer Math
What pointers increment by depends on the size of what is being pointed to. For example:

```rust
unsafe fn main() {
    int a = 3;
    int* b -> a;
    println(sizeof(a));
    println(b);
    println(++b);
    println(b - 2);
}
```
Output:
```
4
0x57910390
0x57910394
0x5791038c
```

As shown above, incrementing a pointer adds the size of what it points to to the pointer (adds `4`). Subtracting `2` thus subtracts `2 * 4 = 8`. Operations such as multiplication and division on pointers is not defined.

````{warning}
It is easy to produce an invalid pointer while using pointer math. Please only use it when it is appropriate for the task. This should *never* be used for general-purpose applications.
````

### Arrays
One of the uses of pointer math is iterating through an array. For example:

```rust
unsafe fn printItems(int[] items, int numToPrint) {
    int* curr = items;
    int* end = items + numToPrint;
    while (curr < end) {
        println(*curr);
        curr++;
    }
}

fn main() {
    int[3] arr = { 3, 5, 7 };
    printItems(arr, 3);
}
```
Output:
```
3
5
7
```

````{warning}
The above is just an example of how pointer math can be used. It is not recommended to iterate through arrays like it is done here. Please use pointers and pointer math at your own risk.
````

## Challenges
TODO!!!

## Challenge Solutions
TODO!!!