# Fundamentals

## String formatting

In the following examples we'll be printing things besides string literals. To do this we'll use `printf`'s string formatting capabilities. Check out the following:

```c
#include <stdio.h>

int main() {
    int age = 20;
    printf("Age: %i\n", age); // prints "Age: 20"
    return 0;
}
```

Here we are creating an `int` variable called age and storing the value 20 in it. Then we print it out.

That goofy %i in the string literal tells C that we'll be supplying an integer we want it to add to the string in that place. There are other such special characters, we'll introduce them as they become relevant.

## Types and Typecasting

So far we've been working exclusively with `int` types and in the next section we'll move on to `char`. There are a bunch of types in C including `float`, `double`, `long`, and a menagerie of others. You can even define your own! Another type is the oddly named `size_t`. This is the biggest integer your computer architecture can natively handle. If you're writing C on any reasonably modern computer this will be a 64 bit unsigned integer.

For example, try compiling this code:

```c
#include <stdio.h>

int main() {
    size_t age = 20;
    printf("Age: %i\n", age);
    return 0;
}
```

When I do I get a warning: 

```
format '%i' expects argument of type 'int', but argument 2 has type 'size_t' {aka 'long long unsigned int'}
```

If you try running the code it works just fine btw. But still, we want to be good C citizens and write warning free code. It's complaining because it has to implicitly convert from a `size_t` to an `int`. Let's tell it to perform that conversion explicitly:

```c
#include <stdio.h>

int main() {
    size_t age = 20;
    printf("Age: %i\n", (int)age);
    return 0;
}
```

This code compiles without any warnings. Before we use `age` in an expression we write `(int)`. This is a typecast and it generally works fine for numeric types, including `char`. I wouldn't advise using it for other types unless you know what you're doing.

We can also solve this by using the `%zu` format identifier like so:

```c
#include <stdio.h>

int main() {
    size_t age = 20;
    printf("Age: %zu\n", age);
    return 0;
}
```

Note that despite being in C since C99 (the version of the C standard released in 1999) `%zu` might not be supported by every compiler out there. If it's not you can typecast it or if you're using C++ you can use the wretched and despicable `cout` syntax (there is a reason I'm teaching C and not C++, actually there are a lot of them).

## Loops

Coming from another language you should be comfortable with `while` and `for` loops but there are a couple things to note about how they work in C.

```c
#include <stdio.h>

int main() {
    int number = 10;
    printf("Countdown!\n");
    while(number > 0){
        printf("%i\n", number);
        number--;
    }
    printf("Blastoff!\n");
}

/* prints
Countdown!
10
9
8
7
6
5
4
3
2
1
Blastoff!
*/
```

The above is an example of a `while` loop. While a condition is true it executes the same block of code over and over. Note that c doesn't have a boolean type built in (there is one in the standard library though). That means that there is no `true` or `false` keyword! So if you want to write an infinite loop you could do it like so:

```c
#include <stdio.h>

int main() {
    while(1){
        printf("end is the beginning is the\n", number);
    }
}
```

1 is evaluated as true. Actually any integer besides 0 is treated as true. With that settled, on to `for` loops. The below code prints out a given number of Fibonacci numbers.

```c
#include <stdio.h>

int main() {
    size_t count = 10;
    int last = 0;
    int current = 1;
    printf("The first %zu Fibonacci numbers:\n", count);
    for(size_t idx = 0; idx < count; idx++){
        printf("%i\n", current);
        int next = current + last;
        last = current;
        current = next;
    }
}

/* prints
The first 10 Fibonacci numbers:
1
1
2
3
5
8
13
21
34
55
*/
```

Typically `for` loops use `i` as the increment variable. This is fine but I've found that it's harder for students to read, especially on a projector screen, while screen sharing, or if they have dyslexia or otherwise struggle to distinguish `i` from `l` or `1` or other symbols. That's the convention I'll be using moving forward.

You'll notice the type of `idx` is `size_t`. That's generally a best practice but it's not strictly required, you could use an `int` and it would probably be fine. Even so, we want to be good C citizens.

## Functions and Forward Declarations

We can declare functions besides `main` of course. Here we define a function called `doubler` that expects a single integer as an argument, doubles it, and returns the result. So far so good.

```c
#include <stdio.h>

int doubler(int value){
    return value * 2;
}

int main() {
    int age = 20;
    printf("Age before: %i\n", age);
    age = doubler(age);
    printf("Age after: %i\n", age);
    return 0;
}
```

What do you think will happen if we declare `doubler` after `main`? Well let's try it. Try compiling the following:

```c
#include <stdio.h>

int main() {
    int age = 20;
    printf("Age before: %i\n", age);
    age = doubler(age);
    printf("Age after: %i\n", age);
    return 0;
}

int doubler(int value){
    return value * 2;
}
```

On my machine I get a warning: "warning: implicit declaration of function 'doubler'" but the program itself still works fine. Other compilers will yell at you a greater or lesser amount over this. When C was first created the limited memory available in the machines of the time meant it had to have a "single-pass compiler". That means that the compiler had to be able to read each file only once from beginning to end. Modern compilers perform many passes of optimization but that's an implementation detail, the language is still designed with single-pass compilers in mind. That means that if you want to use a function you have to declare it before hand. In the first example this was easy enough, but when you have many functions that may depend on each other putting them in the order of use gets unwieldy or impossible fast. That's why C has forward declarations.

```c
#include <stdio.h>

int doubler(int value);

int main() {
    int age = 20;
    printf("Age before: %i\n", age);
    age = doubler(age);
    printf("Age after: %i\n", age);
    return 0;
}

int doubler(int value){
    return value * 2;
}
```

Now we tell the compiler before the main function that we intend to define a function called doubler with the given type signature. That way when it gets to the call to doubler in main it knows that function will exist at some point and will compile without errors.