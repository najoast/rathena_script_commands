### getguildmember
```
*getguildmember <guild id>{,<type>{,<array_variable>}};
```

This command will find all members of a specified guild and returns their names
(or character id or account id depending on the value of "type") into an array
of temporary global variables.

Upon executing this,

```
$@guildmembername$[] is a global temporary string array which contains all the
                     names of these guild members.
                     (only set when type is 0 or not specified)

$@guildmembercid[]   is a global temporary number array which contains the
                     character id of these guild members.
                     (only set when type is 1)

$@guildmemberaid[]   is a global temporary number array which contains the
                     account id of these guild members.
                     (only set when type is 2)

$@guildmembercount   is the number of guild members that were found.
```

The guild members will be found regardless of whether they are online or offline.
Note that the names come in no particular order.

Be sure to use $@guildmembercount to go through this array, and not
'getarraysize', because it is not cleared between runs of 'getguildmember'.

If 'array_variable' is set, the result will be stored to that variable instead
using global variable.

For usage examples, see 'getpartymember'.
