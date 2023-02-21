### implode
```
*implode(<string_array>{,<glue>})
```

Combines all substrings within the specified string array into a single string.
If the glue parameter is specified, it will be inserted inbetween each substring.

Example:
```c
setarray .@my_array$[0], "This", "is", "a", "test";
implode(.@my_array$, " "); //returns "This is a test"
```
