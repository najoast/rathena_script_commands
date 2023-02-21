### getmapflag
```
*getmapflag("<map name>",<flag>{,<type>})
```

This command checks the status of a given mapflag and returns the mapflag's state.
0 means OFF, and 1 means ON. See 'setmapflag' for a list of mapflags.

For MF_RESTRICTED, the zone value of the map is returned.

The optional parameter 'type' is used in the 'skill_damage' mapflag:
 SKILLDMG_MAX: if mapflag is set (default)
 SKILLDMG_PC: damage against players
 SKILLDMG_MOB: damage against mobs
 SKILLDMG_BOSS: damage against bosses
 SKILLDMG_OTHER: damage against other
 SKILLDMG_CASTER: caster type
