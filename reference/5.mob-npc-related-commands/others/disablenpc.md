### disablenpc
```
*disablenpc "<NPC object name>";
```
### enablenpc
```
*enablenpc "<NPC object name>";
```

These two commands will disable and enable, respectively, an NPC object
specified by name. The disabled NPC will disappear from sight and will no longer
be triggerable in the normal way. It is not clear whether it will still be
accessible through 'donpcevent' and other triggering commands, but it probably
will be. You can disable even warp NPCs if you know their object names, which is
an easy way to make a map only accessible through walking half the time. Then
you 'enablenpc' them back.

You can also use these commands to create the illusion of an NPC switching
between several locations, which is often better than actually moving the NPC -
create one NPC object with a visible and a hidden part to their name, make a few
copies, and then disable all except one.
