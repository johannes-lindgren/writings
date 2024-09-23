# Why TypeScript?

The main reason to adopt TypeScript is to establish guarantees of how JavaScript runs. One concrete example is to prevent code from crashing; it is preferable to give the programmer an error in the IDE than to let the client experience an incident in production. Unfortunately, it is not so easy as to simply enable TypeScript in an existing project: before you may reap the benefits, you must first start programming in a much more restrictive way. Going from untyped languages to types languages is a shift in the programming paradigm. Since the advent of computing, programming languages have been following a trend of becoming more restrictive while giving more compile-time guarantees:

- Programming started with raw machine code and Assembly languages that directly translates to machine code. Assembly languages are notoriously difficult to reason about. Therefore, more restrictive languages soon emerged.
- Procedural languages, such as Fortran, contains the programmer to deal with variables, rather than registers and memory addresses.
- Structured languages such as C favors conditional blocks, loops, and subroutines over `goto` statements; which creates a more reasonable control flow.
- Typed languages such as C++ prevents the programmer from using memory in the wrong way; for example by indexing an arrat with a floating point number.
- Garbage-collected languages prohibits you from allocating and freeing up memory directly, but helps to prevent memory leaks.
- Pure functional languages prohibits you from mutating values, which guarantees that the program can be run concurrently.

With every new paradigm on this list, the programmer is more and more limited in what code they are able to produce; but in return, they are better able to write bug-free code. TypeScript is no different in that it adds new constraints to JavaScript; it prohibits you from using data in _potentially_ incorrect ways, which guarantees that you don't run into runtime exceptions. TypeScript limits what code you'd otherwise be allowed to write in JavaScript, but in turn gives you certain guarantees of how your program behaves at runtime. You're trading freedom against:

- Fewer runtime errors—by analyzing the structure of your data and functions
- Improved refactoring—IDEs can better understand how your code is connected
- Improved documentation—from the type signature
