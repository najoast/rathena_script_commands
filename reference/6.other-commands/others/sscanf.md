### sscanf
```
*sscanf(<string>,<format>[,param[,param[,...]]])
```

C style sscanf. All C format specifiers are supported.
More info: sscanf @ www.cplusplus.com. The number of params is only limited
by rA's script engine.

Example:
```c
sscanf("This is a test: 42 foobar", "This is a test: %d %s", .@num, .@str$);
dispbottom(.@num + " " + .@str$); //prints "42 foobar"
```
