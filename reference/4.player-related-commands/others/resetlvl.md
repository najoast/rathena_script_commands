### resetlvl
```
*resetlvl <action type>{,<char_id>};
```

This is a character reset command, meant mostly for rebirth script supporting
Advanced jobs, which will reset the invoking character's stats and level
depending on the action type given. Valid action types are:

```
 1 - Base level 1, Job level 1, 0 skill points, 0 base exp, 0 job exp, wipes the
     status effects (only the ones settable by 'setoption'), sets all stats to 1.
     If the new job is 'Novice High', give 100 status points, give First Aid and
     Play Dead skills.
 2 - Base level 1, Job level 1, 0 skill points, 0 base exp, 0 job exp.
     Skills and attribute values are not altered.
 3 - Base level 1, base exp 0. Nothing else is changed.
 4 - Job level 1, job exp 0. Nothing else is changed.
```

In all cases everything the character has on will be unequipped.

Even though it doesn't return a value, it is used as a function in the official
rebirth scripts. Ask AppleGirl why.
