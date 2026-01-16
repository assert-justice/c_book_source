# Some Pointers on Values and Pointers

Part of C's "low level" programming model is dealing with pointers that represent memory addresses. Back in the 70s they were literal memory addresses, now they are a lot more abstracted by the operating system with virtual paging and a bunch of bs. It's still useful to think of them that way though.

## Pass by Value / Pass by Reference

From other languages you're likely familiar with the concept of passing by value versus passing by reference. When you call a function, does that function get a copy of the data you pass it, or the original data? Different languages have different ways of specifying, in C we use pointers.

## Pointers

Let's look at an example.

```c
#include <stdio.h>

int main() {
    int age = 20;
    int* age_address = &age;
    printf("Age: %i\n", age); // prints "Age: 20"
    printf("Age address: %p\n", age_address); // (this time) prints "Age address: 0000009C847FFA8C"
    return 0;
}
```

Here we're defining an `int` called `age` as before. After that though we're defining another variable `age_address`. Instead of specifying its type as an `int` we call it a `int*`, this means a pointer to an integer.

> Sidebar: The asterisk denoting a pointer is not actually part of the type in C. This is because of some odd and regrettable syntax choices which we'll see more of later. As long as you don't try to define multiple pointers in one statement it's fine.

When we define `age_address` we set it to `&age`. Here the `&` symbol is the reference operator, it's how we get the address of a variable.

When we go to print out `age_address` we have to use %p, the string formatting symbol for a pointer. The time I ran this on my computer it gave the value of "0000009C847FFA8C", it will almost certainly be different when you run it. Again, to simplify, this is the address of the `age` variable, this is where it "lives" in the computer's memory. If we want to reverse this process we'll use the dereference operator `*`. Yes, this is the same symbol C uses for multiplication and indicating a variable is a pointer. Yes it can get confusing. No I don't know why they thought this was a good idea at the time.

Next let's try passing pointers to functions.

```c
#include <stdio.h>

void add_five(int* p){
    *p += 5;
}

int main() {
    int age = 20;
    printf("Age before: %i\n", age); // prints "Age before: 20"
    add_five(&age);
    printf("Age after: %i\n", age);// prints "Age after: 25"
    return 0;
}
```

Here we create a function called `add_five`. We specify has a `void` return type, that is, it returns nothing, and it accepts a pointer to an integer as an argument. This lets us pass the integer in `age` by reference rather than value. Our add_five function can change the value in age without returning anything. Let's see a common mistake ~~that I definitely didn't make while writing these notes~~

```c
#include <stdio.h>

void add_five(int* p){
    int value = *p;
    value += 5;
}

int main() {
    int age = 20;
    printf("Age before: %i\n", age); // prints "Age before: 20"
    add_five(&age);
    printf("Age after: %i\n", age);// still prints "Age after: 20" :/
    return 0;
}
```

Damn, that didn't work. Why not? Well when we dereference `p` and store it in `value` this stores a copy of the value from `age`. Then when we modify it, those changes are being made to the copy, *not* the original. Something to watch out for!

> Sidebar: Pure Functions. Generally speaking you want to avoid having functions modify their arguments like this. It's hard to reason about. That's an example of a "side effect", meaning a function does something besides accept and return values. Other examples of side effects are reading and writing files, making network requests, changing environment variables, stuff like that. Side effects are unavoidable sometimes but should be carefully quarantined in clearly marked places. Function that don't have side effects like our simple `doubler` example from earlier are called "pure" functions.

## Sizeof

On your computer everything is stored in bits, ones and zeros. These bits are arraigned into bytes, which is eight bits. So how big is an integer? Across the decades of C this question has had many answers. In the 70s having eight bit "words" wasn't even entirely standardized. To find out let's run the following:

```c
#include <stdio.h>

int main() {
    printf("Size of int: %zu\n", sizeof(int)); // hopefully prints "Size of int: 4"
    return 0;
}
```

The size of an integer on your system *should* be four bytes or 32 bits. I say *should* because the C standard specifies no such thing. For more serious programming you should use the fixed size integers in `stdint` but we don't need to bother for now. You'll notice I'm using the `%zu` format specifier, that's because `sizeof` returns a `size_t`.

Let's look at some more examples.

```c
#include <stdio.h>

int main() {
    int age = 20;
    char c = 'R';
    printf("Size of int: %zu\n", sizeof(age)); // prints Size of int: 4
    printf("Size of int*: %zu\n", sizeof(&age)); // prints Size of int*: 8
    printf("Size of char: %zu\n", sizeof(c)); // prints Size of char: 4
    printf("Size of char*: %zu\n", sizeof(&c)); // prints Size of char*: 8
    return 0;
}
```

What's a `char`? Well in C a `char` is a type of integer confusingly enough, that's typically used to represent an 8 bit ASCII character. Adequately supporting non ASCII characters and Unicode in C is a *huge* kettle of worms that we don't have time for right now.

You will note that on my machine both pointers to integers and pointers are characters are the same size, 8 bytes or 64 bits. That's because I'm using a 64 bit operating system. One of the problems with a 32 bit operating system is that the pointers are only 32 bits, meaning they can only address around 4GB of memory. Even with modern operating system tricks letting a single program on a 32 bit system use more memory than that is a real pain in the butt. But yeah when we say a device has a 64 bit operating system that generally refers to the size of pointers in that system.
