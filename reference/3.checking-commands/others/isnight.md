### isnight
```
*isnight()
*isday()
```

These functions will return 1 or 0 depending on whether the server is in night
mode or day mode. 'isnight' returns 1 if it's night and 0 if it isn't, 'isday'
the other way around. They can be used interchangeably, pick the one you like
more:

```c
// These two are equivalent:
if (isday())
    mes "I only prowl in the night.";
if (isnight() != 1)
    mes "I only prowl in the night.";
```
