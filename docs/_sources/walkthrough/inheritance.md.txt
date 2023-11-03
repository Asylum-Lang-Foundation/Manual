# Inheritance
In Asylum, structures can inherit data and functions from other structures. Inheritance can be complicated for new programmers, so read carefully.

## Basic Inheritance
You can inherit from another struct by using `:` followed by the name of the struct to inherit from:

```rust
struct Tires {
pub:
    int count;
    int expectedPSI;
}

struct Vehicle {
pub:
    Color color;
    int year;
    string maker;
    string model;
}

struct Car : Vehicle {
pub:
    int trunkSpace;
    int year;
}

fn main() {
    Car car = Car {
        bases.Tires: Tires {
            count: 4,
            expectedPSI: 32
        },
        bases.Vehicle: Vehicle {
            color: Color(0x7F, 0x84, 0x29),
            year: 2017,
            maker: "Toyota",
            model: "Camry"
        },
        trunkSpace: 21,
        year: 2022
    };
    println(car.year);
    println(car.bases.Vehicle.year); // Can specify exactly where variable is from.
    println(car.count); // Is inherited from Tires.
}
```
Output:
```
2022
2017
4
```

Note that what the struct inherits can be referenced done by using `bases.STRUCTNAME.MEMBERNAME`.

### Function Inheritance
You can inherit functions similarly to members:

```rust
struct A {
    pub int val;
}

struct B : A {}
struct C : B {}

impl A {
    pub fn fun() -> int => val;
}

impl B {
    pub fn fun() -> int => 7;
}

impl C {
    pub fn fun() -> int => val;
}

fn main() {
    A a = A {
        val: 1
    };
    B b1 = B {
        bases.A: A {
            val: 2
        }
    };
    C c = C {
        bases.B: B {
            bases.A: A {
                val: 3
            }
        }
    };
    B& b2 -> c;
    println(a.fun());
    println(b1.fun());
    println(c.fun());
    println(b2.fun());
    println(b2.bases.A.fun());
}
```
Output:
```
1
7
3
7
3
```

There is a lot going on here, so let's take it a step at a time:
1. We create a bunch of structures. Here, `A` has an `int` named `val`, `B` inherits `A`, and `C` inherits `B`.
2. We give each of our structures implementations with identically named functions named `fun`. For `A` and `C`, these just return the value stored in `A`. For `B` though, it just returns `7`.
3. Since `a` is an `A`, it just returns the value stored. This is the same for `c`, since it's a `C`. However, `b1` is a `B` so returns `7`.
4. The variable `b2` is a reference to `c`. Since `c` is a `C` and `C` inherits from `B`, that means we have no problem treating `c` as a `B`. We could also treat it as an `A` as we wanted. But since we are treating it as a `B`, the function call `fun` returns `7`.
5. If we just call `A`'s function from `b2`, we print its value like it would if `b2` was an `A`.

## Traits
These are essentially virtual functions. When implementing functions, what function is called is heavily dependent on what struct you are working with as you saw above. However, what if we wanted `b2` to call `C`'s `fun` since we know it is really a `C`? A use case for this would be if we were implementing animals. We could have dogs, cats, etc. Ideally, we would want to just pass around an `Animal` and if it were a `Dog` it should bark, a `Cat` meow, etc. We can do this using traits:

```rust
trait Animal {
    fn noise() -> string => "";
    fn isMammal() -> bool;
}

struct Dog {};
struct Cat {};

impl Animal for Dog {
    fn noise() -> string => "Bark";
    fn isMammal() -> bool => true;
}

impl Animal for Cat {
    fn noise() -> string => "Meow";
    fn isMammal() -> bool => true;
}

fn main() {
    Dog dog;
    Cat cat;
    Animal& dog2 -> dog;
    println(dog.noise());
    println(cat.noise());
    println(dog2.noise());
}
```
Output:
```
Bark
Meow
Bark
```

In the above example, the `Animal` trait has a function for getting the noise of the animal. It has a default implementation that returns an empty noise, while the function for determining if the animal is a mammal has no default and must be implemented for the structure to not be abstract (explained later). Note that if we were to use standard functions rather than traits, `dog2.noise()` would return an empty string.

### Base Calling

### Abstract Structures

## The Diamond Problem
TODO!!!

### Composition VS Inheritance
TODO!!!