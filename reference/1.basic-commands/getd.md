### getd
```
*getd("<variable name>")
```

Returns a reference to a variable, the name can be constructed dynamically.
Refer to 'setd' for usage.

This can also be used to set an array dynamically:
```c
setarray getd(".array[0]"), 1, 2, 3, 4, 5;
```

Examples:

```c
set getd("$varRefence"), 1;
set .@i, getd("$" + "pikachu");
```
