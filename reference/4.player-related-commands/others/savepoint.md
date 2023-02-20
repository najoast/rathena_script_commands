### savepoint
```
*savepoint "<map name>",<x>,<y>{,{<range x>,<range y>,}<char_id>};
*save "<map name>",<x>,<y>{,{<range x>,<range y>,}<char_id>};
```

These commands save where the invoking character will return to upon clicking
"Return to Save Point", after death and in some other cases. The two versions are
equivalent. They ignore any and all mapflags, and can make a character respawn where
no teleportation is otherwise possible.

The `<range x>` and `<range y>` optional values allow for a randomization with the
player's save point. The values will randomly add or subtract from the given `<x>`
and `<y>` coordinates.

```c
savepoint "place",350,75;
savepoint "place",350,75,2,2; // Randomly save the character between 348,73 and 352,77
```
