### getmercinfo
```
*getmercinfo(<type>{,<char id>});
```

Retrieves information about mercenary of the currently attached
character. If char id is given, the information of that character is
retrieved instead. Type specifies what information to retrieve and
can be one of the following:

* 0 - Mercenary ID
* 1 - Mercenary Class
* 2 - Mercenary Name
* 3 - Mercenary faith value for this mercenary's guild, if any
* 4 - Mercenary calls value for this mercenary's guild, if any
* 5 - Mercenary kill count
* 6 - Mercenary remaining life time in msec
* 7 - Mercenary level
* 8 - Mercenary Game ID

If the character does not have a mercenary, the command returns ""
for name and 0 for all other types.
