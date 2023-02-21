### setwall
```
*setwall "<map name>",<x>,<y>,<size>,<dir>,<shootable>,"<name>";
```
### delwall
```
*delwall "<name>";
```

Creates an invisible wall, an array of "setcell" starting from x,y and doing a
line of the given size in the given direction. The difference with setcell is
this one update client part too to avoid the glitch problem. Directions are the
same as NPC sprite facing directions: 0=north, 1=northwest, 2=west, etc.
