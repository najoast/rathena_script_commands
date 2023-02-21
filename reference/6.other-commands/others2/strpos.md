### strpos
```
*strpos(<haystack>,<needle>{,<offset>})
```

PHP style strpos. Finds a substring (needle) within a string (haystack).
The offset parameter indicates the index of the string to start searching.
Returns index of substring on successful search, else -1.
Comparison is case sensitive.

Example:
```c
strpos("foobar", "bar", 0); //returns 3
strpos("foobarfoo", "foo", 0); //returns 0
strpos("foobarfoo", "foo", 1); //returns 6
```
