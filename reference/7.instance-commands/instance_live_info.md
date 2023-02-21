### instance_live_info
```
*instance_live_info(<info type>{,<instance id>});
```

Returns the specified `<info type>` of instance attached to the npc or, if
an instance ID is specified, of that instance.

Valid `<info type>`:
```
ILI_NAME	- Instance Name
			  Return the name of the instance or "" if that fails.
ILI_MODE	- Instance Mode
			  Return IM_NONE, IM_CHAR, IM_PARTY, IM_GUILD, IM_CLAN or -1 if that fails.
ILI_OWNER	- Owner ID
			  Return an ID according to the instance mode of the instance attached/specified or -1 if that fails.
			  When the instance mode is IM_NONE, ILI_OWNER will return the npc ID that created the instance,
			  IM_CHAR	- the owner char ID
			  IM_PARTY	- the party ID
			  IM_GUILD	- the guild ID
			  IM_CLAN	- the clan ID
```

Examples:
```c
// Return the instance name of the instance attached to the npc.
.@instance_name$ = instance_live_info(ILI_NAME);

// Return the guild owner ID of the given instance ID.
.@owner = instance_live_info(ILI_OWNER, instance_id(IM_GUILD));
```
