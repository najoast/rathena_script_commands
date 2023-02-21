### party_changeleader
```
*party_changeleader(<party id>,<character id>);
```

Transfers leadership of a party to the specified character.

The command returns 1 upon success, and these values upon failure:
*  0: Unknown error.
* -1: Party not found.
* -2: Player not found.
* -3: Player is not in the party.
* -4: Player is already party leader.
