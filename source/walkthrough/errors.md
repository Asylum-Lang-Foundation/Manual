# Errors
In Asylum, you can return an error instead of a normal value in a function if things do not go according to plan. For example:

```rust
fn safeDiv(int a, int b) -> int?
{
    if (b == 0)
        return Err();
    else
        return Ok(a / b);
}

fn main()
{
    int? a = safeDiv(5, 3);
    int? b = safeDiv(2, 0);
    if (a) {
        println(*a);
    } else {
        println(0):
    }
    match (b) {
        Err:
            println(0);
        Ok(val):
            println(val):
    }
}
```
Output:
```
1
0
```

The above code will catch a division by zero before it happens. It is important to note that `T?` is actually a shortcut for `Res<T, void>`, so any optional type is actually a result enum! This is why we are allowed to return `Err` and `Ok` in `safeDiv` and can use the `match` statement in `main`. Note that the code for checking `a` and `b` do the same thing. This is because `Res` can be implicitly casted to a boolean, where it is `true` only if there is no error present. Dereferencing, or using `*` on a result will return its valid value. Dereferencing when an error is present will abort the program and dump a stack trace in debug mode but result in undefined behavior in release mode.

## Error Types
Results can specify types for errors as well other than the default `void` for optionals. This allows you to provide more detail about the error encountered. For example, TODO!!!

## False Operator
If you wish to have a default value even if a function returns an error, there is a special usage of the `??` (false operator) in which an error will be treated as "false".

```rust
fn main()
{
    int a = safeDiv(5, 3) ?? 0;
    int b = safeDiv(2, 0) ?? 0;
    println(a);
    println(b);
}
```
Output:
```
1
0
```

For results, you can think of `a ?? b` as doing the following where `a` is `Res<T, U>` and `b` is `T`:
```rust
a ? (*a) : b;
```

Essentially, the value is returned from the result if it exists or an alternative value is returned instead. Notice that the usage of this operator in `main` allows you to do the same thing as the `main` before but with less syntax!

## Error Propagation
Sometimes you would like to exit a function early when an error is encountered. The `!?` (success or die) operator is for this purpose: it will either dereference the value from the result or return the error'd result if the result is an error.

```rust
fn safeDiv(int a, int b) -> int?
{
    if (b == 0)
        return Err();
    else
        return Ok(a / b);
}

fn safeAddDivs(int a, int b) -> int?
{
    int div1 = safeDiv(a, b)!?;
    int div2 = safeDiv(b, a)!?;
    return Ok(div1 + div2);
}

fn main()
{
    println(safeAddDivs(3, 5));
    println(safeAddDivs(0, 2));
}
```
Output:
```
Ok(1)
Err()
```

Though it's important to note that the result type of `safeAddDivs` and `safeDiv` must match. If you wish to return a different result or error you can do so by putting it after the operator:

```rust
fn safeDiv(int a, int b) -> int?
{
    if (b == 0)
        return Err();
    else
        return Ok(a / b);
}

fn safeAddDivs(int a, int b) -> Res<int, str>
{
    int div1 = safeDiv(a, b) !? Err("Division by 0");
    int div2 = safeDiv(b, a) !? Err("Division by 0");
    return Ok(div1 + div2);
}

fn main()
{
    println(safeAddDivs(3, 5));
    println(safeAddDivs(0, 2));
}
```
Output:
```
Ok(1)
Err(Division by 0)
```

While here the `!?` operator is used to return errors immediately, it could also return valid results if you wanted to! Overall, these operators help you write cleaner and denser code.