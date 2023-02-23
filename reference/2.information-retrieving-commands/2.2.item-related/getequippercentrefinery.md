### getequippercentrefinery
```
*getequippercentrefinery(<equipment slot>{,<enriched>,<char_id>})
```

This function calculates and returns the percent value chance to successfully
refine the item found in the specified equipment slot of the invoking character
by +1. There is no actual formula, the success rate for a given weapon level of
a certain refine level is found in the `db/(pre-)re/refine_db.yml` file. For a list of
equipment slots see 'getequipid'.

If enriched parameter is set to true, chance to successfully refine the item with
enriched material is returned instead.

These values can be displayed for the player to see, or used to calculate the
random change of a refine succeeding or failing and then going through with it
(which is what the official NPC refinery scripts use it for)

```c
// This will find a random number from 0 - 99 and if that is equal to or more
// than the value recovered by this command it will go to L_Fail
if (getequippercentrefinery(EQI_HAND_L)<=rand(100))
    goto L_Fail;
```
