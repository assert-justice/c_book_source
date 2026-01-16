# Multi Greeter Assignment Solutions

## Solution

```c
#include <stdio.h>
#include <string.h>

#define BUFFER_SIZE 256

int str_equals(char* a, char* b){
  size_t len_a = strlen(a);
  size_t len_b = strlen(b);
  if(len_a != len_b) return 0;
  for (size_t idx = 0; idx < len_a; idx++)
  {
    if(a[idx] != b[idx]) return 0;
  }
  return 1;
}

int main()
{
  char string [BUFFER_SIZE];
  printf("Hello! I am the greeter. I greet anyone who inputs their name.\n");
  while (1)
  {
    printf ("Input your name here: ");
    fgets(string, BUFFER_SIZE, stdin);
    size_t len = strlen(string);
    string[len-1] = 0;
    if(str_equals(string, "quit")) break;
    printf ("Hi %s!\n",string);
  }
  printf("Goodbye for now!\n");
  return 0;
}
```

## Bonus Solution

```c
#include <stdio.h>
#include <string.h>

#define NAME_BUFFER_SIZE 256
#define LOG_BUFFER_SIZE 256

void str_clear(char* str, int length){
    for (int idx = 0; idx < length; idx++)
    {
        str[idx] = 0;
    }
}

int str_equals(char* a, char* b){
  int len_a = strlen(a);
  int len_b = strlen(b);
  if(len_a != len_b) return 0;
  for (int idx = 0; idx < len_a; idx++)
  {
    if(a[idx] != b[idx]) return 0;
  }
  return 1;
}

int str_contains(char* source, char* pattern){
    int len_source = strlen(source);
    int len_pattern = strlen(pattern);
    if(len_pattern > len_source) return 0;
    for (int idx = 0; idx < len_source - len_pattern + 1; idx++)
    {
        int failed = 0;
        for (int pos = 0; pos < len_pattern; pos++)
        {
            if(source[idx + pos] != pattern[pos]){
                failed = 1;
                break;;
            }
        }
        if(!failed) return 1;
    }
    return 0;
}

int str_concat(char* source, char* target, int source_length){
    int len_source = strlen(source);
    int len_target = strlen(target);
    if(len_source + len_target >= source_length) return 1;
    for (int idx = 0; idx < len_target; idx++)
    {
        source[idx + len_source] = target[idx];
    }
    source[len_target + len_source] = 0;
    return 0;
}

int main()
{
  char name [NAME_BUFFER_SIZE];
  char log [LOG_BUFFER_SIZE];
  str_clear(name, NAME_BUFFER_SIZE);
  str_clear(log, LOG_BUFFER_SIZE);
  printf("Hello! I am the greeter. I greet anyone who inputs their name.\n");
  while (1)
  {
    printf ("Input your name here: ");
    fgets(name, NAME_BUFFER_SIZE, stdin);
    int len = strlen(name);
    name[len-1] = 0;
    if(str_equals(name, "quit")) break;
    int found = str_contains(log, name);
    if(found) printf ("Hi again %s!\n",name);
    else printf ("Hi %s!\n",name);
    if(found) continue; // Do not add duplicate name to log
    int err = str_concat(log, name, LOG_BUFFER_SIZE);
    if(err){
        printf("Insufficient room for a new name. Sorry %s :/\n", name);
        break;
    }
  }
  printf("Goodbye for now!\n");
  return 0;
}
```
