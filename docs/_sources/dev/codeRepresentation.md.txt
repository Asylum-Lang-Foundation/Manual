# Working With The Code Representation
Thanks to the builder API, we now have an object-oriented representation of our AST. This is important for the next steps, but what exactly are the next steps and how does it work?

## Object-Oriented Representation In Greater Detail
First we must understand what the builder actually creates. Our AST gets turned into a series of functions, code statements, expressions, and variable types.

### Universals
A *universal* is an item that can appear at the top level of a file, outside of all functions, structs, enums, etc.

#### Functions
TODO

### Code Statements
A *code statement* is a valid line of code that does something. Some examples include `x += 7;`, `if (x < 8) println("Hi");`, etc.

#### Expression Statement
An expression statement is just one that is an expression such as a function call, operation, etc.

### Expressions
TODO

## Compilation Process
Now that we understand how our AST is layed out in object-oriented form, we can go over the steps that compile our representation into executable code:

1. Variable Resolution - Resolves called variables and functions.
2. Type Resolution - Resolves types of variables, expressions, and finalizes which function overload to call.
3. Declaration Compilation - In functions, variable declarations must be compiled before anything else (more on that later).
4. Compilation - Convert our code into LLVM bitcode.