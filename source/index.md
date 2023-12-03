# Asylum Programming Language Reference Manual
Welcome to the Asylum Programming Language Reference Manual! Here, you will learn all there is to know about Asylum, regardless of your programming experience! Whether you are a programming veteran, or completely new to programming, this reference should go into the necessary depth with even practice problems to quiz your understanding.

```{eval-rst}
.. toctree::
   :maxdepth: 2
   :caption: Asylum Language Reference:

   /about.md
   /walkthrough/helloWorld.md
   /walkthrough/functions.md
   /walkthrough/dataTypes.md
   /walkthrough/variables.md
   /walkthrough/tuples.md
   /walkthrough/errors.md
   /walkthrough/implementations.md
   /walkthrough/inheritance.md
   /walkthrough/controlFlow.md
   /walkthrough/arrays.md
   /walkthrough/strings.md
   /walkthrough/parameterTypes.md
   /walkthrough/references.md
   /walkthrough/enums.md
   /walkthrough/pointers.md
   /walkthrough/allocators.md
   /walkthrough/copyMoveSemantics.md
   /walkthrough/templates.md
```

```{eval-rst}
.. toctree::
   :maxdepth: 2
   :caption: Developer Guide:

   /dev/about.md
   /dev/antlr.md
   /dev/builder.md
   /dev/visitor.md
   /dev/codeRepresentation.md
```

## Goals
Asylum has many goals:
* Be clear on how memory is being utilized (copied, referenced, moved, allocated, destroyed, etc).
* Be beginner friendly (intuitive syntax and semantics, "pits of success", higher-level abstractions, etc).
* Be applicable for general-purpose and low-level development.
* Written densely. This is to make it faster to type, read, and refactor. Most keywords and common types are short. Operator overloading is also allowed and taken advantage of to make operations easier to do and recognize.

## Non-Goals
There are some things Asylum does not aim to be:
* A clone of any language. While Asylum may look similar to a given language and have similar features, it is a different language and will not have the same features and guarantees.