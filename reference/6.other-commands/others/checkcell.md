### checkcell
```
*checkcell ("<map name>",<x>,<y>,<type>);
```

This command will return 1 or 0, depending on whether the specified cell has
the 'type' flag set or not. There are various types to check, all mimicking
the server's cell_chk enumeration. The types can be found in 'src/map/script_constants.hpp'.

The meaning of the individual types can be confusing, so here's an overview:
```
  - cell_chkwall/water/cliff
    these check directly for the 'terrain component' of the specified cell
  - cell_chkpass/reach/nopass/noreach
    passable = not wall & not cliff, reachable = passable wrt. no-stacking mod
  - cell_chknpc/basilica/landprotector/novending/nochat
    these check for specific dynamic flags (their name indicates what they do)
```

Example:
```c
mes "Pick a destination map.";
input .@map$;
mes "Alright, now give me the coordinates.";
input .@x;
input .@y;
if ( !checkcell(.@map$,.@x,.@y,cell_chkpass) ) {
    mes "Can't warp you there, sorry!";
    close;
} else {
    mes "Ok, get ready...";
    close2;
    warp .@map$, .@x, .@y;
    end;
}
```
