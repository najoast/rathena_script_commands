### getpartyleader
```
*getpartyleader(<party id>{,<type>})
```

This function returns some information about the given party-id's leader.
When type is omitted, the default information retrieved is the leader's name.
Possible types are:

* 1: Leader account id
* 2: Leader character id
* 3: Leader's class
* 4: Leader's current map name
* 5: Leader's current level as stored on the party structure (may not be current level if leader leveled up recently).

If retrieval fails (leader not found or party does not exist), this function
returns "null" instead of the character name, and -1 for the other types.
