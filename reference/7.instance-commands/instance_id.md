### instance_id
```
*instance_id({<instance mode>})
```

Returns the unique instance ID of the given mode. By default it returns the
attached script instance. If `<instance mode>` is provided then the instance
of the currently attached player is used. If that fails, the function will return 0.

Instance Mode options:
* IM_CHAR:	Attached to character.
* IM_PARTY:	Attached to character's party.
* IM_GUILD:	Attached to character's guild.
* IM_CLAN:	Attached to character's clan.
