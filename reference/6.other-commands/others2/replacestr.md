### replacestr
```
*replacestr(<input>, <search>, <replace>{, <usecase>{, <count>}})
```

Replaces all instances of a search string in the input with the specified
replacement string. By default is case sensitive unless `<usecase>` is set
to 0. If specified it will only replace as many instances as specified
in the count parameter.

Example:
```c
replacestr("testing tester", "test", "dash"); //returns "dashing dasher"
replacestr("Donkey", "don", "mon", 0); //returns "monkey"
replacestr("test test test test test", "test", "yay", 0, 3); //returns "yay yay yay test test"
```
