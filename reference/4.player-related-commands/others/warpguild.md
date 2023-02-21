### warpguild
```
*warpguild "<map name>",<x>,<y>,<guild_id>;
```

Warps a guild to specified map and coordinate given the guild id, which you can get with
getcharid(2). You can also request another guild id given the member's name with getcharid(2,<player_name>).

```
You can use the following "map names" for special warping behavior:
Random:       All guild members are randomly warped in their current map (as if they
              all used a fly wing)
SavePointAll: All guild members are warped to their respective save point.
SavePoint:    All guild members are warped to the save point of the currently
              attached player (will fail if there's no player attached).
```

Example:

```c
warpguild "prontera",x,y,Guild_ID;
```
