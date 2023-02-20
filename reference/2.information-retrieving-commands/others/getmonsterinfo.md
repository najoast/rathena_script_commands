### getmonsterinfo
```
*getmonsterinfo(<mob ID>,<type>)
```

This function will look up the monster with the specified ID number in the
mob database and return the info set by TYPE argument.
It will return -1 if there is no such monster (or the type value is invalid),
or "null" if you requested the monster's name.

Valid types are:
* MOB_NAME - monster's name, if there is no such monster "null" is returned
* MOB_LV - monster's level
* MOB_MAXHP - monster's maximum hp
* MOB_BASEEXP - monster's base experience
* MOB_JOBEXP - monster's job experience
* MOB_ATK1 - monster's atk
* MOB_ATK2 - monster's atk2
* MOB_DEF - monster's def
* MOB_MDEF - monster's mdef
* MOB_STR - monster's str
* MOB_AGI - monster's agi
* MOB_VIT - monster's vit
* MOB_INT - monster's int
* MOB_DEX - monster's dex
* MOB_LUK - monster's luk
* MOB_RANGE - monster's range
* MOB_RANGE2 - monster's range2
* MOB_RANGE3 - monster's range3
* MOB_SIZE - monster's size
* MOB_RACE - monster's race
* MOB_ELEMENT - monster's element(doesn't return the element level, only the element ID)
* MOB_MODE - monster's mode
* MOB_MVPEXP - monster's mvp experience

For more details, see the sample in 'doc/sample/getmonsterinfo.txt'.
