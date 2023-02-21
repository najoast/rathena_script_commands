### party_delmember
```
*party_delmember({<character id>,<party id>});
```

Removes a player from his/her party. If no player is specified, the command
will run for the invoking player. If that player is the only party member
remaining, the party will be disbanded.

The command returns 1 upon success, and these values upon failure:
*  0: Unknown error.
* -1: Player not found.
* -2: Party not found.
* -3: Player is not in the party.
