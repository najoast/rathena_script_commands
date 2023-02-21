### guild_has_permission
```
*guild_has_permission(<permission>{,<char id>})
```

Checks if the attached player or the player with the given character id has the given permission(s).
Permission can be a bitmask and allows to use multiple values at the same time.
Returns true if the player has all of the given permissions or false if the player does at least
miss one of the given permissions or is not in a guild at all.

Available permissions are:
* `GUILD_PERM_INVITE`	If a player is allowed to invite other players.
* `GUILD_PERM_EXPEL`	If a player is allowed to expel other guild members.
* `GUILD_PERM_STORAGE`	If a player is allowed to access the guild storage.
* `GUILD_PERM_ALL`		A combination of all permissions above.
