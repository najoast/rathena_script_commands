### getgmlevel
```
*getgmlevel({<char_id>})
```

This function will return the (GM) level associated with the player group to which
the invoking character belongs. If this is somehow executed from a console command,
99 will be returned, and 0 will be returned if the account has no GM level.

This allows you to make NPC's only accessible for certain GM levels, or behave
specially when talked to by GMs.

```c
if (getgmlevel())
    mes "What is your command, your godhood?";
```
