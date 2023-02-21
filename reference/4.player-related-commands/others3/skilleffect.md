
### skilleffect
```
*skilleffect <skill id>,<number>;
*skilleffect "<skill name>",<number>;
```

This command displays visual and aural effects of given skill on currently
attached character. The number parameter is for skill whose visual effect
involves displaying of a number (healing or damaging). Note, that this command
will not actually use the skill, it is intended for scripts, which simulate
skill usage by the NPC, such as buffs, by setting appropriate status and
displaying the skill's effect.

```c
mes "Be blessed!";
// Heal of 2000 HP
heal 2000,0;
skilleffect 28,2000;
// Blessing Level 10
sc_start SC_BLESSING,240000,10;
skilleffect 34,0;
// Increase AGI Level 5
sc_start SC_INCREASEAGI,140000,5;
skilleffect 29,0;
```

This will heal the character with 2000 HP, buff it with Blessing Lv 10 and
Increase AGI Lv 5, and display appropriate effects.
