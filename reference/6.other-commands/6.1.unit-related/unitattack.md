### unitattack
```
*unitattack <GID>,<Target ID>{,<action type>};
```
### unitattack
```
*unitattack <GID>,"<Target Name>"{,<action type>};
```

This command will make a `<GID>` attack the specified target. It returns true upon
success and false for all failures.

If `<GID>` is a player and a non-zero `<action type>` is given, the unit will perform a
continuous attack instead of a single attack.

Note:
Using unitattack with `<GID>` 0 means that it will use the currently attached unit.
For players any attack requests will fail, because talking to an NPC prevents attacking a monster.
Therefore you need to detach the player from the NPC before using this command.
