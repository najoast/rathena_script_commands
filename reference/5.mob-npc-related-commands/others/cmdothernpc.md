### cmdothernpc
```
*cmdothernpc "<npc name>","<command>";
```

This is simply `donpcevent <npc name>::OnCommand<command>`.
It is an approximation of official server script language's 'cmdothernpc'.

Returns true if the command was executed on the other NPC successfully, false if not.
