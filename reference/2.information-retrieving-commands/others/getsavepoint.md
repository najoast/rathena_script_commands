### getsavepoint
```c
*getsavepoint(<information type>{,<char_id>})
```

This function will return information about the invoking character's save point.
You can use it to let a character swap between several recorded save points.
Available information types are:

* 0 - Map name (a string)
* 1 - X coordinate
* 2 - Y coordinate
