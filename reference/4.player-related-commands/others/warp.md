### warp
```
*warp "<map name>",<x>,<y>{,<char id>};
```

This command will take the invoking character or <char id>, if specified, to the specified map, and if
wanted, specified coordinates too, but these can be random.

	warp "place",50,55;

This would take them to X 50 Y 55 on the map called "place". If your X and Y
coordinates land on an unwalkable map square, it will send the warped character
to a random place. Same will happen if they are both zero:

	warp "place",0,0;

Notice that while warping people to coordinates 0,0 will normally get them into
a random place, it's not certain to always be so. Darned if I know where this is
actually coded, it might be that this happens because square 0,0 is unwalkable
on all official maps. If you're using custom maps, beware.

There are also three special 'map names' you can use.

"Random" will warp the player randomly on the current map.
"Save" and "SavePoint" will warp the player back to their save point.
