### unitstopwalk
```
*unitstopwalk <GID>{,<flag>};
```

This command will make a `<GID>` stop moving.

Note: If this is called from OnTouch, then the walktimer attached to the unit is
removed from OnTouch which causes this command to not stop the unit from walking.
Suggest to use 'unitblockmove' to forcefully stop the unit with OnTouch.

The `<flag>` value affects how the unit is stopped. The following flags are bitwise
values (can be combined using the pipe operator):
* USW_NONE = Unit will keep walking to their original destination.
* USW_FIXPOS = Issue a fixpos packet afterwards.
* USW_MOVE_ONCE = Force the unit to move one cell if it hasn't yet.
* USW_MOVE_FULL_CELL = Enable moving to the next cell when unit was already half-way there (may cause on-touch/place side-effects, such as a scripted map change).
* USW_FORCE_STOP = Force stop moving.
