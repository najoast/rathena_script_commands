### delwaitingroom
```
*delwaitingroom {"<NPC object name"};
```

This command will delete a waiting room. If no parameter is given, it will
delete a waiting room attached to the NPC object running this command, if it is,
it will delete a waiting room owned by another NPC object. This is the only way
to get rid of a waiting room, nothing else will cause it to disappear.

It's not clear what happens to a waiting room if the NPC is disabled with
'disablenpc', by the way.
