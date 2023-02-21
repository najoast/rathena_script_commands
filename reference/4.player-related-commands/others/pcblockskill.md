### pcblockskill
```
*pcblockskill <id>,<option>;
```
### unitblockskill
```
*unitblockskill <id>,<option>;
```

Prevents the given GID from casting skills when the option is 1, and enables
the ID to cast skills again when the option is 0. This command will run for
the attached unit if the given GID is zero.

Examples:
```c
// Prevents the current char from casting skills.
pcblockskill getcharid(3),1;

// Enables the current char to cast skills again.
pcblockskill getcharid(3),0;
```
