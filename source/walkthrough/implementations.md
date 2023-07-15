# Implementations
On the previous page, you learned how structs can give you custom data types that allow you to hold data. But we are also allowed to give them functions as well inside of implementation blocks:

```rust
struct Color {
pub:
    byte r;
    byte g;
    byte b;
}

impl Color {
    pub fn toHex() -> string => "#" + r.toString("x2") + g.toString("x2") + b.toString("x2");
}
```

In the code above, we define a function for every single `Color` struct. We could then use it like such:

```rust
fn main() {
    Color color {
        r: 0x21,
        g: 0x43,
        b: 0x75
    };
    println(color.toHex());
}
```
Output:
```
#214375
```

## Constructors
As you probably have noticed, it is a bit tedius to construct instances of structs. Defining a constructor allows you to make this process easier:

```rust
impl Color {
    pub This(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}

fn main() {
    Color color(0x7F, 0x84, 0x29);
    println(color.toHex());
}
```
Output:
```
#7f8429
```

A lot is happening here, but let's take this one step at a time. 

* `This` is a type only available in implementation blocks and resolves to the type being implemented. In this case, `This` resolves into `Color`. 
* In the constructor definition, the parameters `a`, `b`, and `c` shadow the members in the `Color` struct and are inaccessible. We are able to get around this by using the `this.` prefix to access them from the current `Color` instance.
* A constructor does not have the `fn` keyword in front and will never had a return type.

````{note}
Since a constructor is defined, you can no longer do the following:
```rust
Color color {
    r: 0x21,
    g: 0x43,
    b: 0x75
};
```
The reason for this is that the default (empty) constructor will get "deleted", as it assumes you don't want your data to be constructed another way! However, assume `Color` now has a new variable described as `byte a` under `b`. The following is legal:
```rust
Color color(0x7F, 0x84, 0x29) {
    g: 0x53,
    a: 0x32
};
```
You're still allowed to change values of members after the constructor is called!
````

````{warning}
Consider the following code:
```rust
Color color = Color(0x7F, 0x84, 0x29);
```
This is bad practice! The newly constructed `Color` is immediately copied, which can cause compiler errors if `Color` can not be copied, or be inefficient if `Color` is a large struct!

The compiler will do the best it can to optimize these situations, though it will warn you. 
````

### Better Contructors
While the above approach works, it is very tedious. Luckily, there is a shortcut:

```rust
impl Color {
    pub This(byte r, byte g, byte b) : r(_), g(g), b(_) {}
}
```

Overall, this is much shorter and cleaner. The following is happening:
* We signify we are constructing a member by calling the member like a function. For example, we are constructing `g` in the `Color` struct and are passing it a parameter with the name `g`.
* We are doing the same things with the `r` and `b` members too, the only different is the `_`. The `_` symbol gets automatically replaced with the name of the member being intialized. By giving our parameters the same names as the members, we can reduce refactoring needed if things in the struct change.

````{warning}
The following does not work:
```rust
impl Color {
    pub This(byte r2, byte g, byte b) : r(_), _(g) {}
}
```
````

The reasons why are:
* The `_` in `r(_)` will try and use the parameter `r`, but it does not exist.
* The `_` symbol does not work for member names to initialize, so `_(g)` does not know which member to initialize.

### Member Initialization
Suppose we have a struct with the following constructor:
```rust
impl Data {
    pub This(string name, int num) : name(_), num(_) {}
}
```

We also have the following struct:
```rust
struct MyData {
pub:
    Data data1;
    Data data2;
    Data data3;
    float val;
}
```

The `data1`, `data2`, and `data3` variables must also be initialized. We can do this using the same technique from earlier:

```rust
impl MyData {
    pub This(string name1, int num1, string name2, int num2, string name3, int num3, float val) : data1(name1, num1), data2(name2, num2), data3(name3, num3) {}
}
```

However, what if these members require being initialized by data from another member? The rule of thumb is that you can not use a struct's member variable until it is initialized, or else the compiler will error. We can get around this by the following:

```rust
impl MyData {
    pub This(string name1, string name2, string name3, int num, float val) : data1(name1, num1), data2(name2, data1.hash()), data3(name3, data2.hash()) {}
}
```

Alternatively, if more complex operations need to be done in advance, blocks can be interlaced with member constructors:

```rust
impl MyData {
    pub This(string name1, string name2, string name3, int num, float val) : data1(name1, num1) {
        int hash = data1.hash();
        // More complex functions here.
    } : data2(name2, hash) {
        int hash = data2.hash();
        // More complex functions here.
    } : data3(name3, hash) {}
}
```

We can chain as many member initializations and code blocks as we desire. Also notice how `hash` is the same name. This is because member intializations only have access to the code block directly before them (if it exists) and the parameters of the constructor (such as `name2`).

### Default Values
TODO!!!

## Operators
TODO!!!

## Casts
TODO!!!