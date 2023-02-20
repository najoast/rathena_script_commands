### getmapxy
```
*getmapxy("<variable for map name>",<variable for x>,<variable for y>{,<type>,"<search value>"})
```

This function will locate a character object, NPC object or pet's coordinates
and place their coordinates into the variables specified when calling it. It
will return 0 if the search was successful, and -1 if the parameters given were
not variables or the search was not successful.

Type is the type of object to search for:
```
	BL_PC   - Character object (default)
	BL_NPC  - NPC object
	BL_PET  - Pet object
	BL_HOM  - Homunculus object
	BL_MER  - Mercenary object
	BL_ELEM - Elemental object
```

The search value is optional. If it is not specified, the location of the
invoking character will always be returned for types `BL_PC` and `BL_PET`,
the location of the NPC running this function for type `BL_NPC`.

If a search value is specified, for types `BL_PC` and `BL_NPC`, the
character or NPC with the specified name or GID will be located.

If type is `BL_PET/BL_HOM/BL_MER/BL_ELEM`, the search
will locate the current object of the character who's name/GID is given in the
search value, it will NOT locate the object by name.

Example:
```c
    prontera,164,301,3%TAB%script%TAB%Meh%TAB%730,{
        mes "My name is Meh. I'm here so that Nyah can find me.";
        close;
    }

    prontera,164,299,3%TAB%script%TAB%Nyah%TAB%730,{
        mes "My name is Nyah.";
        mes "I will now search for Meh all across the world!";
        if (getmapxy(.@mapname$, .@mapx, .@mapy, BL_NPC, "Meh") != 0) {
                mes "I can't seem to find Meh anywhere!";
                close;
        }
        mes "And I found him on map " + .@mapname$ + " at X:" + .@mapx + " Y:" + .@mapy + " !";
        close;
   }
```

Notice that NPC objects disabled with 'disablenpc' will still be located.
