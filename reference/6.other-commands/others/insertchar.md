### insertchar
```
*insertchar(<string>,<char>,<index>)
```

Returns the original string with the specified char inserted at the specified
index. If index is out of range, the char will be inserted on the end of the
string that it is closest. Only the 1st char in the <char> parameter will be used.

Example:
```c
insertchar("laughter", "s", 0); //returns "slaughter"
```
