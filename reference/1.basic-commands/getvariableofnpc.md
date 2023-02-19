### getvariableofnpc
```
*getvariableofnpc(<variable>,"<npc name>")
```

Returns a reference to a NPC variable (. prefix) from the target NPC.
This can only be used to get . variables.

Examples:

```
//This will return the value of .var, note that this can't be used, since the value isn't caught.
getvariableofnpc(.var,"TargetNPC");

//This will set the .v variable to the value of the TargetNPC's .var variable.
set .v, getvariableofnpc(.var,"TargetNPC");

//This will set the .var variable of TargetNPC to 1.
set getvariableofnpc(.var,"TargetNPC"), 1;
```

Note: even though function objects can have .variables,
getvariableofnpc will not work on them.