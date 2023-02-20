### readparam
```c
*readparam(<parameter number>{,"<character name>"})
*readparam(<parameter number>{,<char_id>})
```

This function will return the specified stat of the invoking character, or, if a
character name or character id is specified, of that player. The stat can either
be a number or parameter name, defined in  `src/map/script_constants.hpp`.

Some example parameters:

StatusPoint, BaseLevel, SkillPoint, Class, Upper, Zeny, Sex, Weight, MaxWeight,
JobLevel, BaseExp, JobExp, NextBaseExp, NextJobExp, Hp, MaxHp, Sp, MaxSp,
BaseJob, Karma, Manner, bVit, bDex, bAgi, bStr, bInt, bLuk

All of these also behave as variables, but don't expect to be able to just 'set'
them - some will not work for various internal reasons.

Example 1:

```c
// Returns how many status points you haven't spent yet.
mes "Unused status points: " + readparam(9);
```

Using this particular information as a function call is not required. Typing this
will return the same result:

```c
mes "Unused status points: " + StatusPoint;
```

Example 2:

You can also use this command to get stat values.

```c
if (readparam(bVit) > 77)
    mes "Only people with over 77 Vit are reading this!";
```
