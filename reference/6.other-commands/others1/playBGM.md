### playBGM
```
*playBGM "<BGM filename>";
```
### playBGMall
```
*playBGMall "<BGM filename>"{,"<map name>"{,<x0>,<y0>,<x1>,<y1>}};
```

These two commands will play a Background Music to either the invoking character
only ('playBGM') or multiple characters ('playBGMall').

BGM filename is the filename in /BGM/ folder. It has to be in .mp3 extension.

It's not required to specify the extension inside the script.
If coordinates are omitted, BGM will be broadcasted on the entire map. If the map name
is omitted as well the BGM will be played for the entire server.

You can add your own BGMs this way, naturally.
