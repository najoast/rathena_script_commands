### areaannounce
```
*areaannounce "<map name>",<x1>,<y1>,<x2>,<y2>,"<text>",<flag>{,<fontColor>{,<fontType>{,<fontSize>{,<fontAlign>{,<fontY>}}}}}};
```

This command works like 'announce' but will only broadcast to characters
residing in the specified x1/y1-x2/y2 rectangle on the map given. The flags and
optional parameters are the same as in 'announce', but target and source flags are ignored.

```c
areaannounce "prt_church",0,0,350,350,"God's in his heaven, all right with the world",0;
```
