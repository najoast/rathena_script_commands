### guardian
```
*guardian "<map name>",<x>,<y>,"<name to show>",<mob id>{,"<event label>"{,<guardian index>}};
```

This command is roughly equivalent to 'monster', but is meant to be used with
castle guardian monsters and will only work with them. It will set the guardian
characteristics up according to the castle's investment values and otherwise
set the things up that only castle guardians need.

Since trunk r12524:
Returns the id of the mob or 0 if an error occurred.
When 'guardian index' isn't supplied it produces a temporary guardian.
Temporary guardians are not saved with the castle and can't be accessed by guardianinfo.
