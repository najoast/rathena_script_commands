### hideonnpc
```
*hideonnpc "<NPC object name>";
```
### hideoffnpc
```
*hideoffnpc "<NPC object name>";
```

These commands will make the NPC object specified display as hidden/visible,
even though not actually disabled per se. Hidden as in thief Hide skill, but
unfortunately, not detectable by Ruwach or Sight.

As they are now, these commands are pointless, it is suggested to use
'disablenpc'/'enablenpc', because these two commands actually unload the NPC
sprite location and other accompanying data from memory when it is not used.
However, you can use these for some quest ideas (such as cloaking NPCs talking
while hidden then revealing.... you can wonder around =P
