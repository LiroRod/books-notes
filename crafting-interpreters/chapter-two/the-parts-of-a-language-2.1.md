# The parts of a language

## Chapter Intro 2.1.0

I visualize the network of paths an implementation may choose as climbing a mountain.

You start off at the bottom with the program as raw source text, literally just a string of characters.

Each phase analyzes the program and transforms it to some higher-level representation where the semantics -- what the author wants the computer to do -- becomes more apparent.

Eventually we reach the peak. We have a bird's-eye view of the user's program and can see what their code means. We begin our descent down the other side of the mountain. We transform this highest-level representation down to successively lower-level forms to get closer and closer to something we know how to make the CPU actually execute.

```md
                      --------------------------------
                    /                                 \
                   /                                   \
                  /                                     \
                 /        Optimizing <=== Loop ===> INTERMEDIATE REPESENTATION(S)
                /                                      ||        ||           \
               /                                       ||        ||            \
           Analysis                                    ||        ||             \
             /                                         || Code Generation =====> \
        "SYNTAX TREE" - Tree-Walk ---                  ||        ||               \
            /          Interpreter   \                 ||        ||                \
           /                           Transpiling     ||        ||                 \
          /                                ||          ||        ||                  \
         /                                 ||          ||        ||                   \
        /                                  ||          ||        ||                    \
      Parsing*                             ||          ||        ||                     \
       /                                   ||          ||        ||==>VIRTUAL MACHINE<== \
    "TOKENS"                               ||          ||        ||                       \
     /                                     ||          ||        ||                       ||
   scanning                                ||          ||        ||                       ||
    /                                      ||          ||        ||                       ||
   /                                       \/          ||        \/                       \/
"SOURCE-CODE"                          "HIGH-LEVEL" <==||    "BYTECODE"            "MACHINE CODE"
```

Let's trace through each of those trails and points of interest. Our journey begins on the left with the bare text of the user's source code:
```c
var average = (min+max) / 2;
```

## Scanning 2.1.1

The first step is **scanning** , also know as **lexing**, or (if you're trying to impress someone) **lexical analysis**. They all mean pretty much the same thing. I like "lexing" because it sounds like something an evil supervillain would do, but I'll use "scanning" because it seems to be marginally more commonplace.
>> "Lexical" comes from the Greek root "lex", meaning "word"

A scanner (or lexer) takes in the linear stream of characters and chunks them together into a series of something  more akin to "words". In programming languages each of these words is called a **token**. Some tokens are single characters like "(" and ",". Others may be several characters long, like numbers (123), string literals ("hi!"), and identifiers (min).

Some characters in a source file don't actually mean anything. Whitespace is often insignificant and comments, by definition, are ignored by the language.

The scanner usually discards these, leaving a clean sequence of meaningful tokens

```c
|var| |average| | = | |(| |min| | + | |max| |)| |/| |2| |;|
```

## Parsing 2.1.2

The next step is **parsing**. This is where our syntax gets a **grammar** -- the ability to compose larger expressions and statements out of smaller parts. Did you ever diagram sentences in English class? If so, you've done what a parser does, except that English has thousands and thousands of "keywords" and an overflowing cornucopia of ambiguity. Programming languages are much simpler.

A **parser** takes the flat sequence of tokens and builds a tree structure that mirroes the nested nature of the grammar. These trees have a couple of different name -- "parse tree" or "abstract syntax tree" -- depending on how close to the bare syntactic structure of the source language they are. In practice, language hackers usually call them "syntax trees", "ASTs", or often just "trees".

```md
         Stmt.Var  average
                      | 
         Expr.Binary "/"
                    /   \
      Expr.Binary "+"   "2" Expr.Literal
                 /   \
 Expr.Variable "min" "max" Expr.Variable
```
Parsing has a long, rich history in computer science that is closely tied to the artificial intelligence community. Many of the techniques used today to parse programming languages were originally conceived to parse *human* languages by AI researchers who were trying to get computers to talk to us.

It turns our human languages are too messy for the rigid grammars those parsers could handle, but they were a perfect fit for the simpler artificial grammars of programming languages. Alas, we flawed humans still manage to use those simple grammars incorrectly, so the parser's job also includes letting us know when do by reporting **syntax errors**.

## Static Analysis 2.1.3

The first two stages are pretty similar across all implementations. Now, the individual characteristics of each language start coming into play. At this point, we know the syntactic structure of the code -- things like which expressions are nested in which others - but we don't know much more than that.

In an expression like [a + b], we know we are adding [a] and [b], but we don't know what those names refer to. Are they local variables? Global? Where are they defined?

The first bit of analysis that most languages do is called **binding** or **resolution**. For each **identifier** we find out where that name is defined and wire the two together. This is where **scope** comes into play -- the region of source code where a certain name can be used to refer to a certain declaration.

If the language is statically typed, this is when we type check. Once we know where [a] and [b] are declared, we can also figure out their types. Then if those types don't support being added to each other we report a **type error**.

>> The language we are going to build in this book is dynamically typed, so it will do its type checking later, at runtime.

All this semantic insight that is visible to us from analysis needs to be store somewhere. There are few places we can squirrel it away:

- Often, it gets stored right back as **attributes** on the syntax tree itself -- extra fields in the nodes that aren't initialized during parsing but get filled in later.

- Other times, we may store data in a look-up table off to the side. Typically, the keys to this table are identifiers -- names of variables and declarations. In that case, we call it a **symbol table** and the values it associates with each key tell us what that identifier refers to.

- The most powerful bookkeeping tool is to transform the tree into an entirely new data structure that more directly expresses the semantics of the code. That's the next section.

## Intermediate representations 2.1.4

You can think of the compiler as a pipeline where each stage's job is to organize the data representing the user's code in a way that makes the next stage simpler to implement. The front end of the pipeline is specific to the source language the program is written in. The back end is concerned with the final architecture where the program will run.

In the middle, the code may be stored in some **Intermediate representation** (or **IR**) that isn't tightly tied to either the source or destination forms (hence "Intermediate"). Instead, the IR acts as an interface between there two languages.

This lets you support multiple source languages and target platforms with less effort. Say you want to implement Pascal, C and Fortran compilers and you want to target x86, ARM, and, I dunno, SPARC. Normally, that means you're signing up to write *nine* full compilers: Pascal -> x86, C -> ARM, and every other combination.

>> There are a few well-established styles of IRs out there. Hit your search engine of choice and look for "control flow graph", "static single-assignment", "continuation-passing style", and "three-address code".

## Optimization 2.1.5

Once we understand what the user's program means, we are free to swap it out with a different program that has the *same semantics* but implements them more efficiently -- we can **optimize** it.

A simple example is **constant folding**: if some expression always evaluates to the exact same value, we can do the evaluation at compile time and replace the code for the expression with its result. If the user typed in:
```c
const pennyArea = 3.14159 * (0.75 / 2) * (0.75 / 2);
```
We can do all of that arithmetic in the compiler and change the code to: 
```c
const pennyArea = 3.14159 * (0.75 / 2) * (0.75 / 2);
```
Optimization is a huge part of the programming language business. Many language hackers spend their entire careers here, squeezing every drop of performance they can out of their compilers to get their benchmarks a fraction of a percent faster. It can become a sort of obsession.

## Code Generation 2.1.6

We have applied all of the optimizations we can think of to the user's program. The last step is converting it to a form the machine can actually run. In other words **generating code**, where "code" here usually refers to the kind of primitive assembly-like instructions a CPU runs and not the kind of "source code" a human might want to read.

If your compiler targets x86 machine code, it's not going to run on an ARM device. To get around that hackers made their compilers produce *virtual* machine code. Instead of instructions for some real chip, they produced code for a hypothetical, idealized machine. *Niklaus Wirth* called this "**p-code**" for "portable", but today, we generally call it **bytecode** because each instruction is often a single byte long.

These synthetic instructions are designed to map a little more closely to the language's semantics, and not be so tied to the peculiarities of any one computer architecture and its accumulated historical cruft. You can think of it like a dense, binary encoding of the language's low-level operations.

## Virtual Machine 2.1.7

If your compiler produces bytecode, your work isn't over once that's done. Since there is no chip that speaks that bytecode, it's your job to translate. Again, you have two options. You can write a little mini-compiler for each target architecture that converts the bytecode to native code for that machine. You still have to do work for each chip you support, but this last stage is pretty simple and you get to reuse the rest of the compiler pipeline across all of the machines you support. You're basically using your bytecode as an intermediate representation.

Or you can write a **Virtual Machine**, a program that emulates a hypothetical chip supporting your virtual architecture at runtime. Running bytecode in a VM is slower than translating it to native code ahead of time because every instruction must be simulated at runtime each time it executes. In return, you get simplicity and portability. Implement your VM in, say, C, and you can run your language on any platform that has a C compiler. This is how the second interpreter we build in this book works.

## Runtime 2.1.8

We have finally hammered the user's program into a form that we can execute. The last step is running it. If we compiled it to machine code, we simply tell the operating system to load the executable and off it goes. If we compiled it to bytecode, we need to start up the VM and load the program into that. 

In both cases, for all but the basest of low-level languages, we usually need some services that our language provides while the program is running. For example, it the language automatically manages memory, we need a [garbage collector] going in order to reclaim unused bits. If our language supports  "instance of" test s so you can see what kind of object you have then we need some representation to keep track of the type of each object during execution.

