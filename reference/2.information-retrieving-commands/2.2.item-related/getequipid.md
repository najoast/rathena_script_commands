
### getequipid
```
*getequipid({<equipment slot>,<char_id>})
```

This function returns the item ID of the item slot that calls the script
on the invoking character or the specified equipment slot. If nothing is
equipped there, it returns -1.
Valid equipment slots are:

```
EQI_COMPOUND_ON (-1)      - Item slot that calls this script (In context of item script)
EQI_ACC_L (0)             - Accessory 1
EQI_ACC_R (1)             - Accessory 2
EQI_SHOES (2)             - Footgear (shoes, boots)
EQI_GARMENT (3)           - Garment (mufflers, hoods, manteaux)
EQI_HEAD_LOW (4)          - Lower Headgear (beards, some masks)
EQI_HEAD_MID (5)          - Middle Headgear (masks, glasses)
EQI_HEAD_TOP (6)          - Upper Headgear
EQI_ARMOR (7)             - Armor (jackets, robes)
EQI_HAND_L (8)            - Left hand (weapons, shields)
EQI_HAND_R (9)            - Right hand (weapons)
EQI_COSTUME_HEAD_TOP (10) - Upper Costume Headgear
EQI_COSTUME_HEAD_MID (11) - Middle Costume Headgear
EQI_COSTUME_HEAD_LOW (12) - Lower Costume Headgear
EQI_COSTUME_GARMENT (13)  - Costume Garment
EQI_AMMO (14)    		  - Arrow/Ammunition
EQI_SHADOW_ARMOR (15)     - Shadow Armor
EQI_SHADOW_WEAPON (16)    - Shadow Weapon
EQI_SHADOW_SHIELD (17)    - Shadow Shield
EQI_SHADOW_SHOES (18)     - Shadow Shoes
EQI_SHADOW_ACC_R (19)     - Shadow Accessory 2
EQI_SHADOW_ACC_L (20)     - Shadow Accessory 1
```

Notice that a few items occupy several equipment slots, and if the character is
wearing such an item, 'getequipid' will return its ID number for either slot.

Can be used to check if you have something equipped, or if you haven't got
something equipped:
```c
if (getequipid(EQI_HEAD_TOP) == 2234)
    mes "What a lovely Tiara you have on";
else
    mes "Come back when you have a Tiara on";
close;
```

You can also use it to make sure people don't pass a point before removing an
item totally from them. Let's say you don't want people to wear Legion Plate
armor, but also don't want them to equip if after the check, you would do this:
```c
if (getequipid(EQI_ARMOR) == 2341 || getequipid(EQI_ARMOR) == 2342) {
    mes "You are wearing some Legion Plate Armor, please drop that in your stash before continuing";
    close;
}
// the || is used as an or argument, there is 2341 and 2342 cause there are
// two different legion plate armors, one with a slot one without.

if (countitem(2341) > 0 || countitem(2432) > 0) {
    mes "You have some Legion Plate Armor in your inventory, please drop that in your stash before continuing";
    close;
}
mes "I will lets you pass.";
close2;
warp "place",50,50;
end;
```
