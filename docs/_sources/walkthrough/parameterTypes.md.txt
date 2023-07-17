# Parameter Types
In Asylum, there are different ways to pass data to functions. In the previous section, we explored how to create functions that take parameters and return a result that utilized those parameters. However, consider the below code:

````{warning}
This code will not compile:
```rust
fn setValue(int val) {
    val = 7; // Error, can not set value of an input parameter!
}
```
````

The compiler sees that `val` is an `int` argument in the function. However, we have not told the compiler that we want to be able to modify the value, so it will not let us! In order to tell the compiler that, there are different parameter types.

## Type Listing
| Type | Example | Explanation |
| ---- | ------- | ----------- |
| In | `int val` | Parameter `val` is passed as in input and so it can not be modified (it is *immutable*). |
| Ref | `ref int val` | Parameter `val` is passed by reference and can be modified (it is *mutable*). |
| Move | `move int val` | Parameter `val` is being "moved" to the function being called. |

```{note}
The parameter types `in` and `ref` are around the same performance wise!

More advanced programmers may be concerned about passing by value rather than passing by reference. In other languages, passing by value usually means that the entire structure is copied when calling the function, which is usually not desired.

However, `in` parameters in Asylum automatically pass a reference to a constant (like `const MyStruct&` in C++) rather than copy the structure (pass by value) when it is less costly to do so. Asylum was designed to have the common and most intuitive case be the most efficient! If you are confused, do not worry, this is not too important. Just know that using `ref` is not more or less efficent.
```

## Reference Parameters
The first solution we may do if we want to modify the `in` value is copy it to another variable:

```rust
fn setValue(int val) {
    int newVal = val;
    newVal = 7; // We can modify this!
}

fn main() {
    int x = 3;
    println(x);
    setValue(x);
    println(x);
}
```
Output:
```
3
3
```

Though do note that this is not what we wanted. While we can modify the value passed if we copy it into another variable, it does not impact the original value! This is solved by using a parameter marked as `ref`:

```rust
fn setValue(ref int val) {
    val = 7; // We are allowed to do this now!
}

fn main() {
    int x = 3;
    println(x);
    setValue(ref x);
    println(x);
}
```

Note that we have to do `ref x` to tell the compiler we want to pass a reference to the variable rather than pass it as an `in` parameter. This is because functions with different parameter types are different functions! This means the following code is perfectly valid:

```rust
fn myFunc(int a) -> int => a;
fn myFunc(ref int a) -> int => a;
fn myFunc(move int a) -> int => a;

fn main() {
    int x = 3;
    println(myFunc(x));
    println(myFunc(ref x));
    println(myFunc(move x));
}
```
Output:
```
3
3
3
```

## Move Parameters
You now hopefully understand how both `in` (default) and `ref` (reference) parameters work. But what is this mysterious `move`? Sometimes we have structures we can not copy, or are very large and do not want to be copied. However, we would still like to use the structure somewhere else. This is done by "moving" it to the function we are calling:

```rust
struct Image {
pub:
    Pixels pixels;
}

impl Image {
    pub fn setPixels(move Pixels pixels) {
        this.pixels := pixels;
    }
}
```

In the above example, `Pixels` is a structure that contains large resources and thus should be copied or constructed as little as possible. Therefore, we want to move pixels to the image rather than copy them. This is done by using the move operator `:=`. However, we are only allowed to move an item we have ownership over. If `pixels` was passed as `in` or as `ref`, then we would only get to "borrow" the structure rather than own it! We're allowed to make copies of borrowed items, but we can not move them!

Do not worry if this is confusing, Asylum's "move semantics" (or moving memory rather than borrowing or copying) will be discussed in its own section showing how to use it in practice and go into further detail.

### Use After Move
TODO!!!