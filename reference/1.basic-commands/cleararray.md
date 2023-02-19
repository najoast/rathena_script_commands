
### cleararray
```
*cleararray <array name>[<first value to alter>],<value>,<number of values to set>;
```

This command will change many array values at the same time to the same value.

```
    setarray .@array[0], 100, 200, 300, 400, 500, 600;
    // This will make all 6 values 0
    cleararray .@array[0],0,6;
    // This will make array element 0 change to 245
    cleararray .@array[0],245,1;
    // This will make elements 1 and 2 change to 345
    cleararray .@array[1],345,2;
```

See [setarray](reference/1.basic-commands/setarray.md).
