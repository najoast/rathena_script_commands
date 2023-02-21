### unitskilluseid
```
*unitskilluseid <GID>,<skill id>,<skill lvl>{,<target id>,<casttime>};
*unitskilluseid <GID>,"<skill name>",<skill lvl>{,<target id>,<casttime>};
```
### unitskillusepos
```
*unitskillusepos <GID>,<skill id>,<skill lvl>,<x>,<y>{,<casttime>};
*unitskillusepos <GID>,"<skill name>",<skill lvl>,<x>,<y>{,<casttime>};
```

This is the replacement of the older commands, these use the same values for
GID as the other unit* commands (See 'GID').

Skill ID is the ID of the skill, skill level is the level of the skill.
Cast time is the amount of seconds to add or remove from the skill. Use a positive value to
add and negative value to subtract. Using 0 or no value will use the default skill cast time.
For the position, the x and y are given in the UnitSkillUsePos.
