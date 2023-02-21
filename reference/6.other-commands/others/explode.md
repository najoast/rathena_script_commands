### explode
```
*explode(<dest_array>,<string>,<delimiter>)
```

Breaks a string up into substrings based on the specified delimiter. Substrings
will be stored within the specified string array. Only the 1st char of the
delimiter parameter will be used. If an empty string is passed as a delimiter,
the string will be placed in the array in its original form.

Example:
```c
explode(.@my_array$, "Explode:Test:1965:red:PIE", ":");
//.@my_array$ contents will be...
//.@my_array$[0]: "Explode"
//.@my_array$[1]: "Test"
//.@my_array$[2]: "1965"
//.@my_array$[3]: "red"
//.@my_array$[4]: "PIE"
```
