### instance_check_party
```
*instance_check_party(<party id>{,<amount>{,<min>{,<max>}}})
```

This function checks if a party meets certain requirements, returning 1 if all
conditions are met and 0 otherwise. It will only check online characters.
```
amount - number of online party members (default is 1).
min    - minimum level of all characters in the party (default is 1).
max    - maximum level of all characters in the party (default is max level in conf).
```
Example:
```c
if (instance_check_party(getcharid(1),2,2,149)) {
	mes "Your party meets the Memorial Dungeon requirements.",
	mes "All online members are between levels 1-150 and at least two are online.";
	close;
} else {
	mes "Sorry, your party does not meet requirements.";
	close;
}
```