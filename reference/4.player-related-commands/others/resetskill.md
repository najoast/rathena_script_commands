### resetskill
```
*resetskill({<char_id>});
```

This command takes off all the skill points on the invoking character, so they
only have Basic Skill blanked out (lvl 0) left, and returns the points for them
to spend again. Nothing else will change but the skills. Quest skills will also
reset if 'quest_skill_reset' option is set to Yes in 'battle_athena.conf'. If
the 'quest_skill_learn' option is set in there, the points in the quest skills
will also count towards the total.

Used in reset NPC's (duh!)
