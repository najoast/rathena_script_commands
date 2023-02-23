### getarraysize
```
*getarraysize(<array name>)
```

This function returns highest index of the array that is filled.
Notice that zeros and empty strings at the end of this array are not
counted towards this number.

For example:

```c
setarray .@array[0], 100, 200, 300, 400, 500, 600;
set .@arraysize,getarraysize(.@array);
```

This will make .@arraysize == 6. But if you try this:

```c
setarray .@array[0], 100, 200, 300, 400, 500, 600, 0;
set .@arraysize,getarraysize(.@array);
```

`.@arraysize` will still equal 6, even though you've set 7 values.
