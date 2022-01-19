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
    Color color = Color {
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
    Color color = Color(0x7F, 0x84, 0x29);
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

## Operators
TODO!!!

## Casts
TODO!!!