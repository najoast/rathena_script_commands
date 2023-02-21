### bg_warp
```
*bg_warp <Battle Group>,"<map name>",<x>,<y>;
```

Similar to the 'warp' command.
Places all members of `<Battle Group>` at the specified map and coordinates.

Example:
```c
//place the battle group one for Tierra Gorge at starting position.
bg_warp $@TierraBG1_id1,"bat_a01",352,342;
end;
```
