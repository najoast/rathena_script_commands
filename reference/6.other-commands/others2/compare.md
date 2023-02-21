### compare
```
*compare("<string>","<substring>")
```

This command returns 1 or 0 when the substring is in the main string (1) or not (0).
This command is not case sensitive.

Examples:
```c
//dothis; will be executed ('Bloody Murderer' contains 'Blood').
if (compare("Bloody Murderer","Blood"))
    dothis;

//dothat; will not be executed ('Blood butterfly' does not contain 'Bloody').
if (compare("Blood Butterfly","Bloody"))
    dothat;
```
