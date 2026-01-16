# Collat Conjecture Assignment Solutions

## Solution

```c
#include <stdio.h>

void collatz(int number){
    while (1)
    {
        printf("%i\n", number);
        if(number == 1) break;
        if(number % 2 == 0) number /= 2;
        else number = number * 3 + 1;
    }
}

int main() {
    printf("%i\n", collatz_count(7));
}
```

## Bonus Solution

```c
#include <stdio.h>

int collatz_count(int number){
    int count = 0;
    while (1)
    {
        if(number == 1) return count;
        if(number % 2 == 0) number /= 2;
        else number = number * 3 + 1;
        count++;
    }
}

int main() {
    int res = 0;
    int max = 0;
    for (int idx = 3; idx < 1000; idx++)
    {
        int steps = collatz_count(idx);
        if(steps > max){
            res = idx;
            max = steps;
        }
    }
    printf("Number: %i, Steps: %i\n", res, max);
}
```