### getequipweaponlv
```
*getequipweaponlv(<equipment slot>{,<char_id>})
```

This function returns the weapon level for the weapon equipped in the specified
equipment slot on the invoking character. For a list of equipment slots see
'getequipid'.

Only EQI_HAND_L and EQI_HAND_R normally make sense, since only weapons have
a weapon level. You can, however, probably, use this field for other equippable
custom items as a flag or something.

If no item is equipped in this slot, or if it doesn't have a weapon level
according to the database, 0 will be returned.

Examples:
```c
// Right hand can only contain a weapon.
switch (getequipweaponlv(EQI_HAND_R)) {
    case 1: mes "You are holding a lvl 1 weapon."; break;
    case 2: mes "You are holding a lvl 2 weapon."; break;
    case 3: mes "You are holding a lvl 3 weapon."; break;
    case 4: mes "You are holding a lvl 4 weapon."; break;
    case 5: mes "You are holding a lvl 5 weapon, hm, must be a custom design..."; break;
    default: mes "Seems you don't have a weapon on."; break;
}

// Left hand can hold either a weapon or shield.
if (getequipid(EQI_HAND_R) == 0) {
    mes "Seems you have nothing equipped here.";
    close;
}
switch (getequipweaponlv(EQI_HAND_L)) {
    case 0: mes "You are holding a shield, so it doesn't have a level."; break;
    case 1: mes "You are holding a lvl 1 weapon."; break;
    case 2: mes "You are holding a lvl 2 weapon."; break;
    case 3: mes "You are holding a lvl 3 weapon."; break;
    case 4: mes "You are holding a lvl 4 weapon."; break;
    case 5: mes "You are holding a lvl 5 weapon, hm, must be a custom design..."; break;
}
```
