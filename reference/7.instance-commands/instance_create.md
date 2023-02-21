### instance_create
```
*instance_create("<instance name>"{,<instance mode>{,<owner id>}});
```

Creates an instance for the `<owner id>` of `<mode>`. The instance name, along with
all other instance data, is read from 'db/(pre-)re/instance_db.txt'. Upon success,
the command generates a unique instance ID, duplicates all listed maps and NPCs,
sets the alive time, and triggers the "OnInstanceInit" label in all NPCs inside
the instance.

Instance Mode options:
* IM_NONE: Attached to no one.
* IM_CHAR: Attached to a single character.
* IM_PARTY: Attached to a party (default instance mode).
* IM_GUILD: Attached to a guild.
* IM_CLAN: Attached to a clan.

The command returns the instance ID upon success, and these values upon failure:
* -1: Invalid type.
* -2: Character/Party/Guild/Clan not found.
* -3: Instance already exists.
* -4: No free instances (MAX_INSTANCE exceeded).
