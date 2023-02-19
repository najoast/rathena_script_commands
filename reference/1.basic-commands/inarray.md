### inarray
```
*inarray <array name>,<value>;
```

This command returns the index of the first matching value found in the array.
It will return -1 if the value is not found.

```
setarray .@array[0], 100, 200, 300, 400, 500, 600, 100;

inarray(.@array[0], 200);
//return 1 because 200 is in index 1
//another way to say it that .@array[1] == 200

.@index = inarray(.@array[0], 600);
//.@index is now 5 because .@array[5] == 600

inarray(.@array[0], 100);
//while index 6 is also 100, the command will return the first instance it finds
//return 0 because .@array[0] == 100

inarray(.@array[0], 800);
//return -1 because 800 is not an element of the array .@array
```

For more details, see the sample in 'doc/sample/inarray.txt'.
