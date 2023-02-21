### checkquest
```
*checkquest(<ID>{,PLAYTIME|HUNTING{,<char_id>}})
```

If no additional argument supplied, return the state of the quest:
* -1 = Quest not started (not in quest log)
* 0  = Quest has been given, but the state is "inactive"
* 1  = Quest has been given, and the state is "active"
* 2  = Quest completed

If parameter "PLAYTIME" is supplied:
* -1 = Quest not started (not in quest log)
* 0  = the time limit has not yet been reached
* 1  = the time limit has not been reached but the quest is marked as complete
* 2  = the time limit has been reached

If parameter "HUNTING" is supplied:
* -1 = Quest not started (not in quest log)
* 0  = you haven't killed all of the target monsters and the time limit has not been reached.
* 1  = you haven't killed all of the target monsters but the time limit has been reached.
* 2  = you've killed all of the target monsters
