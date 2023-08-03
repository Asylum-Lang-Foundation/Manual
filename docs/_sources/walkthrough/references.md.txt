# References
In Asylum, you typically do not want to copy data around, especially if it is large. At the same time, you may not want to change how data is moved. References exist for this purpose, to allow you to use memory located somewhere else.

## Standard References
Earlier, we have discussed different ways of passing function arguments. Here is a reminder of passing by reference:

```rust
fn modifyVal(ref int val) {
    val = 5;
}

fn main() {
    int val = 3;
    modifyVal(ref val);
    println(val);
}
```
Output:
```
5
```

By passing by reference, we are allowed to modify data of the variable we pass to the function. However, there is an alternate way of representing references which we will use for the rest of the page:

```rust
fn modifyVal(int& val) {
    val = 5;
}

fn main() {
    int val = 3;
    modifyVal(val&);
    println(val);
}
```
Output:
```
5
```

### Reference Types
That `int&` from earier is not just passing by reference, but it is a type you can store to as well! All a reference does is allow you to have multiple variables share the same value. For example:

```rust
fn main() {
    int val1 = 3;
    int& val2 = val1&;
    println(val1 + ", " + val2);
    val2 = 5;
    println(val1 + ", " + val2);
    val1 = 7;
    println(val1 + ", " + val2);
}
```
Output:
```
3, 3
5, 5
7, 7
```

As you can see above, the values of `val1` and `val2` are always the same, and using one is the equivalent of using the other. There is also a short-hand notation for creating a reference:

```rust
int& val2 -> val1;
```

The arrow (points to operator) can be thought to read as `val2` "references" `val1`. This syntax is preferred and will be used for the rest of this page.

### More References
You can add as many references as you want!

```rust
fn main() {
    int val1 = 3;
    int& val2 -> val1;
    int& val3 -> val1;
    int& val4 -> val3;
    val4 = 5;
    println(val1 + ", " + val2 + ", " + val3 + ", " + val4);
}
```
Output:
```
5, 5, 5, 5
```

Notice that `int& val4 -> val3` does not have a reference reference the reference `val3` but rather the original `val1` value. This is because using a reference is the same as using the original value!

### Accessing References
Sometimes you will want to modify or retrieve a reference instead of the value that is being referenced. In order to do this, you can use the as assignable reference operator `@`:

```rust
fn main() {
    int val1 = 3;
    int val2 = 5;
    int& val3 -> val1;
    println(val3);
    @val3 -> val2;
    int&& val4 -> @val3;
    println(@val3);
    println(val4);
    println(@val4);
}
```
Output:
```
3
-> 5
-> 5
-> -> 5
```

1. We first initalize `val1` and `val2` with their values `3` and `5` respectively.
2. Variable `val3` references `val1`. We then inspect its value.
3. The reference `val3` is changed to now be a reference to `val2`. Notice how we need to put `@` before the name of the reference in order to modify it.
4. A reference to a reference named `val4` is created. In order to reference `val3` we need to treat `val3` as a reference rather than what it is referencing. This is done by putting `@` before the name of the variable.
5. Treating `val3` as a reference is printed and we see it "references" `3`. Printing the value of `val4` shows the same as `val3` as a reference. Printing `val4` as a reference shows that it is indeed a reference to a reference to `3`.

### Dangers Of References
It is important to note that standard references do not hold any data. If the data they are referring to is not there anymore, this can cause problems. For example:

```rust
fn main() {
    int& x;
    {
        int y = 3;
        @x -> y;
    } // Variable y goes out of scope here, x is now an invalid reference!
    println(x);
}
```
Output:
```
[Segmentation Fault]
```

Since `x` is referring to `y` and `y` dies before `x` does, we get a situation in which `x` tries to reference `y`, but does not know that `y` is gone. Thus when we try to use `x` (therefore we're trying to use `y`), our program crashes. The compiler will warn you when this may happen, but definitely keep this in mind.

### Function Arguments
You can not create a reference from function arguments that are of type `in` or `move`. However, you can copy parameters that are references:

```rust
fn myFunc1(int& val) {
    myFunc2(val&); // This makes a new reference.
    myFunc2(@val): // This uses the existing reference (preferred).
}

fn myFunc2(int& val) {
    println(val);
}
```

````{warning}
The `in` parameters are read-only, and so making a reference to this data breaks the promise that the data is read-only:

```rust
fn myFunc(int val) {
    int& myRef -> val; // This is not allowed!
}
```
````

````{warning}
The `move` parameters do not necessarily come from a variable (they can be from constant values), so there may not be any variable to reference. Thus, this is not allowed.

```rust
fn myFunc(int% val) {
    int& myRef -> val; // This is not allowed!
}
```
````

<!-- ## Nullable References
Before we talk about "smart references", we must first discuss a new type of reference: the nullable reference `T@`. The main difference between a nullable reference and a standard reference is that this type can either reference data, or it will not. When the data it references goes out of scope, the reference itself will be made null. Therefore, it is important to check the value of a nullable reference before using it for null:

```rust
fn add(int@ a, int@ b) -> int? {
    if (!@a || !@b) return null;
    else return a + b;
}

fn main() {
    int a = 3;
    int b = 5;
    println(ref a, b&); // Shows passing parameters both ways is the same.
    int@ r1 -> null;
    {
        int c = 7;
        @d -> c;
    }
    int& r2 -> b;
    int@ r3 = @r2;
    println(@r1, @r2);
}
```
Output:
```
8
null
```

* In order to check if a reference is null, we must first treat `a` and `b` like references rather than getting their `int` value. This is done by putting an `@` before them.
* If a nullable reference is valid, the value of the reference can be implicitly converted to a `bool`. This `bool` is true if the reference is valid/not null. This means putting `!` before the value of the reference, `!@x` will return true if the nullable reference `x` is null.

TODO: FIGURE OUT HOW THESE MAGICALLY KNOW HOW TO DIE!!!

TODO: WHAT IF CONDITIONAL:

int x = 3;
int& y -> x;
if (cond) @y -> z;

-->

## Nullable References
Standard references are great for when you know the data they reference is alive, however this is not always the case. There are some situations where you want to reference data and be able to check if the data still exists or not before using it. This is what the nullable reference is for. Nullable references also allow a reference to reference nothing or "null" as their name implies.

```rust
fn main() {
    int@ x;
    println(@x);
    {
        int y = 3;
        @x -> y;
        println(@x);
    }
    println(@x);
}
```
Output:
```
null
-> 3
null
```

We can declare a nullable reference by using `@` instead of `&`. Notice that we do not have to set the value of it like with standard references. Declaring `int@ x;` is the same as declaring `int@ x = null`; When printing the reference, or that it references a value like usual.

````{warning}
It is important to check the value of a nullable reference before using it if you don't know if it is null or not! The following code produces a segmentation fault like earlier:

```rust
fn myFunc(int@ x) {
    println(x);
}
```
Output:
```
[Segmentation Fault]
```

The correct way is this:
```rust
fn myFunc(int@ x) {
    if (@x) println(x);
}
```

The value of `@x` can be converted to a `bool` implicitly. If `x` is a nullable reference, this value is `true` if the reference is not null, or `false` if it is null. Note that `@x` where `x` is a standard reference will always implicitly be converted to `true`!

If you want to guarantee that the variable being passed is not null, then either pass the value by `in` or by `ref`! Nullable references should only be used as arguments to move nullable references around or when references can be null.
````

````{warning}
Nullable references are not a magic bullet. They will work properly if created from owning or counted references (discussed later) or by referencing data on the stack (as shown on the first example with nullable references). If a nullable reference is created from a standard reference, it will **not** know when to become null since the reference guarantees the referenced data will always exist!

```rust
fn main() {
    int x = 3;
    int& y -> x;
    int@ z -> y; // No problems so far. Even if `y` references something else later, `z` will still reference `x` since that's what `y` is doing at this time.
    {
        int w = 5;
        @y -> w; // Nullable reference `z` still references `x`.
        println(z);
    }
    z -> y; // Using `z` will produce segmentation fault now!
    println(@z);
}
```
Output:
```
3
[Segmentation Fault]
```

Even though we would expect `z` to be `null`, there is no way of knowing that `y` is an invalid reference. Thus, the compiler will think the memory that is referenced is valid. While you are allowed to create nullable references from standard references, it is highly recommended you do not.
````

## Smart References
There are certain scenarios in which you want finer control of when data lives. Some of which include not being able to instantiate a struct in its constructor or the struct does not have a definitive compile-time size (as in referred to by an abstract base). The solution to this are smart references.

### Owning References
Owning references are fairly simple in design. Only one reference can be crowned the owner of data. Owning references have type `T^`. You can think of the `^` as a crown that the reference is wearing.

```rust
struct MyStruct {
   pub int val;
}

impl MyStruct {
    pub MyStruct(int val) : val(_) {}
}

fn main() {
    MyStruct^ item = MyStruct^(3);
    println(item.val);
}
```
Output:
```
3
```

The above code shows how an owning reference is created. Owning references construct an instance of the type they hold on the heap. When they go out of scope, the data they hold goes out of scope as well. They behave similarly to `Box<T>` in Rust and `std::unique_ptr<T>` in C++.

#### Nullable References
Nullable references are allowed to copy from an owning reference, though nullable references are not allowed to own any data and thus just reference the same data. When the owning reference goes out of scope, the nullable reference is set to null.

```rust
fn main() {
    MyStruct@ r1;
    {
        MyStruct^ item = MyStruct^(3);
        @r1 -> item;
    }
    println(@r1):
}
```
Output:
```
null
```

#### Passing Arguments
It is possible to pass an owning reference as a nullable reference and a standard reference:

```rust
fn myFunc(MyStruct& a, MyStruct@ b) -> int? {
    if (!@b) return null;
    else return a.val + b.val;
}

fn main() {
    MyStruct^ item = MyStruct^(4);
    println(myFunc(@item, @item));
}
```
Output:
```
8
```

#### Transfering Ownership
Remember that is not possible to copy the value of an owning reference. However, data can be moved instead. This is useful for transferring ownership of data:

```rust
struct MyData {
    pub int^ val;
}

impl MyData {
    pub MyData(int^% val) { // Can also do `: val(_%)` here instead.
        this.val := val;
    }
}

fn main() {
    MyData data1 = MyData(int^(3));
    MyData data2 = MyData(move @data1.val); // Alternative is MyData(@data1.val%);
    println(@data1);
    println(@data2);
}
```
Output:
```
null
-> 3
```

In the above code, the ownership of `3` is transferred from `data1.val` to `data2.val`. When transferring the owner, it is important to make sure the reference gets transferred rather than the `int` it is referring to. Remember to place `@` before references when you want to manipulate the references.

### Counted References
Ownership is distributed among the `T#` references. Nullable references that point to it get nulled when all counted references are out of scope.

```rust
fn myFunc(MyStruct& a, MyStruct@ b) -> int? {
    if (!@b) return null;
    else return a.val + b.val;
}

fn main() {
    MyStruct# item1 = MyStruct^(4);
    {
        MyStruct# item2 -> item1;
        println(myFunc(@item1, @item2));
        println(@item1.count);
    }
    println(@item1.count);
}
```
Output:
```
8
2
1
```

The above code shows how a counted reference is created. Counted references construct an instance of the type they hold on the heap. Copying from a counted reference to another counted reference increments the reference count. When a counted reference goes out of scope, the reference count decreases. When the reference counter reaches 0, the memory referenced is deleted. Counted references behave similarly to `Rc<T>` in Rust and `std::shared_ptr<T>` in C++.

#### Nullable References
Nullable references for counted references work similarly how they do for owned references. when the number of counted references reaches `0`, the nullable references that reference the data are now `null`.

#### About Cycles
TODO!!!

## Challenges
TODO!!!

## Challenge Solutions
TODO!!!