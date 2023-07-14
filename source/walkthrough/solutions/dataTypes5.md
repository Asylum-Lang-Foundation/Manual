```{eval-rst}
:orphan:
```

# Challenge 5
Here, you were to design a circle struct with the following members (making as many additional structs as necessary):
* Radius
* Line Width
* Color
* Circle Inside

First, we must define another struct for the color. While some Asylum libraries take care of this for us, for now let's just assume we need a way to define color:

```rust
struct Color {
    byte red;
    byte green;
    byte blue;
}
```

Each member is a byte, as each color should be a value from 0-255. Other representations of color are valid too (like a float from 0-1), so this implementation is arbitrary. Also note that we did not specify what types the radius and line width should be. It makes logical sense for them to have a decimal point, so let's just go with standard floats:

```rust
struct Circle {
    float radius;
    float lineWidth;
    Color color;
    Circle@ innerCircle;
}
```

Also note that a `struct` can not contain itself due to that causing an infinite struct. To counter this, we made `innerCircle` a reference to a `Circle`.