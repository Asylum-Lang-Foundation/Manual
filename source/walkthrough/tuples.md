# Tuples
Tuples in Asylum allow you to work with variables of multiple types at the same time. For example, you may want to return multiple values at the same time:

```rust
fn getDivRem(int a, int b) -> int, int
{
    return a / b, a % b
}

fn main()
{
    println(getDivRem(10, 3))
    println(getDivRem(2, 3))
    println(getDivRem(8, 4))
}
```
Output:
```
3, 1
0, 2
2, 0
```

## Variables
You can declare variables of tuples like below:

```rust
fn main()
{
    int, str a;
    a = 3, "Test"
    str, int b = "Another test", 5;
    (int, int) c = 0, 1;
    println(a);
    println(b);
    println(c);
}
```
Output:
```
3, Test
Another test, 5
0, 1
```

As you can see, tuples are no different than other types of variables: you just list the values of the tuple.

## Accessing Members
You can use the index operator to operate on a tuple. You are even allowed to iterate over members of a tuple!

```rust
fn main()
{
    int, str, float items = 3, "Hi!", 5.0;
    println(items[2])
    items[0] = 7;
    for (item : items) {
        println(item);
    }
}
```
Output:
```
5
7
Hi!
5
```

## Packing/Unpacking Tuples
You are also allowed to unpack a tuple into multiple variables:

```rust
fn main()
{
    int a = 3;
    str b = "Str";
    int, str myTuple = a, b;
    int c, str d = myTuple;
    println(a);
    println(b);
    println(myTuple);
    println(c);
    println(d);
}
```
Output:
```
3
Str
3, Str
3
Str
```

````warning
The following code is invalid:

```rust
fn main()
{
    int, str myTuple = 3, "Str";
    str a, int b = myTuple;
}
```

You must make sure the types are correct and in the right order to unpack a tuple! However, the following code is legal:

```rust
fn main()
{
    int, str myTuple = 3, "Str";
    float a, str b = myTuple;
}
```

This is because it is possible to convert the `int` into a `float`! In the previous example, the `int` can be converted to `str` but the `str` could not be converted to `int`.
````

## Named Tuples
You are allowed to name the elements of a tuple, making it function similarly to a struct. This is because using the index operator can make it hard to keep track of what you are modifying.

```rust
fn main()
{
    int: num, str: text items = 3, "Hi";
    println(items.num);
    items.text = "Test";
    println(items);
}
```
Output:
```
3
num: 3, text: Test
```

## Batch Operations
Tuples in Asylum have a special property: you can apply an operator on tuples as long as all of the types in the tuple support it. For example:

```rust
fn main()
{
    int, str, float a = 3, "Hello ", 5.7;
    float, int, float b = 8.1, 0, 1.3;
    println(a + b);
}
```
Output:
```
11.1, Hello 0, 7
```

In the above example, a tuple of type `float, str, float` is printed.

### Vectors
Vectors are a special type of tuple that hold multiple of the same type. For example, `float, float, float` is a `float` vector of size `3`. EASL (Embedded Asylum Standard Library) defines vectors as:

```rust
typedef Vector<type T, 2> = T: x, T: y;
typedef Vector<type T, 3> = T: x, T: y, T: z;
typedef Vector<type T, 4> = T: x, T: y, T: z, T: w;
typedef Vector<type T, uint U where U > 4> = T, T...(U - 1);
```

This means that you can declare a vector of `4` floats by doing `Vector<float, 4>`. Note that vectors must have more than `1` element and that for the vectors with `2`, `3`, and `4` elements the items are named as well.

## Challenges
TODO!!!

## Challenge Solutions
TODO!!!