### sprintf
```
*sprintf(<format>[,param[,param[,...]]])
```

C style sprintf. The resulting string is returned same as in PHP. All C format
specifiers are supported except %n. More info: sprintf @ www.cplusplus.com.
The number of params is only limited by rA's script engine.

Example:
```c
.@format$ = "The %s contains %d monkeys";
dispbottom(sprintf(.@format$, "zoo", 5));        //prints "The zoo contains 5 monkeys"
dispbottom(sprintf(.@format$, "barrel", 82));    //prints "The barrel contains 82 monkeys"
```
