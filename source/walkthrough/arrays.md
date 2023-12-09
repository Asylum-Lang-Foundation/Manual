# Arrays
Arrays are groups of the same type that have a fixed size and can not be changed. Below is a simple example of how arrays may be used:

```rust
fn main()
{
    int[4] arr;
    arr[0] = 3;
    arr[1] = 5;
    arr[-1] = 4;
    arr[-3] = 2;
    println(+arr);
    println(-arr);
    println(arr);
    for (int (int: item, int: index) : arr) {
        println(item);
    }
}
```
Output:
```
3
-4
[3, 2, 0, 4]
3
2
0
4
```

There are some interesting things to note:
* Indexing starts from `0`, as in `0` is the first element of the array, `1` is the second, etc.
* Reverse indexing starts from `-1`, as in `-` is the last element of the array, `-2` is the second last, etc.
* Using the `+` (Positive) operator will output the highest possible index of the array. If there are no elements, this will be `-1`.
* Using the `-` (Negative) operator will output the lowest possible negative index of the array. If there are no elements, this will be `0`.
* Any array that has an index higher or lower than the range of `int` is invalid. This means that the maximum size of an array is the highest value of `int` plus `1`.
* Accessing an invalid index will abort the program and dump a stack trace in debug mode but produce undefined behavior in release mode.

## Iterating Over An Array
Iterating over an array can be done either with an iterating for loop or a regular for loop:

```rust
fn main()
{
    int[3] arr = { 7, 8, 9 };
    println("--Forward Iteration--");
    for ((int: item, int: index) : arr) {
        println("{index}: {item}");
    }
    println("--Backward Iteration--");
    for ((int: item, int: index) : ~arr) {
        println("{index}: {item}");
    }
    println("--Forward Index Iteration--");
    for (int i = 0; i <= +arr; i++) {
        println("{i}: {arr[i]}");
    }
    println("--Backward Index Iteration--");
    for (int i = -1; i >= -arr; i--) {
        println("{i}: {arr[i]}");
    }
}
```
Output:
```
--Forward Iteration--
0: 7
1: 8
2: 9
--Backward Iteration--
-1: 9
-2: 8
-3: 7
--Forward Index Iteration--
0: 7
1: 8
2: 9
--Backward Index Iteration--
-1: 9
-2: 8
-3: 7
```

Note that using `~` from an array provides a backwards iterator rather than a forwards iterator. You can also ration that in the case that `arr` is empty, the code runs without error as `+arr` would return `-1`, and `0` is not less than or equal to `-1` so the first forward iteration would not happen. Likewise, `-arr` would return `0`, and `-1` is not greater than or equal to `0` so the first backwards iteration would not happen either. Overall, it is recommended to use the iterators rather than indexing the array manually as the setup for iterating over an array can be tricky.

## Dimensional Arrays
The above example is for 1-dimensional arrays. However, there is such thing as 2 dimensional arrays, 3 dimensional arrays, and so on. This is done by separating dimensions with a comma. For example:

```rust
fn main()
{
    int[2, 3] arr = {
        {0, 1, 2},
        {3, 4, 5}
    }; // An MxN array where M is 2 and N is 3.
    println(arr[1, 2]);
    for ((int[]: row, int: y) : arr) {
        for ((int: item, int: x) : row) {
            println("{y}, {x}: {item}");
        }
    }
    println(+arr);
    println(-arr);
}
```
Output:
```
5
0, 0: 0
0, 1: 1
0, 2: 2
1, 0: 3
1, 1: 4
1, 2: 5
(1, 2)
(-2, -3)
```

Note that since the array is multidimensional, getting the index maxes of it returns a tuple of `(int, int)`. Below is an example of using 3 dimensions:

```rust
fn main()
{
    int[4, 2, 3] arr = {
        { { 0, 1, 2 }, { 3, 4, 5} },
        { { 6, 7, 8 }, { 9, 10, 11} },
        { { 12, 13, 14 }, { 15, 16, 17} },
        { { 18, 19, 20 }, { 21, 22, 23} }
    };
    println(arr[3, 1, 0]);
    for ((int[,]: xy, int: zIndex) : arr) {
        for ((int[]: x, int yIndex) : xy) {
            for ((int: item, int: xIndex) : x) {
                println("{zIndex}, {yIndex}, {xIndex}: {item}");
            }
        }
    }
    println(+arr);
    println(-arr);
}
```
Output:
```
15
0, 0, 0: 0
0, 0, 1: 1
0, 0, 2: 2
0, 1, 0: 3
0, 1, 1: 4
0, 1, 2: 5
1, 0, 0: 6
1, 0, 1: 7
1, 0, 2: 8
1, 1, 0: 9
1, 1, 1: 10
1, 1, 2: 11
2, 0, 0: 12
2, 0, 1: 13
2, 0, 2: 14
2, 1, 0: 15
2, 1, 1: 16
2, 1, 2: 17
3, 0, 0: 18
3, 0, 1: 19
3, 0, 2: 20
3, 1, 0: 21
3, 1, 1: 22
3, 1, 2: 23
(3, 1, 2)
(-4, -2, -3)
```

## Arrays Of Arrays
Of course, it is possible to use arrays of arrays rather than multidimentional arrays as well:

```rust
fn main()
{
    int[2][3] arr = {
        {0, 1},
        {2, 3},
        {4, 5}
    }; // arr is an array of arrays that are size 2.
    println(arr[1][0]);
    for ((int[]: row, int: y) : arr) {
        for ((int: item, int: x) : row) {
            println("{y}, {x}: {item}");
        }
    }
    println(+arr);
    println(+arr[0]);
}
```
Output:
```
2
0, 0: 0
0, 1: 1
1, 0: 2
1, 1: 3
2, 0: 4
2, 1: 5
3
2
```

Notice that since it is a singular dimension array, getting the highest index of it returns only a single value. It also may be confusing that despite `arr` being declared as `int[2][3]`, we have a max index of `arr[2][1]`! This is because types in Asylum are read from left to right (so we have an `int` array of size `2` that is in an array of size `3`). So when indexing, we are first indexing the array of size `3`. It is recommended to use multi-dimensional arrays to avoid this confusion.

## Dynamic Arrays
The size of an array is not always known. Dynamic arrays allow you to handle this case. When initialized, unless the size is determinable at compile time, it is heap-allocated or uses whatever the current allocator is. If the size is determinable at compile time, then it is stack allocated. Dynamic arrays are especially useful for passing an array to a function, as the size of the array may not be known:

```rust
fn printItems(int[] arr)
{
    for (int item : arr) {
        println(item);
    }
}

fn main()
{
    int[3] arr = { 3, 2, 1 };
    printItems(arr);
}
```
Output:
```
3
2
1
```