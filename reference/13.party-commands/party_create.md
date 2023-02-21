### party_create
```
*party_create("<party name>"{,<character id>{,<item share>,<item share type>}});
```

Organizes a party with the attached or specified character as leader. If
successful, the command returns 1 and sets the global temporary variable
"$@party_create_id" to the ID of the party created.

Additionally, item sharing options can be provided:
 - Item Share: 0-Each Take (default), 1-Party Share
 - Item Share Type: 0-Each Take (default), 1-Even Share

These values are returned upon failure:
 0: Unknown error.
-1: Player not found.
-2: Player already has a party.
-3: Party name exists.
