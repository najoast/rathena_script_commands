### instance_check_guild
```
*instance_check_guild(<guild id>{,<amount>{,<min>{,<max>}}})
```

This function checks if a guild meets certain requirements, returning 1 if all
conditions are met and 0 otherwise. It will only check online characters.
```
amount - number of online guild members (default is 1).
min    - minimum level of all characters in the guild (default is 1).
max    - maximum level of all characters in the guild (default is max level in conf).
```
Example:
```c
if (instance_check_guild(getcharid(2),2,2,149)) {
	mes "Your guild meets the Memorial Dungeon requirements.",
	mes "All online members are between levels 1-150 and at least two are online.";
	close;
} else {
	mes "Sorry, your guild does not meet requirements.";
	close;
}
```