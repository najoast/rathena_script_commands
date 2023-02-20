### getequiprefinerycnt
```
*getequiprefinerycnt(<equipment slot>{,<char_id>})
```

Returns the current number of pluses for the item in the specified equipment
slot. For a list of equipment slots see 'getequipid'.

Can be used to check if you have reached a maximum refine value, default for
this is +10:

```c
	if (getequiprefinerycnt(EQI_HEAD_TOP) < 10)
		mes "I will now upgrade your " + getequipname(EQI_HEAD_TOP);
	else
		mes "Sorry, it's not possible to refine hats better than +10";
	close;
```
