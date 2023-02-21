### setchar
```
*setchar(<string>,<char>,<index>)
```

Returns the original string with the char at the specified index set to the
specified char. If index out of range, the original string will be returned.
Only the 1st char in the <char> parameter will be used.

Example:
```c
setchar("Cat", "B", 0); //returns "Bat"
```
