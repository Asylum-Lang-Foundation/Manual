# Structs & Implementations
On the previous page, you learned how structs can give you custom data types that allow you to hold data. But we are also allowed to give them functions as well inside of implementation blocks:

```rust
struct Color {
    byte r;
    byte g;
    byte b;
}

impl Color {
    fn toHex() -> str => "#{r:x2}{g:x2}{b:x2}";
}
```

In the code above, we define a function for every single `Color` struct. We could then use it like such:

```{note}
A struct can have as many implementation blocks as it desires.
```

```rust
fn main()
{
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

```{note}
More experience programmers are probably wondering why we are "copying" the newly created struct rather than moving it.

When a struct is newly constructed and assigned to a variable, it is "constructed in-place". The reason a C++ syntax was not chosen is because it is not intuitive for beginners, and makes constructors messy if certain things that must be constructed depend on code in the constructor.

This property in Asylum will be discussed more in the section about move semantics.
```

## Constructors
As you probably have noticed, it is a bit tedius to construct instances of structs. Defining a constructor allows you to make this process easier:

```rust
impl Color {
    pub This(byte r, byte g, byte b)
    {
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

````{note}
Since a constructor is defined, you can no longer do the following:
```rust
Color color = Color {
    r: 0x21,
    g: 0x43,
    b: 0x75
};
```
The reason for this is that the default (empty) constructor will get "deleted", as it assumes you don't want your data to be constructed another way! However, assume `Color` now has a new variable described as `byte a` under `b`. The following is legal:
```rust
Color color = Color(0x7F, 0x84, 0x29) {
    g: 0x53,
    a: 0x32
};
```
You're still allowed to change values of members after the constructor is called!
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

Alternatively, if more complex operations need to be done in advance, you can fall back to a typical constructor as long as everything gets constructed:

```rust
impl MyData {
    pub This(string name1, string name2, string name3, int num, float val) : data1(name1, num1) {
        int hash = data1.hash();
        data2 = Data(name2, hash);
        hash = data2.hash();
        data3 = Data(name3, hash);
    }
}
```

Just remember to not use a member before it is declared, otherwise the compiler will error. Also note how `this.` is not prefixed in front of `data2` and `data3` since there are not parameter names that can confuse the compiler on which variable is being used.

### Copy Constructors
Suppose we have the below code:

```rust
fn main() {
    MyData data1 = MyData("name1", "name2", "name3", 3, 7);
    MyData data2 = data1;
}
```

Here, we make a copy of the data. Copy constructors will always exist by default for structs, unless:
1. One of the members of the struct does not have a copy constructor.
2. The struct is marked with the `NoCopy` attribute.

The default copy constructor will by default make a copy of each of its members, calling member copy constructors as needed (Ex: The copy constructor of `Data` is called for copying `data1`, `data2`, and `data3`).

#### Custom Copy Constructor
There may be times when you want finer functionality with the copy constructor. This is done by making a constructor with a single parameter of the same type.

```rust
impl MyData {
    pub This(This src) {
        data1 = src.data1;
        data2 = src.data2;
        data3 = src.data3;
        val = src.val + 1;
    }
}
```

### Move Constructors
Suppose we have the following code:

```rust
fn main() {
    MyData data1 = MyData("name1", "name2", "name3", 3, 7);
    MyData data2 := data1;
}
```

Here, we move the data. Move constructors will always exist by default for structs, unless:
1. One of the members of the struct does not have a move constructor.
2. The struct is marked with the `NoMove` attribute.

The default move constructor will by default move each of its members, calling member copy constructors as needed (Ex: The move constructor of `Data` is called for move `data1`, `data2`, and `data3`).

#### Custom Move Constructor
There may be times when you want finer functionality with the move constructor. This is done by making a constructor with a single parameter of the same type, marked with `move`.

```rust
impl MyData {
    pub This(move This src) {
        data1 := src.data1;
        data2 := src.data2;
        data3 := src.data3;
        val = src.val + 1;
    }
}
```

### Empty Constructor
The empty constructor exists by default and is deleted if any constructor for the struct is defined. Of course you are free to define your own empty constructors as you like. But in the case an empty constructor exists, the syntax is valid:

```rust
fn main() {
    Color color;
    println(color.r);
    println(color.g);
    println(color.b);
}
```
Output:
```
0
0
0
```

All structs on the stack must be initialized, and so `color` has all its members with their default values.

## Default Values
Sometimes you don't want to have to define items in the constructor, as it is easier to have the defaults present in the struct itself. This can be done in the struct declaration:

```rust
struct MyData {
pub:
    Data data1;
    Data data2 = Data("Name", 3);
    Data data3;
    float val = 2;
    string myStr = "Hi";
    int num;
}
```

Note that `data1` and `data3` are of type `Data` and do not have a default/parameter-less constructor, and so will need to be constructed in a constructor for `MyData`.

## Destructors
If a struct contains resources that are initialized, you may want them to be destroyed when the struct is freed. This can be done via deconstructors:

```rust
impl MyData {
    ~This() {
        println("Data deleted.");
    }
}

fn main() {
    {
        MyData data1 = MyData("name1", "name2", "name3", 3, 7);
        println("Hi!");
    }
}
```
Output:
```
Data deleted.
Hi!
```

Deconstructors take no parameters and return nothing. They are not marked with `fn`, as they are not functions either. There may only be one deconstructor defined per struct.

## Attributes
It is possible to give structures attributes. Attributes are used either by the programmer or the compiler to mark special properties of structures. An example usage of attributes is below:

```rust
[NoCopy]
[PaddingAlignment(0)]
[AssertMemberSize(val, 4)]
struct MyData {
pub:
    Data data1;
    Data data2 = Data("Name", 3);
    Data data3;
    float val = 2;
    string myStr = "Hi";
    int num;
}
```

An attribute with no parameters has no need for parenthesis, though attributes with parameters have them passed as if calling a function, arguments separated by commas.

### Attribute List
Below is a table of attributes for structs:

| Name | Args | Description |
| ---- | ---- | ----------- |
| AssertMemberSize | member, bytes | Ensure member `member` is `bytes` bytes in size. |
| AssertSize | bytes | Ensure the entire struct is `bytes` bytes in size. |
| NoCopy | - | Prevent copying of the struct. |
| NoMove | - | Prevent moving of the struct. |
| PaddingAlignment | bytes | Ensure that each member of the struct is aligned to `bytes` bytes. |

## Properties
Getters and setters can be annoying to implement, though Asylum does its best to make them easier to work with. Those familiar with C# may recall similar looking code:

```rust
struct MyStruct {
    pub int num1;
    pub int num2 { get; pri set; }
    pub int num3 { pub get; pro set; }
    pub int num5 => num3 + 2;
    pub int num6 {
        get => num5;
        pri set { num3 = _ - 2; }
    }
    pub int num7 {
        get { return num5; }
    }
    pub int num8 {
        get;
        set => num1 = _;
    }
}
```

The above showcases different ways properties may be utilized. The above code may be confusing, hopefully these rules clear things up:
1. Getters or setters for properties each have their own access modifiers. They do not have to match. These getters and setter access modifiers override what the member variable is declared as.
2. If an access modifier is not set for a getter or setter, it will default to the variable member's access modifier.
3. The scope of a getter or setter's access modifier is not allowed to be narrower in scope than the member's access modifier.
4. A lamba operator `=>` can be used to immediately return a value for the get property. It is not possible to assign variables with this a value.
5. The lambda operator `=>` is allowed to be used for getters or setters in particular, or use curly brackets to note that it is a function, and you can call functions implemented by the struct in it. Note that `_` represents the value you give in the set function. The type of `_` will be the same type as the member variable.

## Operators
In the variables section, you learned about various operators that can be applied to variables. However, most of the operators are not implemented by default. Below is how operator overloading works for an example vector structure:

```rust
struct Vec3 {
pub:
    float x;
    float y;
    float z;
}

impl Vec3 {
    pub Vec3(float x, float y, float z) : x(_), y(_), z(_) {}
}

// TODO: IMPLEMENTATION SCOPE???
impl op.Add<Vec3> for Vec3 {
    fn op(Vec3 other) -> Vec3 => Vec3(x + other.x, y + other.y, z + other.z);
}

impl op.Sub<Vec3> for Vec3 {
    fn op(Vec3 other) -> Vec3 => Vec3(x - other.x, y - other.y, z - other.z);
}

impl op.Neg for Vec3 {
    fn op() -> Vec3 => Vec3(-x, -y, -z);
}
```

Inheritance is discussed further in the next section. For now, know that structs are allowed to implement interfaces as they see fit, and such implementations get their own implementation blocks. Asylum has a built-in namespace in its EASL (Embedded Asylum Standard Library) named `op` that contains interfaces for all overloadable operators. The function to implement the operator is always called `op`. For the `Add` and `Sub` implementations above, the `<Vec3>` indicates that we are "adding with" a `Vec3`.

### Overloadable Operators
See [Operators](/walkthrough/variables.html#general-operators) for information about each operator's "intended usage". Below lists overloadable operators:
| Operator | Name | Type Args | Function Args | Example |
| -------- | ---- | --------- | ------------- | ------- |
| + | Add | `T` | `T` other | `This` + `T` |
| - | Sub | `T` | `T` other | `This` - `T` |
| * | Mul | `T` | `T` other | `This` * `T` |
| / | Div | `T` | `T` other | `This` / `T` |
| % | Mod | `T` | `T` other | `This` % `T` |
| ** | Exp | `T` | `T` other | `This` ** `T` |
| .. | Range | `T` | `T` other | `This` .. `T` |
| ..= | RangeEq | `T` | `T` other | `This` ..= `T` |
| & | BitAnd | `T` | `T` other | `This` & `T` |
| \| | BitOr | `T` | `T` other | `This` \| `T` |
| ^ | BitXor | `T` | `T` other | `This` ^ `T` |
| ~ | BitNot | - | - | ~`This` |
| << | Lshift | `T` | `T` other | `This` << `T` |
| >> | Rshift | `T` | `T` other | `This` >> `T` |
| + | Pos | - | - | +`This` |
| - | Neg | - | - | -`This` |
| ++ | Inc | - | - | ++`This`; `This`++ |
| `--` | Dec | - | - | `--This`; `This--` |
| ^ | FromLast | - | - | ^`This` |
| * | Dereference | - | - | *`This` |
| ! | Not | - | - | !`This` |
| == | Eq | `T` | `T` other | `This` == `T` |
| != | Neq | `T` | `T` other | `This` != `T` |
| > | Gt | `T` | `T` other | `This` > `T` |
| < | Lt | `T` | `T` other | `This` < `T` |
| >= | Ge | `T` | `T` other | `This` >= `T` |
| <= | Le | `T` | `T` other | `This` <= `T` |
| <=> | Cmp | `T` | `T` other | `This` <=> `T` |
| [] | Index | `T` | `T` other | `This`[`T`] |

TODO: ASSIGNMENTS!!!

### Dereference Operator Overloading
In Asylum, implementing the dereference operator has a special effect in which the dot `.` operator now operates on the dereferenced item rather than the struct implementing it. In order to access members implementing the struct, the `@` operator must be used instead:

```rust
struct Data {
pub:
    int a;
    int b;
}

struct MyWrapper {
pub:
    Data wrapped;
    int val;
}

impl op.Dereference for MyWrapper {
    fn op() -> ref Data => ref wrapped;
}

fn main() {
    MyWrapper mw = MyWrapper {
        val(7)
    };
    mw.a = 5;
    mw.b = 8;
    println(@mw.val);
}
```
Output:
```
7
```

As you can see above, when we use the `.` operator on `mw` it no longer accesses `MyWrapper`, but rather `Data`. In order to access what is truly in `mw`, you have to treat it as an assignable reference using the `@` operator. The reason for this is to allow the ability of implementing custom wrapper types. This is discussed later in the section on smart references.

### False Operator Special Notes
The false `??` operator can not be overloaded. This is because it only works on a boolean input. However, pointers for example are implicitly castable if boolean.

```rust
pub unsafe fn allocIfNull(int* data) -> int* => data ?? new int;
```

The above code checks to see if `data` is a null pointer. If it is null, then `data` gets converted to `false` implicitly, which causes a `new int` to be returned. Otherwise, `data` is returned as since it has data, it gets converted to `true` implicitly. The operator thus returns data.

```rust
return x ?? y
return (bool)x ? x : y
if (x) return x; else return y;
```

The above three statements are equivalent.

## Casts
It is also possible to make your custom type convert to a custom type by implementing a cast. This is done by implementing the type you want to cast to:

```rust
struct Data {
    int val;
}

impl bool.implicit for Data {
    fn cast() -> bool => val == 7;
}
```

The above code implicitly casts `Data` to a `bool`. This means that whenever a value of `bool` type is expected, a value of `Data` type can be given. You can either implement `bool.implicit` or `bool.explicit` but not both (this holds for any type of course, not just `bool`). The difference is that an explicit casts requires you to cast the value yourself (Ex: `(bool)myVal`) when say a `bool` value is expected, where as an implicit cast will allow you to just pass `myVal` and have it be casted automatically.

## Challenges
TODO!!!

## Challenge Solutions
TODO!!!