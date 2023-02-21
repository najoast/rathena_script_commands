### countstr
```
*countstr(<input>, <search>{, <usecase>})
```

Counts all instances of a search string in the input. By default is case
sensitive unless `<usecase>` is set to 0.

Example:
```c
countstr("test test test Test", "test"); //returns 3
countstr("cake Cake", "Cake", 0); //returns 2
```
