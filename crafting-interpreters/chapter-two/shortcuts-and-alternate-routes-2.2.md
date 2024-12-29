# Shortcuts and alternate routes

That's the long path covering every possible phase you might implement. Many languages do walk the entire route, but there are a few shortcuts and alternate paths.

## Single-pass compilers 2.2.1

Some simple compilers interleave parsing, analysis and code generation so that they produce output code directly in the parser without ever allocating any syntax trees or other IRs. These **single-pass compilers** restrict the design of the language. You have no intermediate data structures to store global information about the program, and you don't revisit any previously parsed part of the code. That means as soon as you see some expression, you need to know enough to correctly compile it.

## Tree-walk interpreters 2.2.2

Some programming languages begin executing code right after parsing it to an AST (with maybe a bit of static analysis applied). To run the program, the interpreter traverses the syntax tree one branch and leaf at a time, evaluating each node as it goes.

This implementation style is common for student projects and little languages, but is not widely used for general-purpose languages since it tends to be slow.

## Transpilers 2.2.3

Writing a complete back end for a language can be a lot of work. If you have some existing generic IR to target, you could bolt your front end onto that. Otherwise, it seems like you're stuck. But what if you treated some other *source language* as if it were an intermediate representation?

You write a front end for your language. Then, in the back end, instead of doing all the work to *lower* the semantics to some primitive target language, you produce a string of valid source code for some other language that's about as high level as yours. Then, you use the existing compilation tools for that language as your escape route off the mountain and down to something you can execute. 

While the first transpiler translated one assembly language to another, today, most transpilers work on high-level languages. After the viral spread of UNIX to machines various and sundry, there began a long tradition of compilers that produced C as their output language. C compilers were available everywhere UNIX was and produced efficient code, so targeting C was a good way to get your language running on a lot of architectures.

Web browsers are the "machines" of today, and their "machine code" is JavaScript, so these days it seems almost every language out there has a compiler that targets JS since that's the min way to get your code running in a browser.

## Just-in-Time compilation (JIT) 2.2.4

This last one is less of a shortcut and more a dangerous alpine scramble best reserved for experts. The fastest way to execute code is by compiling it to machine code, but you might not know what architecture your end user's machine supports. What to do?

You can do the same thing that the HotSpotJVM, Microsoft's CLR and most JavaScript interpreters do. On the end user's machine, when the program is loaded -- either from source in the case of JS, or platform-independent bytecode for the JVM and CLR -- you compile it to native for the architecture their computer supports. Naturally enough, this is called **Just-in-Time compilation**. 

The most sophisticated JITs insert profiling hooks into the generated code to see which regions are most performance critical and what kind of data is flowing through them. Then, over time, they will automatically recompile those hot spots with more advanced optimizations.
