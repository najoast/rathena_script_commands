### skill
```
*skill <skill id>,<level>{,<flag>};
*skill "<skill name>",<level>{,<flag>};
*addtoskill <skill id>,<level>{,<flag>};
*addtoskill "<skill name>",<level>{,<flag>};
```

These commands will give the invoking character a specified skill. This is also
used for item scripts.

Level is obvious. Skill id is the ID number of the skill in question as per
'db/(pre-)re/skill_db.txt'. It is not known for certain whether this can be used to give
a character a monster's skill, but you're welcome to try with the numbers given
in 'db/(pre-)re/mob_skill_db.txt'.

Flag is 0 if the skill is given permanently (will get written with the character
data) or 1 if it is temporary (will be lost eventually, this is meant for card
item scripts usage.).  The flag parameter is optional, and defaults to 1 in
'skill' and to 2 in 'addtoskill'.

Flag 2 means that the level parameter is to be interpreted as a stackable
additional bonus to the skill level. If the character did not have that skill
previously, they will now at 0+the level given.

Flag 3 is the same as flag 1 in that it saves to the database.  However, these skills
are ignored when any action is taken that adjusts the skill tree (reset/job change).

Flag constants:
* 0 - SKILL_PERM
* 1 - SKILL_TEMP
* 2 - SKILL_TEMPLEVEL
* 3 - SKILL_PERM_GRANT

```c
// This will permanently give the character Stone Throw (TF_THROWSTONE,152), at
// level 1.
skill 152,1,0;
```
