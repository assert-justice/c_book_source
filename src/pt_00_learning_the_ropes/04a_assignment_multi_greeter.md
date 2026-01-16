# Assignment: Multi Greeter

This assignment is to write a program that asks for a user's name and prints "Hi [name]!" to the console as before. Unlike the last time this program will then ask for another name. It will keep asking for another name until we pass it the string "quit" at which point the program terminates gracefully. Hopefully no one is named "quit". It seems unlikely but stranger things have happened.

Example session:

```
Hello! I am the greeter. I greet anyone who inputs their name.
Input your name here: Ted
Hello Ted!
Input your name here: Sally
Hello Sally!
Input your name here: quit
Goodbye for now!
```

## Notes

You will need to execute some code an arbitrary number of times. A `while` loop will be perfect!

You need to be able to compare two strings for equality. Either write your own function to do this, or figure out how to use the delightfully terse `strncmp` function from the standard library.

## Bonus Challenge

Keep track of whether you have seen a name before and if a name is entered more than once say "Hello again [name]!". For example:

```
Hello! I am the greeter. I greet anyone who inputs their name.
Input your name here: Ted
Hello Ted!
Input your name here: Sally
Hello Sally!
Input your name here: Ted
Hello again Ted!
Input your name here: quit
Goodbye for now!
```

You'll need to keep around a longer buffer. It's probably easiest to use two buffers, one for the current name and one for all the names you have seen. You should warn the user if this buffer is exceeded. Think about how you can be efficient with a fixed size buffer of, say, 256 characters. Happy coding!

```
Hello! I am the greeter. I greet anyone who inputs their name.
Input your name here: Ted
Hello Ted!
Input your name here: Sally
Hello Sally!
Input your name here: Ted
Hello again Ted!
[many names later]
Input your name here: John
Insufficient room for a new name. Sorry John :/
Goodbye for now!
```
