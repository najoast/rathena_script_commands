### getmapguildusers
```
*getmapguildusers("<map name>",<guild id>)
```

Returns the amount of characters from the specified guild on the given map.

Example:

```c
mes "You have " + getMapGuildUsers("prontera",getcharid(2)) + " guild members in Prontera.";
```
