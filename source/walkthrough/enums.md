# Enums
Sometimes you wish data would have a different form depending on the situation. Enum items pair an enum entry name with an optional tuple:

```rust
enum Vehicle {
    None,
    Unknown,
    Boat(int, str),
    Car(str: license, str: color)
}

fn main()
{
    Vehicle none = Vehicle.None;
    Vehicle boat = Vehicle.Boat(22, "The Speedster");
    Vehicle car1 = Vehicle.Car("PNYZ123", "Grey");
    Vehicle car2 = Vehicle.Car(color: "Black", license: "PNYZ456");
    println(none);
    println(boat);
    println(car1);
    println(car2);
}
```
Output:
```
None
Boat(22, The Speedster)
Car(license: PNYZ123, color: Grey)
Car(license: PNYZ456, color: Black)
```

## Enum Matching
You can handle the different values of an enum by using the `match` statement:

```rust
fn handleVehicle(Vehicle vehicle)
{
    match (vehicle) {
        None:
            println("There is no vehicle.");
        Boat(_, name):
            println("Boat named \"{name}\" does not have a relevant speed so it is not named.");
        Car:
            println("Car has license plate \"{vehicle.license}\" and color {vehicle[1]}.");
        _:
            println("What kind of vehicle is this?");
    }
}
```

In the above example, it is possible to bind the tuple's elements with names as done with `Boat`. Note that an item not needed can be named `_` to be ignored. It is also possible to use the tuple itself as shown with `car`. You are also allowed to set properties of the enum in the `match` statement.

## Getting/Setting Enum Vals
You may want to modify an enum outside of a `match` statement. This can be done by getting an enum "as" a value. This will return an optional reference:

```rust
fn changeCarLicense(Vehicle& vehicle, str newLicense)
{
    Vehicle.Car&? car = vehicle.as<Car>();
    if (car) {
        println("Changing car with license \"{car.license}\" to \"{newLicense}\".");
        +car.license = newLicense;
    } else {
        println("Vehicle is not a car.");
    }
}
```

We pass the `Vehicle` by reference since we wish to be able to modify it. Then, we can call a function that all enums have to interpret it as an enum value. This returns an optional reference as we are referencing the original vehicle still, but do not know if the vehicle is actually a `Car` or not. Thus, we have to first check to make sure if the option is valid first before we work with it.

You may wonder why we use an optional reference `&?` rather than a nullable reference `@`. The reason is that nullable references should only really be used when a reference to data can become invalid. This means that when we initially create a nullable reference we *expect* it to reference valid data but know the data it references can exceed the lifetime of the nullable reference. On the otherhand, we expect the `Vehicle.Car` to possibly not exist to begin with so we use an optional. This is a demonstrates that while there are syntatically multiple ways of doing the same thing in Asylum, it is important to choose the option that makes the most semantic sense.

## Challenges
TODO!!!

## Challenge Solutions
TODO!!!