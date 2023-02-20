### getelementofarray
```c
*getelementofarray(<array name>,<index>)
```

This command retrieves the value of the element of given array at given index.
This is equivalent to using:

```
<array name>[<index>]
```

The reason for this is, that this short form is internally converted into a call
to getelementofarray, when the script is loaded.

Also useful when passing arrays to functions or accessing another npc's arrays:

```c
getelementofarray(getarg(0),<index>)
getelementofarray(getvariableofnpc(.var, "testNPC"),<index>)
```
