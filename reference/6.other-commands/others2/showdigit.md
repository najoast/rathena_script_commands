### showdigit
```
*showdigit <value>{,<type>};
```

Displays given numeric 'value' in large digital clock font on top of
the screen. The optional parameter 'type' specifies visual aspects
of the "clock" and can be one of the following values:

```
0 - Displays the value for 5 seconds (default).
1 - Incremental counter (1 tick/second).
2 - Decremental counter (1 tick/second). Does not stop at zero,
    but overflows.
3 - Decremental counter (2 ticks/second). Two digits only, stops
    at zero.
```

Except for type 3 the value is interpreted as seconds and formatted
as time in days, hours, minutes and seconds. Note, that the official
script command does not have the optional parameter.

```c
// displays 23:59:59 for 5 seconds
showdigit 86399;

// counter that starts at 60 and runs for 30 seconds
showdigit 60,3;
```
