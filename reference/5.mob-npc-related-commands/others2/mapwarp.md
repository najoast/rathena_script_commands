### mapwarp
```
*mapwarp "<from map>","<to map>",<x>,<y>{,<type>,<ID>};
```

This command will collect all characters located on the From map and warp them
wholesale to the same point on the To map, or randomly distribute them there if
the coordinates are zero. "Random" is understood as a special To map name and
will mean randomly shuffling everyone on the same map.

Optionally, a type and ID can be specified. Available types are:

* 0 - Everyone
* 1 - Guild
* 2 - Party

Example:

```c
// Will warp all members of guild with ID 63 on map prontera to map alberta.
mapwarp "prontera","alberta",150,150,1,63;
```
