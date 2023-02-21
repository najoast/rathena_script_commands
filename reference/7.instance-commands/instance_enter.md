
### instance_enter
```
*instance_enter("<instance name>",{<x>,<y>,<char_id>,<instance id>});
```

Warps the attached player to the specified `<instance id>`. If no ID is specified,
the IM_PARTY instance the invoking player is attached to is used.

The map and coordinates are located in 'db/(pre-)re/instance_db.txt'.

The command returns IE_OK upon success, and these values upon failure:
* IE_NOMEMBER: Party/Guild/Clan not found (for party/guild/clan modes).
* IE_NOINSTANCE: Character/Party/Guild/Clan does not have an instance.
* IE_OTHER: Other errors (invalid instance name, instance doesn't match with character/party/guild/clan).

Put -1 for x and y if want to warp player with default entrance coordinates.
