### warpparty
```
*warpparty "<to_mapname>",<x>,<y>,<party_id>,{"<from_mapname>",<range x>,<range y>};
```

Warps a party to specified map and coordinate given the party ID, which you can get with
getcharid(1). You can also request another party id given a member's name with getcharid(1,`<player_name>`).

You can use the following "map names" for special warping behavior:
Random:       All party members are randomly warped in their current map (as if they
              all used a fly wing)
SavePointAll: All party members are warped to their respective save point.
SavePoint:    All party members are warped to the save point of the currently
              attached player (will fail if there's no player attached).
Leader:       All party members are warped to the leader's position. The leader must
              be online and in the current map-server for this to work.

If you specify a from_mapname, 'warpparty' will only affect those on that map.

The `<range x>` and `<range y>` optional values allow for a randomization with the
player's warp point. The values will randomly add or subtract from the given `<x>`
and `<y>` coordinates.

Example:
```c
mes "[Party Warper]";
mes "Here you go!";
close2;
.@party_id = getcharid(1);
warpparty "prontera",150,100,.@party_id;
close;
```
