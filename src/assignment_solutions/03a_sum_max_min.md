# Sum Max Min Assignment Solutions

## Solution

```c
#include <stdio.h>

int sum(int* array, size_t length){
    int total = 0;
    for (size_t idx = 0; idx < length; idx++)
    {
        total += array[idx];
    }
    return total;
}

int max(int* array, size_t length){
    int greatest = 0;
    for (size_t idx = 0; idx < length; idx++)
    {
        if(array[idx] > greatest) greatest = array[idx];
    }
    return greatest;
}

int min(int* array, size_t length){
    int least = 2147483647;
    for (size_t idx = 0; idx < length; idx++)
    {
        if(array[idx] < least) least = array[idx];
    }
    return least;
}

int main() {
    int ages[] = {10, 20, 60, 19};
    printf("sum: %i\n", sum(ages, 4));
    printf("max: %i\n", max(ages, 4));
    printf("min: %i\n", min(ages, 4));
    return 0;
}
```

## Bonus 1

```c
#include <stdio.h>

int sum(int* array, size_t length){
    int total = 0;
    for (size_t idx = 0; idx < length; idx++)
    {
        total += array[idx];
    }
    return total;
}

float average(int* array, size_t length){
    return (float)sum(array, length) / length;
}

int main() {
    printf("5 / 2: %f", (float)5 / 2);
    int ages[] = {10, 20, 60, 19};
    printf("average: %f\n", average(ages, 4));
    return 0;
}
```
