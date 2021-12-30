# Builder Code Calling For ASTs
So we have our AST, and we have a builder API. Put them together, and now any generated AST can be converted into a program we can compile and run. But how exactly does this process work? ANTLR4 does not just give us an AST, but it gives us a way of reading it to!

## ANTLR Visitors
Current visitor code is out of date and does not use the new builder API yet. See [here](https://github.com/Asylum-Lang-Foundation/StraitJacket-Sharp/tree/main/AST) for AST visiting code.