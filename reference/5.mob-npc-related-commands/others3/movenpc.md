### movenpc
```
*movenpc "<NPC name>",<x>,<y>{,<dir>};
```

This command looks like the NPCWalkToxy function,but is a little different.

While NPCWalkToXY just makes the NPC 'walk' to the coordinates given (which
sometimes gives problems if the path isn't a straight line without objects),
this command just moves the NPC. It basically warps out and in on the current
and given spot. Direction can be used to change the NPC's facing direction.

Example:

```c
// This will move Bugga from it's old coordinates to the new coordinates at 100,20 (if those coordinates are legit).
moveNPC "Bugga",100,20;
```
