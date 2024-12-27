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


