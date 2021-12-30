# Program Builder API
Alright, now we have an AST of our program. What do we do now? So far, the AST is composed of a bunch of abstract concepts. The goal of using the builder API is to transform our AST of abstract concepts into one that is object oriented such that we can work with it. The builder is defined in the namespace `StraitJacket.Builder`.

## Test Case
Consider the following file, *Test.asy*:

```rust
using MyLib.Other;
namespace MyLib;

pub fn add(int a, int b) => a + b;

int a = 3;
int b = 7;
if (2 < a) {
  printf("%d\n", add(a, b));
} else {
  printf("%d\n", add(b, b));
}
```

Of course ANTLR4 has visitor classes that we can tell how to handle things such as functions, enums, etc. and transfer that information to the builder. But, assuming we wish to create the AST manually using the builder API, it'll look something like this:

```c#
// Init.
Builder b = new Builder();
b.BeginFile("Test.asy");
b.StatementUsing("MyLib.Other");
b.StatementNamespace("MyLib");

// Add function.
b.PushModifier(Modifier.Public);
List<VarParameter> funcParams = new List<VarParameter>();
funcParams.Add(new VarParameter("int", "a"));
funcParams.Add(new VarParameter("int", "b"));
b.BeginFunction("add", new VarType("int"), funcParams);
b.PopModifier();
b.StatementReturn(
  new ExpressionOperator(Operator.Add, 
  new List<Expression>() {
    new ExpressionVariable("a"),
    new ExpressionVariable("b") 
  }
));
b.EndFunction();

// Top level statements.
b.StatementVariableDeclaration(new VarParameter("int", "a"), new ExpressionConstantInt(3));
b.StatementVariableDeclaration(new VarParameter("int", "b"), new ExpressionConstantInt(7));
b.BeginIf(
  new ExpressionOperator(Operator.Lt,
  new List<Expression>() {
    new ExpressionConstantInt(2),
    new ExpressionVariable("a")
  }
);
b.StatementExpression(
  new ExpressionFunctionCall(
    "printf",
    new List<Expression>() {
      new ExpressionConstantString("%d\n"),
      new ExpressionFunctionCall(
        "add",
        new List<Expression>() {
          new ExpressionVariable("a"),
          new ExpressionVariable("b")
        }
      );
    }
  )
);
b.Else();
b.StatementExpression(
  new ExpressionFunctionCall(
    "printf",
    new List<Expression>() {
      new ExpressionConstantString("%d\n"),
      new ExpressionFunctionCall(
        "add",
        new List<Expression>() {
          new ExpressionVariable("a"),
          new ExpressionVariable("b")
        }
      );
    }
  )
);
b.EndIf();

// End.
b.EndFile();
// Compile, run, or do whatever with code here.
```

As you can see, the builder code is very explicit. This is necessary to be able to properly determine how to compile a program. This also means that any Asylum program can be created programatically without ever having to go through an AST parser. ANTLR4 was chosen to generate the ASTs that we use to call the proper builder code, but this does not have to be the case. You could even make your own subset of Asylum with your own syntax if you wanted to!