### getpartyname
```
*getpartyname(<party id>)
```

This function will return the name of a party that has the specified ID number.
If there is no such party ID, "null" will be returned.

Lets say the ID of a party was saved as a global variable:

```c
// This would return the name of the party from the ID stored in a variable
mes "You're in the '" + getpartyname($@var) + "' party, I know!";
```
