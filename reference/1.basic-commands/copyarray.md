### copyarray
```
*copyarray <destination array>[<first value>],<source array>[<first value>],<amount of data to copy>;
```

This command lets you quickly shuffle a lot of data between arrays, which is in
some cases invaluable.

```c
setarray .@array[0], 100, 200, 300, 400, 500, 600;
// So we have made .@array[]
copyarray .@array2[0],@array[2],2;

// Now, .@array2[0] will be equal to .@array[2] (300) and
// .@array2[1] will be equal to .@array[3].
```

So using the examples above:
```c
 .@array[0] = 100
 .@array[1] = 200
 .@array[2] = 300
 .@array[3] = 400
 .@array[4] = 500
 .@array[5] = 600

New Array:
 .@array2[0] = 300
 .@array2[1] = 400
 .@array2[2] = 0
 .@array2[3] = 0
```

Notice that .@array[4] and .@array[5] won't be copied to the second array, and it will return a
0.
