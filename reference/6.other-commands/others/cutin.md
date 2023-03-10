### cutin
```
*cutin "<filename>",<position>;
```

This command will display a picture, usually an NPC illustration, also called
cutin, for the currently attached client. The position parameter determines the
placement of the illustration and takes following values:

* 0	bottom left corner
* 1	bottom middle
* 2	bottom right corner
* 3	middle of screen in a movable window with an empty title bar
* 4	middle of screen without the window header, but still movable
* 255	clear all displayed cutins

The picture is read from data\texture\유저인터페이스\illust, from both the GRF archive
and data folder, and is required to be a bitmap. The file extension .bmp can be
omitted. Magenta color (#ff00ff) is considered transparent. There is no limit
placed on the size of the illustrations by the client, although loading of large
pictures (about 700x700 and larger) causes the client to freeze shortly (lag).
Typically the size is about 320x480. New illustrations can be added by just
putting the new file into the location above.

The client is able to display only one cutin at the same time and each new one
will cause the old one to disappear. To delete the currently displayed
illustration without displaying a new one, an empty file name and position 255
must be used.

```c
// Displays the Comodo Kafra illustration in lower right corner.
cutin "kafra_07",2;

// Typical way to end a script, which displayed an illustration during a
// dialog with a player.
mes "See you.";
close2;
cutin "",255;
end;
```
