# The Joy of C Strings

Hoo boy. Ok. So dealing with strings in C does not spark joy. If you have a lot of string manipulation in your C app you'll want to write some helper functions or use someone else's. There are a few factors working towards making string manipulation a pain in the ass in C.

The first thing to understand is that C strings are just arrays of characters. Remember that the `char` type is an 8 bit integer. That's fine as far as it goes although it does make supporting, say, utf-8 take some extra work. No the issue is that as we have seen arrays don't know how long they are.

```c
#include <stdio.h>

int main() {
    char name[30] = "Alice";
    printf("Name: %s!\n", name); // prints "Name: Alice!"
    return 0;
}
```

Here we're creating an array of 30 characters (I picked 30 arbitrarily). You can see that we can print it with the %s format character. Alice is only 5 characters though, what about the other 25? Why are there not a bunch of empty spaces trailing that? Allow me to introduce you to one of the four horsemen of the apocalypse, a foe so devious that it has bested braver souls than you or I: the Null Terminator.

When Dennis Ritchie was designing C he considered keeping around an additional number with every string indicating how long it was. He determined, probably rightly, that the extra memory wasn't worth it. Instead every well formatted string ends with a null terminator, a zero value. If you pull up an ASCII chart you'll see that the printable characters don't start until 32, which is a space. The first 32 ASCII codes are control characters and a lot of them are archaic and were never really used. If there isn't an escape sequence that represents it that's because it was never used. The very first character sure was used though, 0. The null terminator. Let's write a little function to help us visualize what's happening.

```c
#include <stdio.h>

void list_chars(char* chars, size_t length){
    for (size_t idx = 0; idx < length; idx++)
    {
        printf("char: %c, code: %i\n", chars[idx], (int)chars[idx]);
    }
}

int main() {
    char name[30] = "Alice";
    list_chars(name, 6);
    return 0;
}

/* prints
char: A, code: 65
char: l, code: 108
char: i, code: 105
char: c, code: 99
char: e, code: 101
char: , code: 0
*/
```

We have defined a new function called `list_chars`. It prints out each character in a char array as well as its ASCII code. Note that we are casting the character at `idx` to an integer to get its ASCII code. The upper case letters start at 65 with `A`. The lower case letters start exactly 32 higher at 97. You can see that lower case `c` has the code 99, two past lower case `a`. Finally you will notice the last code is 0. That's the null terminator. What if we keep going?

When I ran it everything in the array past what we set was also a null terminator. You can't rely on this being the case though! Any old garbage can be in there, the C standard leaves it unspecified.

With that in mind let's write a function that clears the contents of a string to all null terminators.

```c
void str_clear(char* str, size_t length){
    for (size_t idx = 0; idx < length; idx++)
    {
        str[idx] = 0;
    }
}
```

Right now the madness and ennui provoked by the null terminator might leave you mystified. What's so bad about it? Well partly I'm playing it up, but also it's really easy to forget to deal with.

```c
#include <stdio.h>
#include <string.h>

#define BUFFER_CAPACITY 256

void str_clear(char* str, size_t length){
    for (size_t idx = 0; idx < length; idx++)
    {
        str[idx] = 0;
    }
}

void list_chars(char* chars, size_t length){
    for (size_t idx = 0; idx < length; idx++)
    {
        printf("char: %c, code: %i\n", chars[idx], (int)chars[idx]);
    }
}

int set_chars(char* source, char* target, size_t offset, size_t target_length){
    size_t source_len = strlen(source);
    if(offset + source_len > target_length){
        printf("Source string does not fit in target!\n");
        return 1;
    }
    for (size_t idx = 0; idx < source_len; idx++)
    {
        target[idx + offset] = source[idx];
    }
    return 0;
}

int main() {
    char name[BUFFER_CAPACITY];
    str_clear(name, BUFFER_CAPACITY);
    int err = set_chars("Alice", name, 0, BUFFER_CAPACITY);
    if(err) return err;
    printf(name); // prints "Alice"
    return 0;
}
```

Let's talk about the new stuff. Up top we have added the `string.h` header with an include. We're using a function from this header, `strlen`, below. Then we define a constant called `BUFFER_CAPACITY` using the `#define` macro. This macro will replace the text `BUFFER_CAPACITY` in our program with the text of whatever follows it, in this case 256. We can change the defined size of the buffer and it will be updated automatically everywhere in the file. You might think 256 is an odd number to pick. It's arbitrary, but computers like powers of two and neatly aligned memory. Thinking of powers of two as round numbers is a habit you get into. It should go without saying but you don't want to exceed the limits of the buffer you have reserved. 256 should be plenty big for us for now.

Here I've added a new function, `set_chars`. Let's talk about it. It accepts four arguments:
- A source string, the string where the data is coming from.
- A target string, the string we'll be writing to.
- An offset integer, how far into the target string we'll start writing.
- The length of the target string as an integer.

This function returns an integer. Like the `main` function, `set_chars` returns 0 if everything went correctly, and some other number otherwise. The error we are checking for in the body of `set_chars` is a bounds check, we want to make sure the target string is long enough to contain the source string. First we need to calculate the length of the source string, we can do that by including `string.h` at the top of the file, which defines the function `strlen`. Then we check if the length of the source string plus the offset exceeds the capacity of the target string. If it does then we print an error and return 1 indicating something went wrong. Otherwise we loop through `source`, assigning each character from it to the corresponding character in `target`. Finally we return 0 to indicate a successful operation.

Back in `main`, we create the `char` array, we call `set_chars` to write the string "Alice" to the array, and we store the result in a variable called `err`. We check if `err` is zero. If we call `if` with a number it will evaluate to true if it is not zero and false if it is. So if `err` is not zero we return that error code. Remember that a program should return 0 if everything went as expected, and a non zero value if something failed.

If we run the above it will simply print out the string "Alice". Let's test our new error functionality. For the next few snippets I'll only be showing the `main` function, everything above that will remain unchanged.

```c
int main() {
    char name[BUFFER_CAPACITY];
    str_clear(name, BUFFER_CAPACITY);
    int err = set_chars("Alice", name, 0, 2);
    if(err) return err;
    printf(name); // prints "Alice"
    return 0;
}
```

Here we're calling `set_chars` with a length of only 2. This isn't enough to store our string "Alice". Sure enough, when we compile and run the code it prints "Source string does not fit in target!" and returns an error code.

Ok, now that we have checked the error handling is working, let's use `set_chars` twice.

```c
int main() {
    char name[BUFFER_CAPACITY];
    str_clear(name, BUFFER_CAPACITY);
    int err = set_chars("Alice", name, 0, BUFFER_CAPACITY);
    if(err) return err;
    err = set_chars("and Bob", name, 6, BUFFER_CAPACITY);
    if(err) return err;
    printf(name); // still prints "Alice". Why?
    return 0;
}
```

In the above we the second time we called `set_chars` we tried to set the characters "and Bob" so the string would read "Alice and Bob". But that didn't happen! It still only prints "Alice". Why? Because of the offset. We started the "and Bob" portion of the string one character past the end of the old string. That null terminator is still there. Let's call `list_chars` and see what happened.

So we need to replace that null terminator with a zero. Easy enough.

```c
int main() {
    char name[BUFFER_CAPACITY];
    str_clear(name, BUFFER_CAPACITY);
    int err = set_chars("Alice", name, 0, BUFFER_CAPACITY);
    if(err) return err;
    err = set_chars("and Bob", name, 6, BUFFER_CAPACITY);
    if(err) return err;
    list_chars(name, 13);
    return 0;
}

/* prints
char: A, code: 65
char: l, code: 108
char: i, code: 105
char: c, code: 99
char: e, code: 101
char: , code: 0 <- here is our problem
char: a, code: 97
char: n, code: 110
char: d, code: 100
char:  , code: 32
char: B, code: 66
char: o, code: 111
char: b, code: 98
*/
```

```c
int main() {
    char name[BUFFER_CAPACITY];
    str_clear(name, BUFFER_CAPACITY);
    int err = set_chars("Alice", name, 0, BUFFER_CAPACITY);
    if(err) return err;
    err = set_chars(" and Bob", name, 5, BUFFER_CAPACITY);
    if(err) return err;
    printf("%s\n", name);
    list_chars(name, 13);
    return 0;
}

/* prints
Alice and Bob
char: A, code: 65
char: l, code: 108
char: i, code: 105
char: c, code: 99
char: e, code: 101
char:  , code: 32 <- this is now a space, not a null terminator.
char: a, code: 97
char: n, code: 110
char: d, code: 100
char:  , code: 32
char: B, code: 66
char: o, code: 111
char: b, code: 98
*/
```

There are functions in the C standard library that handle various string operations for you. Make sure to check the docs, a lot of them are considered unsafe and should not be used in production. I do think it's worth taking a stab at implementing some of these functions for yourself though.

## Reading from the console

When you run the following code it will accept your name and greet you. Isn't that nice?

```c
#include <stdio.h>

#define BUFFER_CAPACITY 256

int main()
{
  char name [BUFFER_CAPACITY];
  str_clear(name, BUFFER_CAPACITY);
  printf ("Input your name here: ");
  fgets(name, BUFFER_CAPACITY, stdin);
  printf ("Hi %s!\n", name);
  return 0;
}

```

```
Input your name here: Riley
Hi Riley
!
```

Hmm. That's not great. We'll fix the multi line problem in a sec. Let's go over the new stuff though.

The function `fgets` accepts three arguments, a `char` array to write to, the length of the array, and the source to read from. The buffer is in our `string` variable, the length is what we have defined in `BUFFER_SIZE`, 256 in our case, and the source is `stdin`. That just means the standard input for a console application. We can also use this function to read from files, more on that later.

Now let's address our problem. When we input our name and hit enter `fgets` includes the newline after the name as part of the string. That's annoying. We want to set that character to be the null terminator instead. We can do that like so:

```c
#include <stdio.h>
#include <string.h>

#define BUFFER_CAPACITY 256

int main()
{
  char name [BUFFER_CAPACITY];
  str_clear(name, BUFFER_CAPACITY);
  printf ("Input your name here: ");
  fgets(name, BUFFER_CAPACITY, stdin);
  size_t len = strlen(name);
  string[len-1] = 0;
  printf ("Hi %s!\n", name);
  return 0;
}

```

Again we're using `string.h` it because it contains the definition for `strlen`. After calling `fgets` we get the length of the string with `strlen`. Then we write a 0, the null terminator, to the position in the buffer exactly one before the end. Then when we print it, no trailing newline!
