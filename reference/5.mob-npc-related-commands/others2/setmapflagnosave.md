### setmapflagnosave
```
*setmapflagnosave "<map name>","<alternate map name>",<x>,<y>;
```

This command sets the 'nosave' flag for the specified map and also gives an
alternate respawn-upon-relogin point.

It does not make a map impossible to make a save point on as you would normally
think, 'savepoint' will still work. It will, however, make the specified map
kick the reconnecting players off to the alternate map given to the coordinates
specified.
