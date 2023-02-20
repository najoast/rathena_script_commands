### getskilllv
```
*getskilllv(<skill id>)
*getskilllv("<skill name>")
```

This function returns the level of the specified skill that the invoking
character has. If they don't have the skill, 0 will be returned. The full list
of character skills is available in 'db/(pre-)re/skill_db.txt'.

There are two main uses for this function, it can check whether the character
has a skill or not, and it can tell you if the level is high enough.

Example 1:
```c
if (getskilllv(152))
    mes "You have got the skill Throw Stone";
else
    mes "You don't have Throw Stone";
close;
```

Example 2:
```c
if (getskilllv(28) >= 5)
    mes "Your heal lvl is 5 or more";
else if (getskilllv(28) == 10)
    mes "Your heal lvl has been maxed";
else
    mes "You heal skill is below lvl 5";
close;
```
