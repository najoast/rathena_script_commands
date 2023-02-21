### guardianinfo
```
*guardianinfo("<map name>", <guardian number>, <type>);
```

This function will return various info about the specified guardian, or -1
if it fails for some reason. It is primarily used in the castle manager NPC.

Map name and guardian number (value between 0 and 7) define the target.
Type indicates what information to return:
* 0 - visibility (whether the guardian is installed or not)
* 1 - max. hp
* 2 - current hp
