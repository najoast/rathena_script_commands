### strmobinfo
```
*strmobinfo(<type>,<monster id>);
```

This function will return information about a monster record in the database, as
per 'db/(pre-)re/mob_db.txt'. Type is the kind of information returned. Valid types are:
It will return 0 if there is no such monster (or the type value is invalid),
or an empty string if you requested the monster's name.

* 1 - 'english name' field in the database, a string.
* 2 - 'japanese name' field in the database, a string.  All other returned values are numbers:
* 3 - Level.
* 4 - Maximum HP.
* 5 - Maximum SP.
* 6 - Experience reward.
* 7 - Job experience reward.
