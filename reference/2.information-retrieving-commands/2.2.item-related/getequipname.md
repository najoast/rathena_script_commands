### getequipname
```
*getequipname(<equipment slot>{,<char_id>})
```

Returns the jname of the item equipped in the specified equipment slot on the
invoking character, or an empty string if nothing is equipped in that position.
Does the same thing as `getitemname(getequipid())`. Useful for an NPC to state
what your are wearing, or maybe saving as a string variable.
See `getequipid` for a full list of valid equipment slots.

```c
if ( getequipname(EQI_HEAD_TOP) != "" )
    mes "So you are wearing a " + getequipname(EQI_HEAD_TOP) + " on your head";
else
    mes "You are not wearing any head gear";
```
