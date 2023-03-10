### mobcount
```
*mobcount("<map name>","<event label>")
```

This function will count all the monsters on the specified map that have a given
event label and return the number or 0 if it can't find any. Naturally, only
monsters spawned with 'monster' and 'areamonster' script commands can have non-empty
event label.
If you pass this function an empty string for the event label, it will return
the total count of monster without event label, including permanently spawning monsters.
With the dynamic mobs system enabled, where mobs are not kept
in memory for maps with no actual people playing on them, this will return a 0
for any such map.
If the event label is given as "all", all monsters will be counted, regardless of
having any event label attached.

If the map name is given as "this", the map the invoking character is on will
be used. If the map is not found, or the invoker is not a character while the map
is "this", it will return -1.
