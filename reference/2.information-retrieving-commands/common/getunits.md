
# getunits
```
*getunits(<type>{,<array_variable>[<first value>]})
```

The 'getunits' command will return the number of `<type>` objects active on the server.


Type is the type of object to search for:

```
	BL_PC   - Character objects
	BL_MOB  - Monster objects
	BL_PET  - Pet objects
	BL_HOM  - Homunculus objects
	BL_MER  - Mercenary objects
	BL_NPC  - NPC objects
	BL_ELEM - Elemental objects
```

If `<array_variable>` is provided:
* An int variable will return the list of GID.
* A string variable will return the list of names.

Example 1:
```c
// getting the players count and building a string array of the names.
.@num = getunits(BL_PC,.@array$[0]);

mes "the number of Users Connected to the server is " + .@num + " .";
mes "list of Players names :";
freeloop(1);	// for if the list was too big.
for(.@i=0;.@i<getarraysize(.@array$);.@i++)
    mes (.@i + 1) + " " + .@array$[.@i];
freeloop(0);
end;
```

Example 2:
```c
// getting the npc count in Prontera and building a string array of the names.
.@num = getmapunits(BL_NPC,"prontera",.@array$[0]);

mes "the number of NPCs in Prontera is " + .@num + " .";
mes "list of NPCs name :";
freeloop(1);	// for if the list was too big.
for(.@i=0;.@i<getarraysize(.@array$);.@i++)
    mes (.@i + 1) + " " + .@array$[.@i];
freeloop(0);
end;
```

Example 3:
```c
// getting the monster count in Prontera with specific coordinates and building a int array of the GIDs.
.@num = getareaunits(BL_MOB,"prontera",154,186,159,182,.@array[0]);

mes "the number of Monsters in Prontera in that Coordinates is " + .@num + " .";
mes "list of Monsters GID :";
freeloop(1);	// for if the list was too big.
for(.@i=0;.@i<getarraysize(.@array);.@i++)
    mes (.@i + 1) + " " + .@array[.@i];
freeloop(0);
end;
```
