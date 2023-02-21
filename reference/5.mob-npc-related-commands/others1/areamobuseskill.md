
### areamobuseskill
```
*areamobuseskill "<map name>",<x>,<y>,<range>,<mob id>,<skill id>,<skill level>,<cast time>,<cancelable>,<emotion>,<target type>;
*areamobuseskill "<map name>",<x>,<y>,<range>,<mob id>,"<skill name>",<skill level>,<cast time>,<cancelable>,<emotion>,<target type>;
```

This command will make all monsters of the specified mob ID in the specified
area use the specified skill. Map name, x, and y define the center of the area,
which extending `<range>` cells in each direction (ex: a range of 3 would create
a 7x7 square). The skill can be specified by skill ID or name. `<cast time>` is in
milliseconds (1000 = 1 second), and the rest should be self-explanatory.

`<target type>` can be:
* 0 = self
* 1 = the mob's current target
* 2 = the mob's master
* 3 = random target

Example:

```c
// spawn 1 Shining Plant in the 5x5 area centered on (155,188)
areamonster "prontera",153,186,157,190,"Shining Plant",1083,1;
// make the plant cast level 10 Cold Bolt on a random target
areamobuseskill "prontera",155,188,2,1083,"MG_COLDBOLT",10,3000,1,ET_KEK,3;
```
