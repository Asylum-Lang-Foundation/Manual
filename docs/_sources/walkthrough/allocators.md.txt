# Allocators
Asylum's owned and counted references provide a convenient way of managing memory on the heap, and should be useful for most purposes. However, there is no memory-management solution that fits every situation. Thus, Asylum provides alternative ways of managing memory as well as providing programmers a way to implement their own.

## Arena Allocators
An arena is a pool of memory in which allocations are taken from. There are different variations of arenas, each with their own distinct purposes. For the most part, an arena is for when a lot of fast allocations must be done, but are all freed at the same time. A good use for this system would be game objects in a map, as many of them must be loaded (even at runtime), but would all be unloaded when the level is exited.

### Fixed Size Arena
A fixed size arena holds up to a certain number of a given type. It is most useful for when you know of an exact upper-bound of how many items you want to allocate:

```rust
fn main() {
    ArenaFixedSize<int> arena = ArenaFixedSize<int>(2);
    int@ a = arena.new(5);
    int@ b = arena.new(7);
    int@ c = arena.new(3);
    println(@a);
    println(@b);
    println(@c);
    arena.freeAll();
    println(@a);
}
```
Output:
```
-> 5
-> 7
null
null
```

The arena above takes a type of what it should allocate and a number of how many items that can be allocated at a time. Notice that a nullable reference is returned for allocations. If the nullable reference is null, then the item could not be allocated. Once items from the allocator are freed, the references to them are automatically set to `null`.

### Fixed Length Arena
A fixed length arena holds up to a certain number of bytes rather than a count of a particular type. This makes allocations much more flexible, though it comes at the expense of having the amount remaining allocations be much more abstract:

```rust
struct Data {
pub:
    int[3] vals;
    float val;
}

fn main() {
    ArenaFixedLength arena = ArenaFixedLength(0x10);
    int@ a = arena.new<int>(3):
    int@ b = arena.new<int>(5):
    Data@ data = arena.new<Data>();
    println(@a);
    println(@b);
    println(@data);
    arena.freeAll();
    @data := arena.new<Data>();
    println(@data);
}
```
Output:
```
-> 3
-> 5
null
-> { { 0, 0, 0 }, 0.0 }
```

The above code allocates a buffer of `0x10` or `16` bytes. This is enough to allocate the two `int`s as shown, but not enough to allocate the `Data` field. Note that after freeing the arena, there is enough room to allocate the `Data` field.

````{warning}
Do not count on byte sizes of allocations to match up with the total available! Sometimes alignments of structs being allocated can add up to larger sums depending on the platform and order of allocations. For example, structure sizes of `4` and `1` being allocated may actually add up to `8`! If it is important for sizes to match, then you should signify that the allocations are packed like so:

```rust
ArenaFixedLength arena = ArenaFixedLength(0x10, true);
```

This can come at the cost of efficency of course.
````

### Adjustable Size Arena

### Adjustable Length Arena

## General Purpose Allocators

### Passing Allocators

### Pushing/Popping Allocators

## Custom Allocators

## Challenges
TODO!!!

## Challenge Solutions
TODO!!!