### summon
```
*summon "monster name",<monster id>{,<Time Out>{,"event label"}};
```

This command will summon a monster. (see also 'monster') Unlike monsters spawned
with other commands, this one will set up the monster to fight to protect the
invoking character. Monster name and mob id obey the same rules as the one given
at the beginning of this document for permanent monster spawns with the
exceptions mentioned when describing 'monster' command.

The effect for the skill 'Call Homunculus' will be displayed centered on the
invoking character.

Timeout is the time in milliseconds the summon lives, and is set default
to 60000 (1 minute). Note that also the value 0 will set the timer to default,
and it is not possible to create a spawn that lasts forever.
If an event label is given, upon the monster being killed, the event label will
run as if by 'donpcevent'.

Returned value is the game ID of the spawned monster.

```c
// Will summon a dead branch-style monster to fight for the character.
summon "--ja--",-1;
```
