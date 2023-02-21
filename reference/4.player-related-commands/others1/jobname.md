### jobname
```
*jobname(<job number>)
```

This command retrieves the name of the given job using the map_msg entries 550->655.

```c
mes "[Kid]";
mes "I never thought I'd met a " + jobname(Class) + " here of all places.";
close;
```
