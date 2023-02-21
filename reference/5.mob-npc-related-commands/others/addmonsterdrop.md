### addmonsterdrop
```
*addmonsterdrop <monster id>,<item id>,<rate>;
*addmonsterdrop "<monster name>",<item id>,<rate>;
```
### delmonsterdrop
```
*delmonsterdrop <monster id>,<item id>;
*delmonsterdrop "<monster name>",<item id>;
```

These commands will temporarily add or delete a monster drop, which will be reset
when the mob database reloads or the server shuts down. They return 1 upon success.

If the monster already drops the specified item, its drop rate will be updated with
the given rate (100 = 1%).

Examples:
```c
// Makes Owl Baron drop Honey at an 80% rate.
addmonsterdrop 1295,518,8000;

// Deletes Executioner's Mitten from Rybio.
delmonsterdrop 1201,7017;
```
