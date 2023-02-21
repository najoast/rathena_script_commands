### warpportal
```
*warpportal <source x>,<source y>,"<map name>",<target x>,<target y>;
```

Creates a warp portal identical to the Acolyte "Warp Portal" skill.
The source coordinates specify the portal's location on the map of the invoking NPC.
The target map and coordinates determine the destination of the portal.

Examples:

```c
// Will create a warp portal on the NPC's map at 150,150 leading to prontera, coords 150,180.
warpportal 150,150,"prontera",150,180;
```
