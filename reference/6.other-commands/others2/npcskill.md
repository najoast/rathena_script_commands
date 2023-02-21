### npcskill
```
*npcskill <skill id>,<skill lvl>,<stat point>,<NPC level>;
*npcskill "<skill name>",<skill lvl>,<stat point>,<NPC level>;
```

This command causes the attached NPC object to cast a skill on the attached
player. The skill will have no cast time or cooldown. The player must be
within the default skill range or the command will fail silently.

The "stat point" parameter temporarily sets all NPC stats to the given value,
and "NPC level" is the temporary level of the NPC (used in some skills).
Neither value can be greater than the max level defined in config, and will
not work properly if the NPC has a mob sprite.

Before using skills, NPCs must have basic stats applied to them depending on the
skill being used: UNPC_ATKMIN, UNPC_ATKMAX, UNPC_MATKMIN, UNPC_MATKMAX, UNPC_STR,
UNPC_AGI, UNPC_VIT, UNPC_INT, UNPC_DEX, UNPC_LUK.
See 'setunitdata' for more information on usage.

```c
// Casts Level 10 Heal on the attached player, calculated with
// all stats 99 and base level 60.
npcskill "AL_HEAL",10,99,60;
```
