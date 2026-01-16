# Introduction

This guide is an introduction to C programming for people who have some experience in other languages, especially higher level scripting languages like Python and JavaScript. It assumes you know the basics of variables, functions, loops, arrays, and file io. It also assumes you're comfortable using a shell and invoking apps with a cli. It assumes you have a text editor or ide suitable for writing code as well as a C compiler installed. It does *not* assume any prior knowledge static typing, pointers or manual memory management.

## A Brief History

C was created by Dennis Ritchie in 1972 while he was working at Bell Labs. He was developing Unix at the time on a PDP-8 and had gotten fed up with writing assembly and the untyped predecessor to the language called B. The capabilities of computers of the early 70s to now are incomparable. Computers are vastly more capable and vastly more complex and many of C's design decisions that seem idiosyncratic or archaic make a lot of sense when viewed with the context of the time. For better or worse, C is the lingua franca of computers to this day. The C ABI (application binary interface) is used by every compiled programming language to some extent. All that to say we're pretty much stuck with it.

C is called a low level language, and it certainly is compared to Python or Java or JavaScript or most other modern popular languages. It's worth pointing out though that the C standard describes the behavior of an abstract machine and it's up to compiler and hardware developers to make that work. C's popularity means supporting it drives the computer architecture, not the other way around. Compilers do quite a lot of work to get C to make use of the power of modern machines.

### Compiled vs Interpreted, it's not a binary

C is a compiled programming language. Before it can be run we need to use a program like GCC to convert it into an executable. An executable is a file like any other, but instead of containing text or an image or audio it contains instructions for the computer to run in its own machine language. If you're on Windows or Linux you'll most likely be using x86_64 assembly. If you're on a modern Mac with Apple Silicon you'll be running Arm assembly.

The alternative to a compiled language is an interpreted language. There are a lot of these but let's pick on Python as an example. When you run a Python script it isn't translated into native machine instructions. Instead you start a program called an interpreter and it reads the source files you give it and executes them. Think of it as a little robot that reads your scripts and does what those scripts tell it to. This process adds a lot of overhead to running a program, so why do it?

Well when you're working with a compiled language like C you can only target one of the architectures I mentioned above at a time. If you want to get a Windows app working on Linux or vice versa you would have to recompile it for that architecture and operating system (I'm ignoring virtual machines, wrappers, containerization right now for simplicity).

With an interpreted language the interpreter is typically written in a compiled language and has to be ported to every OS and architecture you want to support, but after that you're done! Any Python script can run on any computer with a Python interpreter (with some caveats that aren't worth going into here).