### pcblockmove
```
*pcblockmove <id>,<option>;
```
### unitblockmove
```
*unitblockmove <id>,<option>;
```

Prevents the given GID from moving when the option is 1, and enables the ID to
move again when the option is 0. This command will run for the attached unit
if the given GID is zero.

Examples:
```c
// Prevents the current char from moving away.
pcblockmove getcharid(3),1;

// Enables the current char to move again.
pcblockmove getcharid(3),0;
```
