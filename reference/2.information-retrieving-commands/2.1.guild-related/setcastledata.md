### setcastledata
```
*setcastledata "<map name>",<type of data>,<value>;
```

The 'setcastledata' command will behave identically, but instead of returning
values for the specified types of accessible data, it will alter them and cause
them to be sent to the char-server for storage.

Changing Guild ID or Castle Defense will trigger additional actions, like
recalculating guardians' HP.

see [getcastledata](reference/2.information-retrieving-commands/guild-related/getcastledata.md) for more information about `<type of data>`.
