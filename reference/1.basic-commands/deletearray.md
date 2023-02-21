### deletearray
```
*deletearray <array name>[<first value>],<how much to delete>;
```

This command will delete a specified number of array elements totally from an
array, shifting all the elements beyond this towards the beginning.

```c
// This will delete array element 0, and move all the other array elements
// up one place.
deletearray .@array[0],1

// This would delete array elements numbered 1, 2 and 3, leave element 0 in its
// place, and move the other elements ups, so there are no gaps.
deletearray .@array[1],3
```