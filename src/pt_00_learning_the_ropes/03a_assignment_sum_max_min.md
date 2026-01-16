# Assignment: Sum, Max, and Min

Write functions to find the sum, maximum value, and minimum value of an array, given a pointer to the first element and a length.

For example:

```c
#include <stdio.h>

int sum(int* array, size_t length){
    // implementation here
}

int max(int* array, size_t length){
    // implementation here
}

int min(int* array, size_t length){
    // implementation here
}

int main() {
    int ages[] = {10, 20, 60, 19};
    printf("sum: %i\n", sum(ages, 4));
    printf("max: %i\n", max(ages, 4));
    printf("min: %i\n", min(ages, 4));
    return 0;
}
```

prints the following:

```
sum: 109
max: 60
min: 10
```

## Bonus 1

Define another function, `average`. The average of a set of numbers, you probably remember, is their sum divided by the number of values. The tricky part for you is that the function will be supplied a list of integers but must return a `float`, and we haven't really talked about those yet. Never fear, you know about typecasting and you have a friendly search engine to help you. That said here are a couple snippets to get you started.

```c
#include <stdio.h>

int main() {
    printf("5 / 2: %f\n", (float)5 / 2); // prints "5 / 2: 2.500000"
    return 0;
}
```

Notice we are using the `%f` format specifier to print out floats. Also notice that we are typecasting the 5 to a float but not the 2. When a math operation operates on a float and an integer, the integer is converted into a float automatically. All the trailing zeros after the 2.5 is a little annoying, there is a way you can specify the precision that a number should be printed out as with a format specifier. I'll leave that for you to look up ;)

Here is the code with the functions you should implement left empty. Note that you can call a function from another function, could be handy!

```c
#include <stdio.h>

int sum(int* array, size_t length){
    // implementation here
}

float average(int* array, size_t length){
    // implementation here
}

int main() {
    int ages[] = {10, 20, 60, 19};
    printf("average: %f\n", average(ages, 4));
    return 0;
}
```

Warning, the following bonus problems are *extremely* optional. I'm not providing snippets and the solutions are not at the end of the book. You're on your own.

## Bonus 2: Bonus Harder

Write a function that calculates the [mean](https://en.wikipedia.org/wiki/Mean) of an array. So if you sort an array by value, the mean is the value in the middle or, if the array has an even length, the average of the two values in the middle. That means you'll probably need to sort the array. Wikipedia has you covered for an explanation of [bubble sort](https://en.wikipedia.org/wiki/Bubble_sort), probably the simplest sorting algorithm out there. Feel free to use a fancier sorting algorithm if you want, I'm not your boss.

## Bonus 3: Tokyo Drift

Write a function to find the [standard deviation](https://en.wikipedia.org/wiki/Standard_deviation) of an array. Good luck with that pal. You'll need to include `sqrt` from the standard library or, hell, implement [Newton's method](https://en.wikipedia.org/wiki/Newton%27s_method) yourself because clearly you're unstoppable.
