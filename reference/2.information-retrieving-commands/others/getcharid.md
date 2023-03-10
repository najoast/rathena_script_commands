### getcharid
```
*getcharid(<type>{,"<character name>"})
```

This function will return a unique ID number of the invoking character, or, if a
character name is specified, of that player.

Type is the kind of associated ID number required:

* 0 - Character ID
* 1 - Party ID
* 2 - Guild ID
* 3 - Account ID
* 4 - Battle Ground ID
* 5 - Clan ID

For most purposes other than printing it, a number is better to have than a name
(people do horrifying things to their character names).

If the character is not in a party or not in a guild, the function will return 0
if guild or party number is requested. If a name is specified and the character
is not found, 0 is returned.

If getcharid(0) returns a zero, the script got called not by a character and
doesn't have an attached RID. Note that this will cause the map server to
print "player not attached!" error messages, so it is preferred to use
"playerattached" to check for the character attached to the script.

```c
if (getcharid(2) == 0)
	mes "Only members of a guild are allowed here!";
```
