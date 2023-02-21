### getfreecell
```
*getfreecell "<map name>",<rX>,<rY>{,<x>,<y>,<rangeX>,<rangeY>,<flag>};
```

Finds a free cell on the given map and stores the reference to the found cell
in `<rX>` and `<rY>`. Passing `<x>` and `<y>` with `<rangeX>` and `<rangeY>` allows for
searching within a specified area on the given map. The `<flag>` is a bitmask
and has the following possible values:
* 1 = Random cell on the map or from `<x>`,`<y>` range. (default)
* 2 = The target should be able to walk to the target tile.
* 4 = There shouldn't be any players around the target tile (use the no_spawn_on_player setting).

Examples:
```c
getfreecell("prontera",.@x,.@y); // Find a random empty cell in Prontera and store it within .@x and .@y
getfreecell("prontera",.@x,.@y,150,150,5,5); // Find a random empty cell on 150,150 (with a range of 5x5) in Prontera and store it within .@x and .@y
```
