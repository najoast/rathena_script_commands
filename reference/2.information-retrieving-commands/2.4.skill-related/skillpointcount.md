### skillpointcount
```
*skillpointcount({<char_id>})
```

Returns the total amount of skill points a character possesses (SkillPoint+SP's used in skills)
This command can be used to check the currently attached characters total amount of skill points.
This means the skill points used in skill are counted, and added to SkillPoints (number of skill points not used).
This command does not count skills which are set as flag 4 (permament granted) (ALL_BUYING_STORE/ALL_INCCARRY)

Example 1:
```c
.@skillPoints = skillpointcount();
mes "You have " + .@skillPoints + " skill points in total!";
```

Example 2:
```c
if (skillpointcount() > 20)
    mes "Wow, you have more then 20 Skill Points in total!";
```
