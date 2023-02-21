### substr
```
*substr(<string>,<start_index>,<end_index>)
```

Returns the sub-string of the specified string inclusively between the set
indexes. If indexes are out of range, or the start index is after the end
index, an empty string will be returned.

Example:
```c
substr("foobar", 3, 5); //returns "bar"
```
