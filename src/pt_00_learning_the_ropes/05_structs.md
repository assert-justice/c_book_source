# Structs

## Representing Things

```c
#include <stdio.h>
#include <string.h>

#define NAME_LENGTH 32

typedef struct Person{
    char name[NAME_LENGTH];
    int age;
} Person;

void person_print(Person person){
    printf("Name: %s\nAge: %i\n", person.name, person.age);
}

int person_init(Person* person, char* name, int age){
    //
}

int main() {
    Person alice = {"Alice", 34};
    Person bob = {"Bob", 20};
    person_print(alice);
    person_print(bob);
    return 0;
}
```

Structs are passed by value

Avoid uninitialized memory!

We're using a fixed length string for the name. Potentially problematic!

## A Not-So-Dynamic Array

