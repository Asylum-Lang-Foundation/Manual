# References
In Asylum, you typically do not want to copy data around, especially if it is large. At the same time, you may not want to change how data is moved. References exist for this purpose, to allow you to use memory located somewhere else.

## Standard References
TODO!!!

## Nullable References
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
Remember that is not possible to copy the value of an owning pointer. However, data can be moved instead. This is useful for transferring ownership of data:

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