# Assignment: Phonebook

This assignment is to write a phonebook cli app. You need to support a few different operations:
- add [name]: [number]: add a name and number to the list of stored names and report that the operation succeeded. If a name is inserted you have seen before, update the phone number and report that. Assume inputs are always formatted correctly. If the name is too long or there is no room in the phonebook report the error but do not exit the program.
- lookup [name]: prints the number for the given name if it exists. if it does not exist report that but do not exit the program.
- clear: erase the contents of the phonebook
- quit: exit the program.
- help: display help text

You haven't learned how to read or write files yet so data can't persist between runs of the program. When you learn how to do that feel free to circle back and give it a shot.

The maximum size of the phonebook and maximum length of the name of each person is up to you.

Example session (assume the size of the phonebook is 4 and the maximum name length is 16):

```
Hello! I am your friendly phonebook app, Phony!
add Riley: (555)000-0000
Added Riley successfully.
add Sally: (777)999-9999
Added Sally successfully.
lookup Sally
Sally: (777)999-9999
add Riley: (555)111-1111
Entry for Riley updated!
lookup Theo
No number for name 'Theo' found.
add Wolfgang Amadeus Mozart: (555)000-1756
Name 'Wolfgang Amadeus Mozart' is too long.
```