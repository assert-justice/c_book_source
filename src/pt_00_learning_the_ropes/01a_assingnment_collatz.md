# Assignment: The Collatz Conjecture

Write a function `collatz` that accepts an integer and performs the following operation repeatedly:

- print the number
- if the number is 1, stop.
- if the number is even, divide it by 2.
- if instead the number is odd, multiply it by 3 and add 1.

The Collatz Conjecture says that every number will eventually reach one. You can read more about it [here](https://en.wikipedia.org/wiki/Collatz_conjecture) and [here](https://www.youtube.com/watch?v=5mFpVDpKX70) is a good video about it.

Example:

Your code should look like the following, with the `collatz` function filled in of course.

```c
#include <stdio.h>

void collatz(int number){
    // implementation here
}

int main() {
    collatz(7);
}
```

It should print the following

```
7
22
11
34
17
52
26
13
40
20
10
5
16
8
4
2
1
```

## Notes

You will need to figure out if a positive integer is even. If you don't know you can use the remainder operator `%` to do this. Feel free to go to your search engine of choice for help ;)

## Bonus Challenge

Write another function `collatz_count` that counts the number of steps a given number takes to get to one. For example if you call `collatz_count(7)` it should return 16. Use this function to find the number less than a thousand that takes the greatest number of steps.
