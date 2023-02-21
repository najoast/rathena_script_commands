### party_changeoption
```
*party_changeoption(<party id>,<option>,<flag>);
```

Changes a party option.

Valid options are:
 0 - Exp Share (flags: 0-Each Take, 1-Even Share)
 1 - Item Share (flags: 0-Each Take, 1-Party Share)
 2 - Item Share Type (flags: 0-Each Take, 1-Even Share)

The command returns 1 upon success, and these values upon failure:
 0: Invalid option.
-1: Party not found.
