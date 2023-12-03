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

## Dynamic Arrays