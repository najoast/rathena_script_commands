### clone
```
*clone "<map name>",<x>,<y>,"<event>",<char id>{,<master_id>{,<mode>{,<flag>,<duration>}}}
```

This command creates a monster which is a copy of another player. The first
four arguments serve the same purpose as in the monster script command, The
`<char id>` is the character id of the player to clone (player must be online).
If `<master id>` is given, the clone will be a 'slave/minion' of it. Master_id
must be a character id of another online player.

The mode can be specified to determine the behavior of the clone. Its
values are the same as the ones used for the mode field in the mob_db. The
default mode is aggressive, assists, can move, can attack.

Flag can be either zero or one currently. If zero, the clone is a normal
monster that'll target players, if one, it is considered a summoned monster,
and as such, it'll target other monsters. Defaults to zero.

The duration specifies how long the clone will live before it is auto-removed.
Specified in seconds, defaults to no limit (zero).

Returned value is the monster ID of the spawned clone. If command fails,
returned value is zero.
