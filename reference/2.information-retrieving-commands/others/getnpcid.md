### getnpcid
```c
*getnpcid(<type>{,"<npc name>"});
```

Retrieves IDs of the currently invoked NPC. If a unique npc name is
given, IDs of that NPC are retrieved instead. Type specifies what ID
to retrieve and can be one of the following:
* 0 - NPC Game ID

If an invalid type is given or the NPC does not exist, 0 is returned.
