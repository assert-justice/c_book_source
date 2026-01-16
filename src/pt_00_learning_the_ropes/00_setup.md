# Setup

You should have GCC or Clang installed and runnable from the command line. I'll be using GCC in the examples but the two compilers have very similar command line interfaces.

## Your First Shell Command

If you open a shell and run this command:

```bash
gcc --version
```

it should print pretty much the following:

```
gcc.exe (Rev6, Built by MSYS2 project) 13.1.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

Obviously your version and the exact text may be different. If you can run that command successfully congratulations, you have GCC installed. If not you have some more work to do.

## Hello World

Open up your text editor/ide of choice and write the following in a file called `hello.c`.

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n"); // prints "Hello World!"
    return 0;
}
```

You can probably guess this program will print "Hello World!" to the console. There are a couple of things to point out before we compile and run this sucker. We'll get to the include on the first line in a sec. At the heart of every C program is a function called `main`. The `int` before the name of the function tells us the function's return type, in this case an integer.

We call `printf` inside our main function and pass it a string literal. We'll get to printing other things besides strings in a bit.

Finally we return 0. By convention our program should return 0 if everything ran correctly. We can report to the OS that something went wrong by returning something other than zero.

## What's the deal with include?

Let's zero in on the first line of our hello world program. 

```c
#include <stdio.h>
```

Those of you who have used more modern languages might assume it's importing a module or library of some kind. That is... sorta true. Any keyword you see that starts with a # symbol is a C macro. We'll get around to talking about macros later, they are an eldrich science, but include simply dumps the contents of another file into ours. In this case we're asking for a file with the definition for `printf` along with a lot of other baloney we're not using yet. For most purposes you can think of it like importing a library but it is a lot more primitive than that implies and this has some implications later.

## Basic GCC usage

Assuming you have GCC installed and have your code in a file called `hello.c` you can compile it like so:

```bash
gcc -o hello ./hello.c
```

If you run that and it doesn't report any issues then congratulations! You've compiled your first C program. If you're on Windows it will have created a file called `hello.exe`, if you're on Linux or Mac it will simply be called `hello` with no file extension. To run the program you'll need to call it from the command line.

If on windows run

```bash
./hello.exe
```

Otherwise run

```bash
./hello
```

That should print "Hello World!" to the console. If not, well, you have some debugging to do. Good luck.

## Warnings

By default C is very permissive of bad practices. There is only so much you can do about this without investing heavily in tooling and a review process but we can at least ask the compiler to yell at us if we do anything silly. For the code that follows I'll be running the compiler with the and Wall and Wextra flags enabled like so:

```bash
gcc -Wall -Wextra -o hello ./hello.c
```

You'll get errors this way and sometimes you won't want these extra checks, especially if you're running someone else's code.