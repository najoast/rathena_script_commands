### viewpoint
```
*viewpoint <action>,<x>,<y>,<point number>,<color>{,<Char ID>};
```

This command will mark places on the mini map in the client connected to the
invoking character. It uses the normal X and Y coordinates from the main map.
The colors of the marks are defined using a hexadecimal number, same as the ones
used to color text in 'mes' output, but are written as hexadecimal numbers in C.
(They look like `0x<six numbers>`.)

Action is what you want to do with a point, 1 will set it, while 2 will clear
it. 0 will also set it, but automatically removes the point after 15 seconds.
Point number is the number of the point - you can have several. If more than
one point is drawn at the same coordinates, they will cycle, which can be used
to create flashing marks.

```c
// This command will show a mark at coordinates X 30 Y 40, is mark number 1,
// and will be red.
viewpoint 1,30,40,1,0xFF0000;
```

This will create three points:

```c
viewpoint 1,30,40,1,0xFF0000;
viewpoint 1,35,45,2,0xFF0000;
viewpoint 1,40,50,3,0xFF0000;
```

And this is how you remove them:

```c
viewpoint 2,30,40,1,0xFF0000;
viewpoint 2,35,45,2,0xFF0000;
viewpoint 2,40,50,3,0xFF0000;
```

The client determines what it does with the points entirely, the server keeps no
memory of where the points are set whatsoever.
