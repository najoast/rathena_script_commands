### unitwarp
```
*unitwarp <GID>,"<map name>",<x>,<y>;
```

This command will warp a `<GID>` to the specified map and coordinates.

If `<GID>` is zero, the command runs for the unit that invoked the script. This can be
used with "OnTouch" to warp monsters:

```c
OnTouch:
	unitwarp 0,"this",-1,-1;
```
