# Arrays and Pointers

Let's look at arrays. In C they are... kinda wacky.

```c
#include <stdio.h>

int main() {
    int ages[] = {10, 20, 60, 19};
    printf("Ages\n");
    for(size_t idx = 0; idx < 4; idx++){
        printf("Age %zu: %i\n", idx, ages[idx]);
    }
    return 0;
}
/* prints
Ages
Age 0: 10
Age 1: 20
Age 2: 60
Age 3: 19
*/
```

This should look pretty standard coming from other languages. We have an array with four elements called `ages`, we iterate through it printing each element, and then we return like normal. So far so good.

The first bit of weirdness is here:

```c
int ages[] = {10, 20, 60, 19};
```

We're creating an array of integers right? So why are the brackets *after* the variable name? This is the equivalent in C# for reference:

```c#
int[] ages = [10, 20, 60, 19];
```

You can see there the brackets are after the type `int` (and the beginning and end of the array are marked with brackets not braces). That's because the type is an array of integers. But that's just not how C rolls.

So how do we get the length of an array. It's... well, you'll see.

```c
#include <stdio.h>

int main() {
    int ages[] = {10, 20, 60, 19};
    printf("Size of (this) array: %zu\n", sizeof(ages));
    printf("Length of this array: %zu\n", sizeof(ages)/sizeof(int));
    printf("Size of pointer to array: %zu\n", sizeof(&ages));
    return 0;
}
/* prints
Size of (this) array: 16
Length of this array: 4
Size of pointer to array: 8
*/
```

So the size of the array is the size of whatever is in it times the number of elements. If we're using four 4 byte integers then the size is (check me on this) 4 * 4 = 16. That means we can get the length of the array by dividing its size by the size of its elements. Which is silly, but works in this case. Of course, we know how long the list is because we set it ourselves. Unfortunately this trick doesn't work if we're dealing with a pointer to an array, that's the size of every other pointer.

Another good question, what will the following print?

```c
#include <stdio.h>

int main() {
    int ages[] = {10, 20, 60, 19};
    printf("Age %i: %i\n", 4, ages[4]);
    return 0;
}
```

4 is beyond the last element of the array, so you should get an error right? Uh, no. Unlike any other programming language you are likely to use (besides C++ and Objective C where C code is syntactically valid) C has no bounds checking an arrays. You can gleefully read from arbitrary memory floating around on your computer. This is, not to get too technical, *very bad*. C code is inherently memory unsafe and it causes a large percentage of errors and exploits in any field where it is used. This isn't a skill issue, The NSA, White House, and EU have basically begged countries and corporations to stop using C and C++ for this reason (nobody uses Objective C any more besides old school Apple fanboys). Like I said though, it's inescapable and will never entirely go away.

Now that we've gotten that out of the way though, let's talk about how pointers and arrays are basically the same thing!

```c
#include <stdio.h>

void print_array(int* values, size_t length){
    for (size_t idx = 0; idx < length; idx++)
    {
        printf("%i; ", values[idx]);
    }
    printf("\n");
}

int main() {
    int ages[] = {10, 20, 60, 19};
    print_array(ages, 4); // prints "10; 20; 60; 19; "
    return 0;
}
```

Here we have a function that accepts a pointer to an int and a length, and prints out each element up to that length. What might strike you as odd is that `ages` in our `main` function is defined as an array of integers, but we can pass it to a function that expects an int pointer without any casting or warnings or anything. Like I said a second ago, C arrays are basically just pointers. You can treat all pointers like they are arrays whether they are or not. See what I said about memory safety above. Ready for some more fun and not at all infuriating quirks?

```c
#include <stdio.h>

void print_array(int* values, size_t length){
    for (size_t idx = 0; idx < length; idx++)
    {
        printf("%i; ", values[idx]);
    }
    printf("\n");
}

void double_all(int* values, size_t length){
    for (size_t i = 0; i < length; i++)
    {
        values[i] *= 2;
    }
}

int main() {
    int ages[] = {10, 20, 60, 19};
    print_array(ages, 4); // prints "10; 20; 60; 19; "
    double_all(ages, 4);
    print_array(ages, 4); // prints "20; 40; 120; 38;"
    return 0;
}
```

The above code should make sense based on what we've discussed so far. Our main function creates an array of four integers, passes it to `print_array` to be printed, passes it to `double_all` to double all the numbers, and finally passes the array to `print_array` again.

You can see in `double_all` we are looping through the elements of the array and doubling each one. Because we pass things by reference by passing their pointer, we are manipulating the same array that we defined in `main`, not a copy, so when we print out the array again its values have been updated. Let's leave everything the same except how `double_all` is implemented.

```c
void double_all(int* head, size_t length){
    int* end = head + length;
    while (head != end)
    {
        *head *= 2;
        head++;
    }
}
```

Wtf? What's happening?

This is an example of what's called "pointer arithmetic". We can add to pointers, subtract from them, compare them to other numbers and pointers. For most purposes pointers are just integers. Let's break it down line by line.

```c
void double_all(int* head, size_t length)
```

We have simply renamed `values` to `head`. We're calling the pointer `head` because instead of always pointing to the start of the array, we will move it to the next element each time through the loop. Think of it like the head of a typewriter.

```c
int* end = values + length;
```

We create another int pointer called `end`. This represents the address just past the last value in our array. We'll compare the `head` pointer to `end` as we iterate.

```c
while (head != end)
```

Our condition is that the `head` isn't yet at the end of the array. While this is true, keep looping.

```c
*head *= 2;
```

We dereference the value at `head` and double it.

```c
head++;
```

Finally we update the position of `head`. By adding 1 to it we "move" the pointer to the next element in the array.

This stuff takes some time to wrap your head around. For the most part the first way we implemented the function is fine but you should be able to read and understand pointer manipulation. For one thing, looking something up by its pointer is typically faster than using an index although this will vary a lot based on what your code specifically is doing, what compiler you're using, and what optimization flags you have set. Don't worry about it for now. I feel like I'm saying that a lot.
