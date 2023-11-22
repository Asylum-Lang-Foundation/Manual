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
   /walkthrough/implementations.md
   /walkthrough/inheritance.md
   /walkthrough/controlFlow.md
   /walkthrough/enums.md
   /walkthrough/strings.md
   /walkthrough/parameterTypes.md
   /walkthrough/optional.md
   /walkthrough/references.md
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

## Non-Goals
There are some things Asylum does not aim to be:
* Have everything C++ does. Asylum has a lot of features, but not all of them.