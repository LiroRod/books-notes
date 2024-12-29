# Automatic memory management 3.2.2

There are two main techniques for managing memory: **reference counting** and **tracing garbage collection** (usually called "**garbage collection**" or "**GC**"). Ref counters are much simpler to implement. But, over time, the limitations of ref counting become too troublesome. All of those languages eventually ended up adding a full tracing GC or at least enough of one to clean up object cycles.

# Statements 3.5

When an expression's main job is to produce a *value*, a statement's job is to produce an *effect*. Since, by definition, statements don't evaluate to a value, to be useful they have to otherwise change the world in some way -- usually modifying some state, reading input, or producing output.

# Control flow 3.7

It's hard to write useful programs if you can't skip some code, or execute some more than once. That means control flow. In addition to the logical operators we already covered, Lox lifts three statements straight from C.

An *if* statement executes one of two statements based on some condition:

```c
if (condition) {
  print "yes";
} else {
  print "no";
}
```
A while loop executes the body repeatedly as long as the condition expression evaluates to true:
```c
var a = 1;
while (a < 10) {
    print a;
    a = a + 1;
}
```

Finally we have for loops
```c
for (var a = 1; a < 10; a = a + 1 ) {
    print a;
}
```

This loop does the same thing as the previous while loop. Most modern languages also have some sort of for-in or "foreach" loop for explicitly iterating over various sequence types. In a real language, that's nicer than the crude C-style for loop we got here. Lox keeps it basic.

# Functions 3.8

A function call expression looks the same as it does in C:
```c
makeBreakfast(bacon, eggs, toast);
```
You can also call a function without passing anything to it:
```c
makeBreakfast();
```
Unlike, say, Ruby the parentheses are mandatory in this case. If you leave them off, it doesn't *call* the function, it just refers to it.

In Lox, you declare your functions with the "fun" key:
```c
fun printSum(a, b) {
  print a + b;
}
```

