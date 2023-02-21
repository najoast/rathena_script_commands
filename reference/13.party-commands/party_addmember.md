### party_addmember
```
*party_addmember(<party id>,<character id>);
```

Adds a player to an existing party.

The command returns 1 upon success, and these values upon failure:
 0: Unknown error.
-1: Player not found.
-2: Player already has a party.
-3: Party not found.
-4: Party is full.
-5: Another character from the same account is already in the party.
